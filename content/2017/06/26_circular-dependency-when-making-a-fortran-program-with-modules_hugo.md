---
title: "Circular dependency when making a Fortran program with modules"
Date: 2017-06-26T16:50:42+09:00
Category: misc
Slug: circular-dependency-when-making-a-fortran-program-with-modules
Authors: Ryo KOBAYASHI
tags: 
- "fortran"
- " makefile"
---

以下のようなFortranのプログラムがある．
- いくつかのファイルに分割されている．
- ファイルと同じ名前のモジュールを含むファイルがある．（つまり，`hoge.F90`の中に`module hoge`が宣言されている．）

このような場合，そのモジュールを含む`hoge.F90`を変更した後に`make`してもなぜか`hoge.F90`がコンパイルし直されず，変更が反映されない．
```
$ make -d 
```
のように，`-d`オプションをつけて`make`を実行すると，なぜにそれがコンパイルされないかが分かるのだが，
```
Trying implicit prerequisite `hoge.mod'.
Found an implicit rule for `hoge.o'.
　Considering target file `hoge.mod'.
Prerequisite `hoge.mod' is older than target `hoge.o'.
```

のように，なぜか`.mod`ファイルを見つけてそれが`.o`ファイルより古いから`hoge.o`を更新する必要がないと判断されてしまう．
（`.mod`ファイルはモジュールのヘッダファイルのようなもので，そのモジュールが呼び出される際に必要な情報が入っているが，それらの情報が変更されない限り変更されないファイルである．）
この，`.mod`ファイルの`.o`ファイルへの依存性は，`makefile`内に陽には記述されていないのだが，暗黙のルールとして定義されているようで，
```
.F90.o:
  ${MPIFC} -c $<
```
のように陽に記述されている`.F90`ファイルへの依存性よりも先に判定されてしまい，`.F90`ファイルが更新されてもコンパイルの必要なしと判断されてしまっている．

## 解決策１：ファイル名と同じ名前のモジュール名を使わない

この解決策は確実に上手くいくのだが，なんかバカっぽいし，この問題を忘れた頃にまた同じようにファイル名と同名のモジュールを作成して，同じ問題が生じると思われる．
もっと根本的な解決策が欲しい．


## 解決策２：`.mod`も含めた依存関係を明示する

```
hoge.o hoge.mod : hoge.F90
    $(MPIFC) -c $(MPIFLAGS) $(CPPFLAGS) $<
```
のように，`.o`と`.mod`の２つが`.F90`に依存していると書くことで上記の問題は回避できる．

これですべての場合に対応しているかは不明だが，とりあえず現状の自分の課題は解決される．．．


