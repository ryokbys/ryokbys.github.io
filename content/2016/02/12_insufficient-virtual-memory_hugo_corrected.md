---
title: insufficient virtual memory
date: 2016-02-12 17:39:52
category: misc
slug: insufficient-virtual-memory
authors: Ryo KOBAYASHI
---

tags: fortran, research
When I run a program on Linux, I got the following error, :

    forrtl: severe (41): insufficient virtual memory

This error seems to be clear. As it says, I seem to require too many
memory. But when I printed the number of memory I was requiring, it
seemed to be not too much, like 1.8GB, for the system having 32GB.

Actually this bug was related to the limit of 4-Byte integer. Since the
maximum number 4-Byte integer can handle is :

    2^31 = 2147483648

And if the number is larger than this, 4-Byte integer cannot express
correct value and shows wrong number, which is called overflow.

So I printed the value in 8-Byte integer by using cast function
`int(n,kind=8)`, it turns out that I was requesting about 40GB!! Now I
understand the above error message is correct.

Debugging is always annoying task\...
