---
layout: post
date: '2015-04-05 16:31'
title: 'How to equilibrate a polymer melt?'
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
---

## How to equilibrate a polymer melt?

__Introduction to polymer physics__


The simplest model to describe polymer chains is the Freely Jointed
Chain (FJC) model [FJC]\_. In this simple model ([randomcoil]\_) a
polymer chain is represented as a succession of monomer units that
interact \$textbf{only}\$ by covalent binding forces, therefore the
potential energy of the polymer is taken to be independent of its shape.
Therefore at thermodynamic equilibrium, all of its shape configurations
are equally likely to occur as the polymer fluctuates in time, according
to the Boltzmann distribution. As a result, the bond length is fixed,
and the internal rotations are completely free [equilibrating]\_

Polymer configuration can be described by setting up the positions of
each monomer: for example \$3N\$ Cartesian coordinates \$XYZ\$ with
respect to the laboratory fixed frame. Since all of the configurations
are random, the end-to-end vector, connecting the first and the last
units of a chain, should have a zero average [average]\_ and a certain
variance.

\$\$\\mathbf{R}(N)=\\sum\_{i=1}\^{i=N}\\mathbf{r}\_i\$\$

Evidently [central\_limit]\_, a long freely jointed chain will obey
Gaussian statistics: the probability to observe a certain end-to-end
vector \$\\mathbf{R}\$ is given by the Gaussian distribution function
with zero average (centered at the origin) and mean squared value.

\$\$\\Big \\langle \\mathbf{R}(N) \\Big \\rangle = 0\$\$

\$\$\\Big \\langle \\mathbf{R\^2}(N)\\Big \\rangle \\equiv \\Big
\\langle R\^2(N)\\Big \\rangle = Nb\^2\$\$ where \$b\$ is the bond
length and \$N\$ is the number of bonds. Another value used in polymer
physics is the radius of gyration \$R\_g\$ which is defined by the:

\$\$R\^2\_g(N) = \\frac{1}{N}
\\Sigma\_{i}(\\mathbf{r}\_i-\\mathbf{r}\_{CM})\^2\$\$

where \$\\mathbf{r\_i}\$ \\- is the position vector of bead number \$i\$
in the chain and \$r\_{CM}\$ is the position vector of the center of
mass of the polymer chain. Schematically it is represented in
[figPolymerParameters]\_. For ideal chains\\index{ideal chains}, the
radius of gyration (see [figPolymerParameters]\_) can also be calculated
by:

\$\$\<R\^2\_g(N)\> = \\frac{\<R\^2(N)\>}{6}\$\$

Despite its simplicity, \$\<R\^2\_g(N)\> = \\frac{\<R\^2(N)\>}{6}\$ is
among the most fundamental results of polymer science - it provides an
estimate of the length scales in polymer melts, as well as serving as a
bridge to connect experimental results to MD.

![](../images/end_to_end_explained.png)

FIG. [figPolymerParameters]\_ Schematic representation of a polymer
chain. Two main characteristic values are shown: the end-to-end distance
which corresponds to the vector connecting the first and the last beads
of the polymer, and the gyration radius which is a characteristic
average size of the polymer chain.

In practice, chains are non-ideal, they interact and have internal
stiffness. Moreover, since the number of beads \$N\$ is usually a large
number, no distinction is made between the finite and the infinite
number of bonds. In 1969 Flory used these ideas to define the
characteristic ratio \$C\_{\\infty}\$ of a polymer as:

\$\$C\_{\\infty} = \\lim\_{N\\to\\infty} \\frac{\<R\^2(N)\>}{Nb\^2}\$\$

By definition, \$C\^{FJC}\_{\\infty} = 1\$. Values of \$C\_{\\infty}\$
larger than \$1\$ occur when some of the degrees of freedom are
constrained. The \$C\_{\\infty}\$ can be used as a measure of stiffness
along the polymer backbone. The value \$C\_{\\infty}\$ is experimentally
observable therefore one can judge the level of equilibration of polymer
melts by how well their Mean Square Internal Distances(\$R\^2(n)/n\$)
plot saturate on the value \$\<R\^2(n \\to \\infty)\> \\to C\_{\\infty}
Nb\^2\$, explained in [figMSID]\_.

![](../images/msid_explained.png)

FIG. [figMSID]\_ Schematic representation of evolution of Mean Square
Internal Distances (MSID) for equilibrated and non-equilibrated polymer
melts. Upon equilibration chain MSID saturates on correct End-to-End
distances that can be directly obtained from number of units in the
chain, bond length and chain stiffness

Theoretical aspects of polymer melt equilibration in MD
-------------------------------------------------------

The MD scheme takes care of the time evolution of the model used to
study a particular system. Still, the first configuration (meaning
positions and velocities (for all particles) have to be defined. For the
case of short inorganic molecules, the initial positions can be set up
”by hand” on the vertices of a perfect crystal. This is not the case for
the long chain high-temperature polymer melt. Therefore, one needs to
start with a configuration that closely resembles an equilibrated,
disordered and amorphous system. This can be achieved using a
Monte-Carlo algorithm that will efficiently decorrelate an artificial
configuration so as to let it acquire equilibrium properties. For
instance, in the present work melts were created using self-avoiding
random walk via a chain.f tool provided by the LAMMPS Molecular Dynamics
Simulator ([lammps]\_) package.

After initial configurations are created, polymer chains need to be
equilibrated. Unlike short molecules, long chain polymers require both
thermodynamic and configurational equilibration. Configurational
equilibration can be achieved when the Mean Square Internal Distance
(\$MSID\$) parameter is equilibrated and correspond to pseudo-Gaussian
chain, see FIG.[figmsid]\_. This was done via Kremer-Grest equilibration
process 7 using bead-springs polymer representation ([sliozberg]\_ )The
method used for configurational equilibration is a fast ’Dpd-push-off’ -
it is a commonly used way to prepare well-equilibrated melts. This
method is an extension of the slow push-off method developed by Auhl et
al. ([auhl]\_). The idea of application of soft repulsive potentials for
equilibration of polymer melts is effective provided the potential is
applied to the initial configurations that closely match equilibrium
structures at large length scales. The details and the practical aspects
of the algorithm will be discussed below. The MSID plots, alongside
plots of the thermodynamic parameters evolution provide evidence on a
fine quality of equilibration achieved using the procedure explained
above.

![](../images/output_20_1.png)

Mean Square Internal Distance Plot (\$MSID\$). The plot indicates well
equilibrated character of polymer melt. Linear growth at the initial
stages is followed by saturation on the

Practical aspects of polymer equilibration
------------------------------------------

We have already discussed the theoretical aspects of polymer
equilibration. We emphasized the importance of creating decorrelated
initial configurtions and tracking the evolution of MSID parameter as a
gauge of equilibration. In this subsection we will discussed the
practical aspects of polymer equilibration.

Now it's time to "make hands dirty" and cover the steps and code used
for equilibration.

> a.  We start with unphysical Kremer-Grest equilibration using
>     bead-spring model. (no angles in the atom topology only
>     atoms+bonds)
> b.  We add the angle/dihedral parts
> c.  Finish up via physical NVE+NPT run

Below is my LAMMPS code I used for the Kremer-Grest equilibration.
Details may be found in papers of [sliozberg]\_ or [auhl]\_. This is the
main part of the equilibration, after this step we expect the chains to
be fully relaxed and have Gaussian chain statistics.

``` {.sourceCode .perl}
# Kremer-Grest model.

units lj
atom_style bond
special_bonds lj/coul 0 1 1
read_data init.data
neighbor 0.4 bin
neigh_modify every 1 delay 1
comm_modify vel yes
bond_style fene
bond_coeff * 30.0 1.5 1.0 1.0
dump            mydump all dcd 50000 equil.dcd
timestep 0.01
thermo 100
thermo_modify norm no
pair_style dpd 1.0 1.0 122347   # very soft pair-potential
pair_coeff * * 25 4.5 1.0

velocity all create 1.0 17786140

# bonds in init.data are unphysicaly close
# fix nve/limit doesn't let the system to explode
# during the equilibration run

fix 1 all nve/limit 0.001
run 500
fix 1 all nve/limit 0.05
run 500
fix 1 all nve/limit 0.1
run 500
unfix 1
fix 1 all nve
run 50000

write_data tmp.restart_dpd.data

pair_coeff * * 50.0 4.5 1.0
velocity all create 1.0 15086120
run 50
pair_coeff * * 100.0 4.5 1.0
velocity all create 1.0 15786120
run 50
#......
run 100
pair_coeff * * 1000.0 4.5 1.0
velocity all create 1.0 15086189
run 100
write_data tmp.restart_dpd1.data

pair_style hybrid/overlay lj/cut 1.122462 dpd/tstat 1.0 1.0 1.122462 122347
pair_modify shift yes
pair_coeff * * lj/cut 1.0 1.0 1.122462
pair_coeff * * dpd/tstat 4.5 1.122462
velocity all create 1.0 1508612013   # this velocity reset is repeated 10 times
run 50
write_data tmp.restart_push.data
velocity all create 1.0 15086125
run 2000000

write_data equil.data
```

The init.data that was created using self-avoiding random walk and
imported at the beginning of the script above didn't have any angle
sections. But the actual simulation that we will run will need this
parameters. So here I present a short SED/AWK script to update the data
file. Interested read may find a good Introductory tutorial on
<https://quickleft.com/blog/command-line-tutorials-sed-awk/>.

``` {.sourceCode .perl}
LAMMPS FENE chain data file

   140  atoms
   134  bonds
   0  angles
   0  dihedrals
   0  impropers
...
```

So the data file didn't have any information about angle, dihedrals in
the systems. Lets now consider possible ways of tackling this problem.

``` {.sourceCode .perl}
#!/bin/bash


# here is the main file for creating a polymer melt
# input : DATA_file_input = initial melt created by chain.f, or the equil.data - equilibrated melt by in.kremer that has only fene Bonds
# output : DATA_file_output = final melt that has angles ( if required dihedrals as well)

#set input and output
datain="equil.data"
dataout="tmp.data"
#get information about number of atoms and number of bonds
STR=$(less $datain | grep atoms)
LIT=$(echo $STR | grep -o [0-9]*)
natoms=$LIT
# calculate number of angles, knowing how many atoms and bonds we have
STRb=$(less $datain | grep bonds)
LITb=$(echo $STRb | grep -o [0-9]*)
nbond=$LITb

let nangle=(2*$nbond-$natoms)
let ndih=0

#write data file in required format
cp $datain  2.info

echo "LAMMPS  data file

$natoms  atoms
$nbond  bonds
$nangle  angles

1  atom types
1  bond types
1  angle types
" > 1.info

sed -i '1,/bond types/d' 2.info
sed -i '1d' 2.info
sed -i '/Velocities/,/Bonds/{//!d}' 2.info
sed -i '/Velocities/d' 2.info

cat *.info > temprary_file
rm *.info

#creating additional files for generating angle bond topology
echo "1  * * *   * * " > angles_by_type.txt

# sed -i '/improper/d' result_without.data
bash gen_all_angles_topo.sh temprary_file  $dataout
rm temprary_file
rm angles_by_type.txt

#replace multiple blanc lines with a single one
sed -i '/^$/N;/^\n$/D' $dataout

sed -i '/dihedral/d' $dataout

sed -i '/improper/d' $dataout

sed -i '/Bond Coeffs/,/Atoms/{//!d}'  $dataout
sed -i '/Bond Coeffs/d'  $dataout
```

After applying this script we get the following data file:

``` {.sourceCode .perl}
LAMMPS  data file

140  atoms
134  bonds
128  angles

1  atom types
1  bond types
1  angle types
...
```
