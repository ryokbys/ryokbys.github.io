---
title: A script to keep results in Linux
date: 2016-02-24 16:35:12
category: misc
slug: a-script-to-keep-results-in-linux
authors: Ryo KOBAYASHI
---

tags: linux, shellscript
In the [previous
post](https://dl.dropboxusercontent.com/u/7875490/pelican_blog/output/posts/2016/02/05/script-for-keeping-results/index.html),
I wrote about an shellscript that store some data in a directory whose
name includes date info.

Here I modified it to be able to add an identifier after the date info
in the directory name. As shown in the following code, `getopts`
function? is used to receive `-i` option. After analyzing the options,
`shift` line is necessary, otherwise identifier is also counted as an
argument.

``` bash
#!/bin/bash

CMDNAME=`basename $0`
while getopts i: OPT
do
  case $OPT in
    "i" ) flag_i="true"; identifier="$OPTARG" ;;
      * ) echo "Usage: $CMDNAME [-i identifier] FILE [FILE, ...]"
  esac
done
shift `expr $OPTIND - 1`

if [ $# -eq 0 ]; then
    echo "There are no files to be saved..."
    exit 0
fi

now=`date +%y%m%d_%H%M%S`
if [ "$flag_i" = "true" ]; then
    dname=results_${now}_${identifier}
else
    dname=results_${now}
fi
mkdir $dname
cp $* $dname/
echo $*
echo "These files are saved in " $dname
```

If you save this script as `keep_results.sh` somewhere in your system,
you can use this as :

    $ /path/to/keep_results.sh -i identifier file1 file2 file3 ...

to save `file#` files into the directory whose name is like
`results_160225_003814_identifier`.
