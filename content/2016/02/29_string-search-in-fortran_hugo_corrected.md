---
title: String search in Fortran
date: 2016-02-29 15:40:22
category: misc
slug: string-search-in-fortran
authors: Ryo KOBAYASHI
---

tags: fortran
Think of a problem finding the substring `def` in the string `abcdefg`.

I though Fortran does not have an efficient way of doing this and I have
to make own subroutine to achieve this. But Fortran has such a function,
`index`.

See, [INDEX \-- position of a substring within a
string](https://gcc.gnu.org/onlinedocs/gfortran/INDEX-intrinsic.html)

Its syntax is, :

    RESULT = INDEX(STRING, SUBSTRING [, BACK [, KIND]])

and the return value is an integer of the substring position.

The return value starts from **1** if the substring exists. If there is
nothing matched with the substring, the return value becomes **0**.
