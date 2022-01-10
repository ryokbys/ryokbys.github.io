---
title: "clean script"
Date: 2017-07-27T18:35:08+09:00
Category: misc
Slug: clean-script
Authors: Ryo KOBAYASHI
tags: 
- "linux"
---

I made a simple script that removes files specified by a file `.clean` in the current working directory.

The script is like this.
```bash
#!/bin/bash

CLEAN_FILE='.clean'

if [ ! -f ${CLEAN_FILE} ]; then
    echo "  There is no " ${CLEAN_FILE} " file."
    echo "  You write a list of files to be cleaned into " ${CLEAN_FILE} " file."
    echo "  Then this script will remove these files from the current directory."
    exit 1
fi

rm -rf $(cat ${CLEAN_FILE})
```

1. Put this script at `~/bin/` or somewhere the shell can find by looking at `PATH` environment variable.
2. In any directory where there are many files you want to remove often, create the file `.clean` which is something like
```
out.*
*.txt
```
3. Run the above script,
```
$ clean
```
   Then the files specifined by the `.clean` file will be removed.

It is a very simple script, but it is very convenient for my daily routines.
