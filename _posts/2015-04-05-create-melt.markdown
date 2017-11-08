---
layout: post
date: '2015-04-05 16:31'
title: 'How to create a polymer melt?'
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
tags: research polymer simulation Molecular-Dynamics
---

## How to create a polymer melt?

How to create a data file of a polymer system for Lammps.

Creating data files for polymer systems appears to be a daunting task.
The process of creating a data file includes three stages. One of
possible ways of tackling this problem would be:

1.  Create initial configuration using Monte-Carlo random walk
2.  Run a Lammps simulation for equilibrating melt
3.  Analyze chain confirmation, if not satisfactory go to stage 2.

Creating initial configuration is relatively easy. The Lammps package
comes with an tool called chain.f. Very fast Fortran program for
creating initial configurations.

To create a melt we need to create a file def.chain, which is located in
the tools/ directory.

def.chain:

```perl
Polymer chain definition

0.8442          rhostar
592984          random # seed (8 digits or less)
1               # of sets of chains (blank line + 6 values for each set)
0               molecule tag rule: 0 = by mol, 1 = from 1 end, 2 = from 2 ends

320             number of chains
100             monomers/chain
1               type of monomers (for output into LAMMPS file)
1               type of bonds (for output into LAMMPS file)
0.97            distance between monomers (in reduced units)
1.02            no distance less than this from site i-1 to i+1 (reduced unit)
```

After compiling `chain.f`:

```bash
gfortran chain.f -o chain.out
```

We may create a polymer melt using this command:

```bash
./chain.out < def.chain > init.data
```

Now the init.data looks like:

```perl
LAMMPS FENE chain data file

   140  atoms
   134  bonds
   0  angles
   0  dihedrals
   0  impropers
...
```
