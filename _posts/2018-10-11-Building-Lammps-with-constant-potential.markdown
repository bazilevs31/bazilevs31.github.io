---
layout:     post
title:      "Building Lammps to study electric double layers"
subtitle:   "Using constant potential method to study electric double layers"
date:       2018-10-11
author:     "Vasilii Triandafilidi"
header-img: "img/eng/auto_gears.jpg"
categories: [Research]
tags: [research, programming]
---


Building Lammps with constant potential

0. You have to download the latest lammps from

   `git clone https://github.com/lammps/lammps`

Installing https://github.com/zhenxingwang/lammps-conp

1. Download the link into the lammps main directory

   `git clone https://github.com/zhenxingwang/lammps-conp`

2. Go to Lammps `src` directory and copy the files

   `cp ../lammps-conp/fix_conp.* .`

3. Build and add `blas`, `lapack` libraries to your system's default directories. Follow the instructions here: https://pheiter.wordpress.com/2012/09/04/howto-installing-lapack-and-blas-on-mac-os/

4. To run the constant potential you need to use `coul/long` potential and hence pre-build lammps with the `kspace` package

   `make yes-kspace`

5. Build lammps:

   `make mpi`

   N.B. if you have more than 1 processor on your computer (4 in my case) you can speed up the compilation and linking stages by using:

   `make -j4 mpi`

6. If you see something like this:

   ```bash
   ... region_cone.o region_cylinder.o region_intersect.o region_plane.o region_prism.o region_sphere.o region_union.o replicate.o rerun.o reset_ids.o respa.o run.o set.o special.o thermo.o timer.o universe.o update.o variable.o velocity.o verlet.o write_coeff.o write_data.o write_dump.o write_restart.o -latc     -lblas -llapack   -o ../lmp_mpi

   size ../lmp_mpi

   TEXT	DATA	__OBJC	others	dec	hex

   6344704	258048	0	4298391552	4304994304	100990000

   ```

then you have successsfully compiled lammps with constant potential



6. Add Lammps to your bin directory and add that directory to Path if you haven't already done so:

   `cp lmp_mpi ~/bin/`

   Add this line to your `~/.bashrc` or `~/.bash_profile` file:

   `export PATH="~/bin:$PATH"`

   and 'refresh it' `source ~/.bashrc`

7.  Now you are ready to use it. Just head to the lammps-conp directory and try the built exe file on the system:

   `~/bin/lmp_mpi -in example_input`

   change:

   `thermo 1000` to `thermo 10`

   and comment thermo_style line

    ```perl
   ...
   1 neighbor lists, perpetual/occasional/extra = 1 0 0
     (1) pair lj/cut/coul/long, perpetual
         attributes: half, newton off
         pair build: half/bin/newtoff
         stencil: half/bin/3d/newtoff
         bin: standard
   Setting up Verlet run ...
     Unit style    : real
     Current step  : 0
     Time step     : 1
   Per MPI rank memory allocation (min/avg/max) = 43.43 | 43.43 | 43.43 Mbytes
   Step Temp E_pair E_mol TotEng Press
          0            0    2017824.2    1813.2185    2019637.4     82380863
         10    872.53943    2016614.2    1815.9302    2019551.1     82380973
         20     1009.294    2016162.8    1837.5044      2019297     82314530
         30    960.36591    2016053.3    1751.1329    2019038.2     82358021
         40    877.21297    2015968.1    1661.6829    2018756.8     82319175
         50    818.58353    2015932.8    1501.1969    2018485.7     82349946
   ....
    ```
