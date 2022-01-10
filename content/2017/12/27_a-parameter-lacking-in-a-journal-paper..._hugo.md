---
title: "A parameter lacking in a journal paper..."
Date: 2017-12-27T22:19:29+09:00
Category: misc
Slug: a-parameter-lacking-in-a-journal-paper...
Authors: Ryo KOBAYASHI
tags: 
- "research"
---

There is an erratum in the JAP journal paper (Ref.[1]) by Bonny et al about tungsten-rhenium EAM potential.
The parameters for $V_\mathrm{WRe}$ listed in the paper, cubic spline with 5 knots, were not sufficient and
the following 6th knot should be added.
```
r_6 = 4.2
a_6 = -0.25133241
```
I extracted these numbers by numerical derivative of raw data provided in Ref.[2], so the precision of the data may not be so high.

1. G. Bonny, A. Bakaev, D. Terentyev, and Y.A. Mastrikov, J. Appl. Phys. 121, 165107 (2017).
2. https://www.ctcms.nist.gov/potentials/W-Re.html

