---
title: "Ovito from command line on macOS"
Date: 2017-03-02T11:10:59+09:00
Category: misc
Slug: ovito-from-command-line-on-macos
Authors: Ryo KOBAYASHI
tags: 
- "mac"
---

[Ovito](https://ovito.org) is a visualization tool for atomistic configurations and simulation results.

When I open a file from command line, I had to directly call `ovito` in `/Applications/Ovito.app/Contents/MacOS/`. Since it is cumbersome to type this everytime I open Ovito, firstly I made its link to the directory where my `PATH` environment can find. But unfortunately and I don't know the reason why, it did not work for Ovito.

So I created a script `~/bin/ovito`,
```bash
#!/bin/bash

ovito=/Applications/Ovito.app/Contents/MacOS/ovito

if [ $# -eq 0 ]; then
    echo "You need at least one argument..."
    exit 0
fi

$ovito $@
```
Now I can call `ovito` from command line as,
```bash
$ ovito some-file-to-open
```
