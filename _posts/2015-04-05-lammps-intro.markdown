---
layout: post
date: '2015-04-05 16:31'
title: 'How to run a simple simulation in Lammps?'
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
categories: [Research]
tags: research polymer simulation Molecular-Dynamics Lammps
---

:   How to run a simples simulation in Lammps

Running simulations in Lammps. In this section we will create a group of
atoms and run an easy lammps simulation with lj potential.

```bash
# 3d Lennard-Jones melt
units       real
#Angstroms, T kelvins,
```

```bash
source
boundary p p p
#periodic boundary conds

atom_style  atomic
lattice     bcc 3.82
#10 times x , 10 times y , 10 times z #size
region      box block 0 10 0 10 0 10

create_box  1 box
create_atoms    1 box
mass        1 20.0
```

Specifying pair potential that we are using:

```bash
#potential
pair_style  lj/cut 10.5
pair_coeff  1 1 0.11 4.11 10.5
```

```bash
#assigning random velocties to diffrent atoms
velocity all create 273.3 50007878

neighbor  0.3 bin
neigh_modify  every 20 delay 0 check no

#how often we want to dump thermodynamic output
thermo 100

#how often we want to dump coordinates
dump 1 all cfg 50 dump.*.cfg mass type xs ys zs
```

Specifying what type of `fix` we are using:

```bash
fix     1 all nve
run 1000
write_data result.data
```

How to run a simulation from a saved data file as initial coordinates
input file, and dump trajectory for further visualization

```bash
#LAMMPS
units real
boundary p p p
atom_style atomic

pair_style    lj/cut 10.5
# use this pair style , ljcut means that we have 12/6 potential Van-der-Vaals, and 10.5 is its cutoff
read_data result.data         # read data from a saved state
pair_coeff    1 1 0.11 4.11 10.5 # read parameters

#set particles velocity to 0 C
velocity all create 273.3 50007878

# list thermodynamic output every 100 steps
thermo 100

# dump images that would be visualized by the AtomEye program
# They all have cfg format
# 1 is a name of our dump
# all - is that we dump all atoms
# cfg is format
# 50 - is how often we dumping
# name.*.cfg , where * means that when we dump these files they are going to be saved in this format
# step=0 myrun.0.cfg, step=50 myrun.50.cfg and etc
# we are dumping mass, type of the particles and x , y , z coordinates
dump 1 all cfg 50 myrun.*.cfg mass type xs ys zs
#use this fix file
fix        1 all nve
#run for these many steps
run        2000
#write output into result2.data (if we want to simulate it later)
write_data result2.data
# initial - > result.data -> result2.data
```
