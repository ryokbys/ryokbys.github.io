---
title: Constraint atom motion in LAMMPS
date: 2016-03-11 22:26:13
category: misc
slug: constraint-atom-motion-in-lammps
authors: Ryo KOBAYASHI
---

tags: lammps, research
Suppose if one creates a dislocation in a system with two surfaces top
and bottom on z axis and periodic in x and y axes. Motions of atoms at
the top and bottom layer should be limited along x and y direction and
not to move z direction. One can achieve this constraint in LAMMPS by
writing as follows, :

    region top block      0.0  300.0  0.0  20.0  212.643446964  300.0
    region bottom block   0.0  300.0  0.0  20.0  0.0    7.0
    group  topatoms  region top
    group  botatoms  region bottom
    fix  constraint  topatoms  setforce  NULL  NULL  0.0
    fix  constraint  botatoms  setforce  NULL  NULL  0.0
    velocity all set  0.0  0.0  0.0

Here two regions are defined, called `top` and `bottom`. Then groups of
atoms called `topatoms` and `botatoms` are defined, and the forces of z
component are reset to 0.
