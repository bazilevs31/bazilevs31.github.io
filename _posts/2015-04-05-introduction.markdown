---
layout: post
date: '2015-04-05 16:31'
title: 'Why do I write this blog?'
author: "Vasilii Triandafilidi"
header-img: "img/prog/coding.jpg"
---


About me
--------

--

Hi, my name is Vasilii Triandafilidi. I am a first year PhD student at
the University of British Columbia. I work under supervision of Prof.
Savvas Hatzikiriakos' lab [lab]\_ and Prof. Rottler [Prof\_Rottler]\_ .
In the past I had a pleasure working in the Prof. Norman's group
[Prof\_Norman]\_ under supervision of Dr. Stegailov.

In my research I focus on investigating properties of matter using
Method of Molecular Dynamics. Molecular Dynamics [md]\_ is a
computational tool that treats matter as a system of interacting
particles.

![](../images/md_explained.png)

Schematic representation of Molecular Dynamics flow

In order to run a Molecular Dynamics Simulation one needs to provide the
initial atomic positions \$\\mathbf{r\_1},\\mathbf{r\_2} ...
\\mathbf{r\_N}\$, the inter-atomic potential \$U(\\mathbf{r})\$ and a
set of constraints also known as an ensemble for integrating the
equations of motion. Then using this information of the atomic
positions, and interacting potential; the Newtons equations of motion
are used to calculate the atomic positions on the next time step. After
simulation is terminated one obtains the final positions
\$\\mathbf{r\^{'}\_1},\\mathbf{r\^{'}\_2} ...\\mathbf{r\^{'}\_N}\$. The
macroscopic parameters can be calculated by integrating and averaging,
e.g temperature can be calculated as a mean square kinetic energy, i.e
velocity of the system.

In particular I am very interested in investigating the process of
polymer crystallization.

![](../images/spagetti.png)

> width
>
> :   300pt
>
> align
>
> :   center
>
Spaghetti representation of polymer chains

![](../images/bidisp_equal1.png)

Bundle like crystallite upon bidisperse polymer crystallization. Short
chains are colored in red, long chains in green

Tools:
------

To do Molecular Dynamics simulation I use open source code [lammps]\_ ,
to analyze trajectories I use a powerful Python based package
[mdanalysis]\_ , to vizualize my results I use [vmd]\_ .

So why do I write this blog?
----------------------------

--

I write this blog mostly because I want to keep track of my progress. I
do also believe in an open-source and reproducible science. I am very
against inventing bicycles over and over again. The best way to avoid it
is to share, to talk to people and hear their feedback.

I chose Python language because of its open-source nature and great
package infrastructure. If you have a cool program you load it, and
other person can install it as easy as

``` {.sourceCode .bash}
pip install packagename
```

I think scientific community (especially those who work in simulation)
have to learn a lot from this approach. Those who got interested in
open-source science may address a great project by Mozilla
[mozillascience]\_. Besides,Python is a beautiful and powerful language.
The range of its capabilities varies from building a static blog
[pelican]\_ to solving a numerical equations. If you haven't used it
yet, I think, you should give it a try.

I spend a lot of time working on my project and I hope the stuff that I
am doing maybe useful for other people as well.

Contact information:
--------------------

email: vasiliy(dot)triandafilidi( at )gmail.com
