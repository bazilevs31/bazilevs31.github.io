---
layout: post
date: '2015-04-08 16:31'
title: 'You have run a simulation, now what? How to analyze MD trajectories?'
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
categories: [Research]
tags: research polymer simulation Molecular-Dynamics Lammps MDAnalysis
---

You have run a simulation, now what? How to analyze MD trajectories?

This is a ipython notebook which demonstrates how to use MDAnalysis
package, a package that is indispensable in my research.

```python
from MDAnalysis import *
import numpy as np
%matplotlib inline
```

```python
u = Universe("poly.psf", "poly.pdb")
print u
```

```python
u.atoms
```

Now we have imported a universe, which has all of our frames and
information of our system. Accessing it becomes pretty straightforward:

```python
a = u.selectAtoms("all")
```

```python
a.positions
```

Now imagine we want to find something in our selection, lets say center
of mass, vous a la:

```python
a.centerOfMass()
```

MDanalysis allows you to access some other fields rather than just
atoms:

```python
a.bonds
```

```python
b1 = a.bonds.atom1.positions
```

```python
b2 = a.bonds.atom2.positions
```

```python
bonds_vectors = (b2-b1)
```

```python
bonds_vectors
```

This way we can write a function that takes a Universe as an input and
produces a normalized bond\_vector list as an output:

```python
def get_bondlist_coords(u):
    """
    input: Universe
    output: bonds (that are in the domain, normalized)

    generate normalized coordinates of bond vectors
    get universe , return bonds(coordinates)
    generate coor of all bonds(bond = chord i-1 - i+1 ), normalize it
    """
    bonds = u.bonds.atom2.positions - u.bonds.atom1.positions
    # angles = u.angles
    # bonds = angles.atom3.positions - angles.atom1.positions
    # coords = angles.atom2.positions
    norm = np.linalg.norm(bonds,axis=1)
    bonds /= norm[:, None] #the norm vector is a (nx1) and we have to create dummy directions -> (n,3)
    return bonds
```

```python
get_bondlist_coords(u)
```

Interesting project:
--------------------

![image](./images/output_1_0.png)

Lets calculate something more interesting, say Mean Square Internal
Difference parameter for the trajectory. Our script will be able to
consider polydisperse chains as well as monodisperse ones. Imagine we
have a polymer of the size 2N atoms per chain. So by definition :

$$R_{ij} = (r_i - r_j)$$

$$MSID = < \dfrac{R^2_{ij}(i-j)}{|i-j|} >$$

, where :

$$i = 0,..,N//2, j = N-i-1$$

averaging is being done over all chains

```python
import matplotlib.pyplot as plt

def max_chain(u):
    """
    input: MDAnalysis universe
    output: maximum length of all of the chains present(integer)
    """
    maxlen=0
    for res in u.residues:
        reslen=len(res)
        if maxlen<reslen:
            maxlen=reslen
    # print "maxlen = %f" % maxlen
    return  maxlen-1

def save_plot_r2n(n_array, R2_array,psffile,frame,logplot=False):
    """
    input: n_array - array of n's, R2_array - array of R_n, psffile - name of future files, frame, logplot
    saves a frame of r2_n
    """
    # plt.cla()
    plt.ylabel(r'$\mathrm{\frac{R^2(n)}{n}}$')
    if logplot==True:
        print "xlogscale"
        plt.xscale('log')
        plt.xlabel(r'$\mathrm{log(N)}$')
    else:
        print "regular xscale"
        plt.xlabel(r'$\mathrm{N}$')
    plt.title(r'$\mathrm{\frac{R^2(n)}{n} evolution, frame = %s} $' % (frame) )
    plt.plot(n_array,R2_array,'--')
    plt.show()
#     plt.savefig('R2%s_%.5d.png' % (psffile,frame))
    return None

def get_r2n(u,psffile,frame,Noff=1,logplot=False):
    """
    create a list of
    R2_array - array of distances
    n_array - array of number of bonds between atoms
    k_array - array of number of atoms with this bonds
    start looping in residues
    for every residue:
        start from the middle of it
            calculate the closest atom_i - atom_-i
            if it is the first time we have the number of bonds so big, we expand our lists by appending
            else: we just put it to the nth position
    then the last elements of the array will be deleted, since there is not enough statistics for this chains

    """
    R2_array = []
    n_array = []
    k_array = []

    for res in u.residues:
        chainlen = len(res)/2
        for i in range(chainlen)[::-1]:
            ag1, ag2 = res.atoms[i].pos, res.atoms[-i-1].pos
            tmpdiff = ag1-ag2
            r2 = np.dot(tmpdiff,tmpdiff)
            n = (chainlen-i)
            # print n

            # calc n
            if n >= len(R2_array):
                R2_array.append(r2)
                n_array.append(2*n-1)
                k_array.append(1)
            else:
                R2_array[n] += r2
                k_array[n] += 1
                n_array[n] = n*2-1

    R2_array = np.array(R2_array)
    n_array = np.array(n_array)
    k_array = np.array(k_array)
    R2_array /= k_array*n_array
    R2_array = R2_array[:-Noff]
    n_array = n_array[:-Noff]
    save_plot_r2n(n_array, R2_array,psffile,frame,logplot)

    return None

get_r2n(u,"myfile",0,Noff=1,logplot=False)
```
![Static Structure Factor]({{ "/img/res/output_20_1.png" | absolute_url }})
