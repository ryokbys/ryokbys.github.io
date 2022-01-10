---
title: GNU parallel for easy multiprocessing
date: 2016-05-12 13:45:48
category: misc
slug: gnu-parallel-for-easy-multiprocessing
authors: Ryo KOBAYASHI
---

tags: linux
-   [コマンドを並列に実行するGNU
    parallelがとても便利](http://bicycle1885.hatenablog.com/entry/2014/08/10/143612)
-   [GNU
    Parallelがすごすぎて生きるのがつらい](https://blog.riywo.com/2011/04/19/022802/)
-   [GNU parallel
    使用例](http://w.koshigoe.jp/study/?%5Bsystem%5D+GNU+parallel+%BB%C8%CD%D1%CE%E3)

などで **GNU parallel**
の紹介がされていて，ちょうど自分がやりたかったことにマッチしてうれしい．

# インストール

<http://ftp.gnu.org/gnu/parallel/> から `parallel-latest.tar.bz2`
をダウンロードし，

``` bash
$ bzip2 -d parallel-latest.tar.bz2
$ tar xvf parallel-latest.tar
$ cd parallel-??????
$ ./configure --prefix=$HOME/local/ && make && make install
```

としてインストールした．

# 並列処理

自分の場合，多くのディレクトリ内（ `smpl_*/smd/` 内 ）で `smd`
というプログラムを実行したかった．

``` bash
$ parallel --bar -j 12 "cd {}/smd/; ~/src/nap/pmd/smd > out.smd; cd ../.." ::: smpl_*
```

とすると12プロセス並列で実行してくれる． `--bar`
オプションで進捗状況表示もしてくれるから便利．
