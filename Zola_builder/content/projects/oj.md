+++
title = "Deciding if strictly one shake of an orange juice bottle is enough to mix it for breakfast"
description = "Multiphase CFD simulations with analytical verification of mixing in container under sudden acceleration and deceleration"
weight = 5
date = "2025-03-27"
updated = "2025-04-20T10:54:17"
authors = ["Matheus"]
draft = true


[taxonomies]
tags=["EN", "CFD" , "OpenFoam"]
[extra]
local_image = "/media/oj.png"
link_to = "./oj/"
read_time = true
comment = true
toc = true
tldr = "Using multiphase CFD simulations in OpenFoam for mixing in container under sudden acceleration"
banner = true
archive = false
+++

# Links
- [GitHub repo](https://github.com/ChurroGelato/oj)

# To-Do
- ~~Find out which solver to use~~
- Set up and run sim
- Post-process results
- Verify results with analytical estimations

# Context
This project idea came to me during breakfast [and you can check out in this blog post](/posts/oj-problem) why it is, most likely, a virtual waste of effort and computing power.

However, I have not worked with OpenFoam before and I think that this might be a good idea to get to know the workflow of the software.

The basic idea is to check if it is enough to shake my orange juice bottle *only* once to appropriately mix the pulp to the juice before pouring into the cup.

# Methodology
## Which solver to use?
Naturally, this is a problem of multiphase flow, given that inside the bottle, at the very least we have to consider the presence of air and juice.
I know that OpenFoam is able to deal with multiphase problems. Time for some research.
### Multiphase flow using interFoam
One possible way of simulating this would be by using a 2 phase flow simulation (air and juice inside the bottle). [OpenFoam's interFoam](https://www.openfoam.com/documentation/guides/latest/doc/guide-applications-solvers-multiphase-interFoam.html) could be a relatively easy way of solving that, [there's even a great video by Rose Walker going through interFoam for 2 phase immiscible fluids](https://www.youtube.com/watch?v=wK_0s7DnMRs). The main drawback of this would be that, since we would not be simulating the pulp as it's own phase, there would be no way of determining the answer to the main question I have and we would have to determine implicitly if the results are appropriate.

Maybe a multiphase flow with 3 fluids could be done.

### Multiphase flow using interMixingFoam
One of the best things that can happen is when you need something and you find that something without much effort or hardship. As described in [OpenFoam's documentation](https://www.openfoam.com/documentation/guides/v2012/man/interMixingFoam.html) interMixingFoam is a:

>Solver for three incompressible fluids (two of which are immiscible) using VOF phase-fraction based interface capturing. With optional mesh motion and mesh topology changes including adaptive re-meshing.

Beautiful.

_Yes, I am well aware that air is not incompressible. But, if I could shake a bottle of juice hard enough to have to take the compressibility of air into account, I would probably be a superhero, not an engineer._ 

I'm also aware that the pulp is, technically, also immiscible. But for all intents and purposes, I think we could consider that it mixes with the juice and limit the simulation to maybe 1 second after the shake, since that is roughly when I would be pouring the juice to drink it.

Some additional references that might be helpful at this stage are:
- [SimFlow's interMixingFoam documentation](https://help.sim-flow.com/solvers/inter-mixing-foam)
- [SimFlow's interMixingFoam Tutorial](https://help.sim-flow.com/tutorials/mixing-tank)

## Setting up the Simulation
### Geometry Modeling
Wasn't too difficult. Did it quickly in Fusion 360. Not going to worry too much about it now, just want to run something first, then I’ll make it better.

### Meshing
Found this *very* useful video on [importing Ansys Fluent meshes to OpenFoam](https://www.youtube.com/watch?v=dUme0MUoqbk).