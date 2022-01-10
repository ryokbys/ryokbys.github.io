---
title: Cell relax in LAMMPS
date: 2016-02-10 10:33:01
category: misc
slug: cell-relax-in-lammps
authors: Ryo KOBAYASHI
---

tags: research, lammps
These days I am learning how to use LAMMPS.

To allow the simulation cell to relax, you need to specify `box/relax`
with `fix` command as, :

    fix 1 all box/relax tri 0.0

Here it mentions that the cell is triclinic, which meams x, y, z, yz,
zx, and xy are controlled independently. And final value `0.0` means
pressure to be achieved.

Then you minimize the system as, :

    minimize    1.0e-7 0.001 1000 10000

Here the values indicate etol, ftol, maxiter, and maxeval respectively.

For example, if you use MEAM potential with `meam.library` and
`meam.coeff` files. The LAMMPS input file can be as follows, :

    units metal 
    boundary p p p 
    atom_modify sort 0 0.0 

    read_data data_lammps

    ### interactions 
    pair_style meam 
    pair_coeff * * meam.library Al Mg Si meam.coeff Al Mg Si 

    ### run
    fix 1 all box/relax tri 0.0
    dump dump_all all custom 1 trj_lammps* id type x y z vx vy vz fx fy fz
    thermo_style custom step temp press cpu pxx pyy pzz pxy pxz pyz ke pe etotal vol lx ly lz atoms
    thermo_modify flush yes
    thermo 1
    minimize    1.0e-7 0.001 1000 10000
