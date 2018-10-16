---
layout:     post
title:      "Building Lammps structures using EMC package"
subtitle:   "Creating complex shapes for Lammps"
date:       2018-10-12
author:     "Vasilii Triandafilidi"
header-img: "img/eng/auto_gears.jpg"
tags: [research, programming]
---

# Building structures for Lammps



1. Install EMC by downloading the file and copying it into the lammps directory

   http://montecarlo.sourceforge.net/emc/Download.html

2. Unpack the archive by using:

   `tar -xzvf <name of the file .tgz>`

3. Go into the `<emc directory>`

4. Add two directories to the path:

   ```bash
   export PATH="/Users/bazilevs/packages/lammps/emcv9.4.3/bin:$PATH"
   export PATH="/Users/bazilevs/packages/lammps/emcv9.4.3/scripts:$PATH"
   # format of the
   export PATH="<your path to the main emc directory>/scripts:$PATH"
   ```

   5. Go into the examples  `cd ./examples/setup/chemistry/polymer/random`

6. Lets try to run the sample file:

   ```bash
   # build the emc input file
   ./polymer.esh
   # build the lammps and vmd files
   emc_macos build.emc
   ```

7. After that you get the following:



![Screen Shot 2018-10-11 at 1.47.25 PM](/img/lammps/Screen Shot 2018-10-11 at 1.47.25 PM.png)



```bash
vmd -e dpd.vmd
```





![Screen Shot 2018-10-11 at 1.53.05 PM](/img/lammps/Screen Shot 2018-10-11 at 1.53.05 PM.png)

