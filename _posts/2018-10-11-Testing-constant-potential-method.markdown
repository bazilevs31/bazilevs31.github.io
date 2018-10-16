---
layout:     post
title:      "Running Lammps simulation"
subtitle:   "Using constant potential method to study electric double layers"
date:       2018-10-11
author:     "Vasilii Triandafilidi"
tags: [research, programming]
header-img: "img/eng/auto_gears.jpg"
---

# Running a constant potential simulation and analyzing the results via VMD



1. Running the simulation:

```bash
~/bin/lmp_mpi -in example_input
```

Here lammps outputs the relevant information

```perl
dump 2 all dcd 500 lmp.dcd
```

This is the potential defined in lammps

```perl
#fix [ID] all conp [Nevery] [η] [Molecule-ID 1] [Molecule-ID 2] [Potential 1] [Potential 2] [Method] [Log] [Matrix]
#Potential 1 = -0.5 V
#Potential 2 = 0.5 V
fix e all conp 1 1.979 81 82 -0.5 0.5 inv iter
```

2. Analyzing the potential via VMD

After that lets read the dcd file and analyze the results of the simulation. To do that lets create a `viz.vmd` file:

```tcl
topo readlammpsdata example_datafile full waitfor all
animate read dcd lmp.dcd waitfor all
mol modstyle 0 0 VDW 0.600000 12.000000
```

Then lets open the file:

```bash
vmd -e viz.vmd
```

There we will see this:



![Screen Shot 2018-10-11 at 3.08.20 PM](/img/lammps/Screen Shot 2018-10-11 at 3.08.20 PM.png)



To analyze the potential distribution, we head to the Analysis/Volmap tool where we see this window. We  update the selection, choose coulombmsm, select the molecule 0 and compute for all frames







![Screen Shot 2018-10-11 at 3.08.28 PM](/img/lammps/Screen Shot 2018-10-11 at 3.08.28 PM.png)

This simulation produces a `volmap_out.dx`  file that we will read using Python module gridData (make sure to install it beforehand https://pypi.org/project/GridDataFormats/)

4. Plotting the results

Analyzing results of the simulation



```python
import pandas as pd
import numpy as np
import string
from gridData import Grid
import matplotlib.pyplot as plt
plt.style.use('fivethirtyeight')

def analyze_dx(_f):
    """analyzes dx _file

    Args:
        _f (string): filename with extension of the dx file that we want to analyze

    Returns:
        pandas: pandas dataframe columns=['coord_phi','phiz']
    """
    g = Grid(_f)
    nx, ny, nz = g.grid.shape
    Lx = g.edges[0][0]
    Ly = g.edges[1][0]
    Lzl = g.edges[2][0]
    Lzr = g.edges[2][-1]

    g_all = np.zeros(nz)

    count = 0

    for i in range(nx):
        for j in range(ny):
            g_all += g.grid[i][j]
            count += 1
    r_array = np.linspace(Lzl, Lzr, nz)

    yy = g_all/count

    A = np.vstack([r_array, yy])
    df = pd.DataFrame(A.T, columns=['coord_phi','phiz'])
    return df


filename = 'volmap_out'
df = analyze_dx(filename + '.dx')
plt.plot(df['coord_phi'], df['phiz'],'o--')
plt.xlabel('z')
plt.ylabel(r'$\phi(z)$')
plt.savefig(filename+'.png')
```



This script will produce this plot:

![potential_volmap](/img/lammps/potential_volmap.png)



![volmap_out_1](/img/lammps/volmap_out_1.png)

6. Analyzing the plotted data

The data is very noisy, the terminal values are but it is important to notice that terminal values are of opposite sign and are equal.  After proper equilibration the potential will converge to a proper values of -0.5-0.5

7. Automating the calculation:

```tcl
set a [atomselect top all]
volmap coulombmsm $a -res 1.0 -allframes -combine avg -o volmap_out.dx
volmap coulombmsm $a -res 1.0 -allframes -combine stdev -o volmap_out_stdv.dx

```

```python
filename = 'volmap_out'
df = parse_file_info.analyze_dx(filename + '.dx')
df_std = parse_file_info.analyze_dx(filename + '_stdv.dx')

plt.errorbar(df['coord_phi'], df['phiz'], yerr=df_std['phiz'], fmt='o')
plt.xlabel('z')
plt.ylabel(r'$\phi(z)$')
plt.tight_layout()
plt.savefig(filename+'.png')
```



we are getting this:

![volmap_out](/img/lammps/volmap_out.png)

Now we can see how "un-equilibrated" the simulation is

