---
title: "Bug in vasprun.xml?"
Date: 2017-02-15T18:25:33+09:00
Category: misc
Slug: bug-in-vasprun.xml
Authors: Ryo KOBAYASHI
tags: 
- "vasp"
- " research"
---

After running a VASP simulation with Li, La, Zr, and O, I found a bug in `vasprun.xml` in which *Zr* is written as *r*.
```
$ grep 'PAW_PBE' vasprun.xml 
    <rc><c>  28</c><c>Li</c><c>      7.01000000</c><c>      1.00000000</c><c>  PAW_PBE Li 17Jan2003                  </c></rc>
    <rc><c>  12</c><c>La</c><c>    138.90000000</c><c>      9.00000000</c><c>  PAW_PBE La_s 06Sep2000                </c></rc>
    <rc><c>   8</c><c>r </c><c>     91.22400000</c><c>     12.00000000</c><c>  PAW_PBE Zr_sv 04Jan2005               </c></rc>
    <rc><c>  48</c><c>O </c><c>     16.00000000</c><c>      6.00000000</c><c>  PAW_PBE O_s 07Sep2000                 </c></rc>
```

So reading the `vasprun.xml` file with using *ASE* module or something like that does not work.

My current solution is just to correct using `sed` command.
```
$ sed -i "" -e 's|<c>r </c>|<c>Zr</c>|' vasprun.xml
```
But this is kind of an annoying bug when one does some automatic parsing of `vasprun.xml`...

