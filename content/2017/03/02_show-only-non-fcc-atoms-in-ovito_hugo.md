---
title: "Show only non-FCC atoms in Ovito"
Date: 2017-03-02T11:38:28+09:00
Category: misc
Slug: show-only-non-fcc-atoms-in-ovito
Authors: Ryo KOBAYASHI
tags: 
- "mac"
- " research"
---

In Ovito, there are following modifications to the view,

- Common neighbor analysis (CNA)
- Expression select
- Delete selected particles

So you can use CNA to identify which atoms are FCC-like and which are not. Then you can select FCC atoms using `Expression select` by adding the following expression into the `Boolean expression:` text box,
```
StructureType == 1
```

And by adding `Delete selected particles` modification, you can see only non-FCC atoms in the view.


