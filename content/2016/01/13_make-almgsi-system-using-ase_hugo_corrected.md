---
title: Make AlMgSi-system LAMMPS file using ASE
date: 2016-01-13 16:58:33
category: misc
slug: make-almgsi-system-using-ase
authors: Ryo KOBAYASHI
---

tags: research, alloy, ase, lammps, python
# A bit Math first

To get $y$-at%Mg2Si for the 1.2wt%Mg2Si system, calculate the following
simultaneous equations,

$$\begin{aligned}
\frac{(\text{Mg$_2$Si})_y}{\text{Al}_x +(\text{Mg$_2$Si})_y} &=& 0.012, \\
x + y &=& 1,
\end{aligned}$$

using masses, Al=26,981, Mg=24.305, and Si=28.085. We get
$y\simeq 0.00425$.

# In case of 32000-atom system

If we put (20,20,20) cubic FCC cells including 4 atoms in the simulation
box, the box contains 32,000 atoms. In the 32000-atom system,

$$32000*y \simeq 32000 *0.00425 \sim 136,$$

there are 136 Mg$_2$Si units. So there are about 90 Mg atoms and 45 Si
atoms in the system, and rest of all are Al atoms.

``` python
from ase import Atoms
from ase.lattice.cubic import FaceCenteredCubic
atoms=FaceCenteredCubic(symbol='Al',latticeconstant=4.041,size=(20,20,20))
print atoms.get_number_of_atoms()
```

::: parsed-literal
32000
:::

``` python
from random import random
natm0= atoms.get_number_of_atoms()
n_si= 0
n_mg= 0
for i in range(natm0):
    rnd= random()
    if rnd < 0.00425:
        if rnd < 0.00425/3:
            atoms[i].symbol= 'Si'
            n_si += 1
        else:
            atoms[i].symbol= 'Mg'
            n_mg += 1
print 'number of Mg = ',n_mg
print 'number of Si = ',n_si
```

::: parsed-literal
number of Mg = 102 number of Si = 46
:::

# Introduce vacancy

For solute atoms to move, there must be sufficient amount of vacancies
in the system. Here we introduce vacancies of almost the same amount of
solute atoms.

``` python
n_vac= 0
for i in range(natm0-1,0-1,-1):
    if random() < 0.00425:
        atoms.pop(i)
        n_vac += 1

print 'number of vacancies = ',n_vac
```

::: parsed-literal
number of vacancies = 139
:::

``` python
n_si= 0
n_mg= 0
for i in range(len(atoms)):
    if atoms[i].symbol == 'Si':
        n_si += 1
    elif atoms[i].symbol == 'Mg':
        n_mg += 1
print 'number of Mg = ',n_mg
print 'number of Si = ',n_si
```

::: parsed-literal
number of Mg = 101 number of Si = 46
:::

# Write the atoms to lammps-readable format

In order to write an atomic configuration file for LAMMPS, ase\'s
`write` function does not work. Instead, we have to use
`write_lammps_data` function in `lammpsrun` module.

``` python
from ase.calculators import lammpsrun
```

``` python
fname= 'data.AlMgSi'
lammpsrun.write_lammps_data(fname,atoms)
```

``` python
!head -n15 data.AlMgSi
```

::: parsed-literal
data.AlMgSi (written by ASE)

31861 atoms 3 atom types 0.0 80.820000000 xlo xhi 0.0 80.820000000 ylo
yhi 0.0 80.819999999 zlo zhi

Atoms

> 1 1 0E-9 0E-9 0E-9 2 1 2.020500000 2.020500000 0E-9 3 1 2.020500000
> 0E-9 2.020500000 4 1 0E-9 2.020500000 2.020500000
:::

Note that in this case, the order of chemical species are Al, Mg, and
Si, because in LAMMPS atoms types are assigned according to the
alphabetic order.
