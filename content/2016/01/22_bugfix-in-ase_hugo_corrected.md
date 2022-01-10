---
title: Bugfix in ASE
date: 2016-01-22 10:49:15
category: misc
slug: bugfix-in-ase
authors: Ryo KOBAYASHI
---

tags: research, ase
There was a bug in ASE package.

When I tried to read a `vasprun.xml` file using `ase.io.read` like,

``` python
atoms= read('vasprun.xml', index=0, format='vasp-xml')
```

and the `vasprun.xml` contains some constraints for atoms, I got the
following message,

    Traceback (most recent call last):
      File "/Users/kobayashi/src/fitpot/vasprun2fp.py", line 63, in <module>
        atoms= read('vasprun.xml',index=0,format='vasp-xml')
      File "/Users/kobayashi/src/ase/ase/io/formats.py", line 290, in read
        return next(_iread(filename, slice(index, None), format, **kwargs))
      File "/Users/kobayashi/src/ase/ase/io/formats.py", line 360, in _iread
        for dct in io.read(fd, *args, **kwargs):
      File "/Users/kobayashi/src/ase/ase/io/vasp.py", line 451, in read_vasp_xml
        constraints.append(FixScaled(cell, i, flags))
    UnboundLocalError: local variable 'cell' referenced before assignment

I fixed `cell` to `cell_init` on a line 451 in `ase/io/vasp.py`. Then
now it works fine ;)
