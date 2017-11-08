---
layout: post
date: '2015-04-08 16:31'
title: 'Lammps, VMD, MDAnalysis, Software, Python'
subtitle: 'Molecular Dynamics lunch box. How to install software on your
    machine?'
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
tags: software programming Lammps MDAnalysis programming VMD
---


## What do we need?

--

In order to do Molecular Dynamics one needs to install essential
software.

We'll need to install several packages in order to be fully ready for
our simulations:

1.  Molecular Dynamics package [lammps]\_
2.  Python installation with all essential packages [anaconda]\_
3.  Molecular Trajectory Analysis package [mdanalysis]\_ , [pizza]\_ ,
    [moltemplate]\_
4.  Tool to visualize your trajectory [vmd]\_

How to install Lammps?
----------------------

--

To install Lammps one may follow one of these
[tutorials](http://lammps.sandia.gov/doc/Section_start.html#start_2_5)

The easiest way to install [lammps]\_ on Ubuntu machines would be :

```bash
sudo add-apt-repository ppa:gladky-anton/lammps
sudo apt-get update
sudo apt-get install lammps-daily

#it builds with FFTW3 and OpenMPI.

lammps-daily -in in.lj

#To get a copy of the current documentation and examples:
#which will download the doc files in /usr/share/doc/lammps-daily-doc/doc and example problems in /usr/share/doc/lammps-doc/examples.
sudo apt-get install lammps-daily-doc
```

On my mac it was very straight-forward as well:

```bash
brew tap homebrew/science
brew install lammps
#brew install lammps --HEAD --with-mpi
```

On Windows one may download a setup file and install it, or install it
using [lammps\_cygwin](http://sjbyrnes.com/LAMMPStutorial.html)

How to install Anaconda Python distribution?
--------------------------------------------

I love Python, i think it is arguably the easiest "heavyweight"
programming language to learn, it has enormous potential for your tasks,
and a wonderful community. For scientific use one may download an
open-source distribution called Anaconda which has everything a
scientist may need. On any \*nix machine one may install it by
downloading .sh file, and running bash Anaconda\*.sh

Installing any other Python packages will be as easy as:
pip install \<name of the package\>

So to install Molecular Dynamics trajectory Analysis program one can
install [mdanalysis]\_, which is a Python based program, with an easy
interface and good community.

To install it one simply needs to:

```bash
pip install MDAnalysis
```

To install [pizza]\_ and [moltemplate]\_ one simply needs to download
the .tar files, extract them and add to the path To do that we need to
add this lines to \~/.bashrc file

```bash
export PATH="$PATH:/path_to_moltempalate/moltemplate/src"
export MOLTEMPLATE_PATH="/path_to_moltempalate/moltemplate/common"
export PYTHONPATH="$PYTHONPATH:/path_to_pizza/pizza-2Jul14/src"
```

Visualizing Molecular Dynamics Trajectory
-----------------------------------------

To visualize MD trajectories, we need to install [vmd]\_. To do that on
Linux systems, we need to go to their website and download a .tar file.

```bash
tar -xzvf vmd.tar.gz
cd vmd/
```

After downloading and untaring the archive it is all gravy: We just need
vim configure, and change home\_bin\_dir=..,\`home\_library\_dir\` to
where we want VMD to be.

On mac systems just needs to download a .dmg file and install it by just
clicking it. Sometimes we also need to specify the location of the
executable:

```bash
vmdappdir='/Applications/VMD 1.9.2.app/Contents'
# vmdappdir='/Applications/VMD1.8.5.app/Contents'
    # (change it to where vmd lies, obviously ;))
alias vmd='"$vmdappdir/Resources/VMD.app/Contents/MacOS/VMD" $\*'
alias textvmd="vmd -dispdev text $\*"
```
