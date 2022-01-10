---
title: 日付入りディレクトリへのデータ保存スクリプト
date: 2016-02-05 15:00:53
category: misc
slug: script-for-keeping-results
authors: Ryo KOBAYASHI
---

tags: research, linux
Linux上での結果を残しておくための簡単なスクリプト例をメモしておく．

いくつかの結果ファイルを日付の入ったディレクトリに保存するスクリプト．

``` bash
#!/bin/bash

now=`date +%y%m%d_%H%M%S`
dname=results_$now
mkdir $dname
cp some result files $dname/
echo some result files
echo "These files are Saved at " $dname
```

引数を受け取って保存するようにしたい場合は，

``` bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "There is nothing to be saved..."
    exit 0
fi
now=`date +%y%m%d_%H%M%S`
dname=results_$now
mkdir $dname
cp $* $dname/
echo $*
echo "These files are saved at " $dname
```
