---
title: "Install numpy on Fujitsu FX100"
Date: 2017-02-02T18:51:18+09:00
Category: misc
Slug: install-numpy-on-fujitsu-fx100
Authors: Ryo KOBAYASHI
tags: 
- "FX100"
- " python"
- " numpy"
- " gcc"
- " gfortran"
---


FX100でも，大きな計算だけでなく大量の小さな計算をバカ並列で行うことが認められて来ているので，FX100計算サーバ上でnumpyを用いたpythonコードを走らせたいというモチベーションがあった．
しかし，numpyは入っていないし，管理者に要望を伝えても，「メンテナンスの時でないとインストールできないし，優先順位は低い」みたいな感じだったので，自分でインストールしなければならなかった．

## 前提

* 名大スパコン **FX100**
* `$ uname -a`とすると，`Linux f13c06n08 2.6.32 #2 SMP Wed Nov 2 17:44:12 JST 2016 s64fx s64fx s64fx GNU/Linux`となる．Sparc64の上でLinuxが動いている環境．
* gccは初めから入っているが，gfortranは存在しない．
* 管理者権限はないし，スパコン側からインターネットに接続できないから，`$ pip install numpy`のような便利コマンドが使えないため，自分で依存関係も含めてソースコードからコンパイルしてインストールしなければならない．
* numpyのインストールにはFortranコンパイラが必要そう．
  

## gcc-4.4.7

最新バージョンではないが，もともとそのスパコンにはこのバージョンのgccがインストールされていたので，このバージョンを選択．

[ここ](http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-4.4.7/)から
`gcc-4.4.7.tar.gz` を落として来て，`fx:src/`にコピー．

```
$ ./configure --prefix=$HOME/local-fx --enable-languages=c,fortran
...
configure: error: cannot guess build type; you must specify one
```
となって，進むことができない．
オプションに`--build=sparc64-linux-gnu`を追加すると，
```
checking for correct version of gmp.h... no
configure: error: Building GCC requires GMP 4.1+ and MPFR 2.3.2+.
Try the --with-gmp and/or --with-mpfr options to specify their locations.
Copies of these libraries' source code can be found at their respective
hosting sites as well as at ftp://gcc.gnu.org/pub/gcc/infrastructure/.
See also http://gcc.gnu.org/install/prerequisites.html for additional info.
If you obtained GMP and/or MPFR from a vendor distribution package, make
sure that you have installed both the libraries and the header files.
They may be located in separate packages.
```
となり，進めない．どうも`GMP`と`MPFR`というライブラリが必要とのこと．

**次のセクションにMPFRとGMPのインストールを記載する．**

GMPとMPFRをインストールしたら，それらのヘッダファイルやライブラリファイルへのパスを明示的に渡してやって`./configure`を行う．
```
$ ./configure --prefix=$HOME/local-fx/ --build=sparc64-linux-gnu --with-gmp-include=$HOME/local-fx/include --with-gmp-lib=$HOME/local-fx/lib --with-mpfr-include=$HOME/local-fx/include --with-mpfr-lib=$HOME/local-fx/lib --enable-languages=c,fortran --disable-multilib
$ make -j24
$ make install
```

* `--disable-multilib`をつけないと，コンパイル途中で，`gnu/stubs-32.h`が見つからないというエラーで止まってしまう．（[ココ](http://qiita.com/liveralmask/items/6ed4a98ebb3bf6b7f707)を参照．） 


### MPFR

* [http://www.mpfr.org](http://www.mpfr.org)
からダウンロードしてきて，解凍して，
```
$ ./configure --prefix=$HOME/local-fx/ --build=sparc64-linux-gnu
...
checking for gmp.h... no
configure: error: gmp.h can't be found, or is unusable.
```
と，結局`GMP`が必要．

次のセクションのGMPをインストール後，下記のようにGMPの`gmp.h`と`libgmp.a`の場所を明示的にconfigureに渡してやれば，configureは通る．
```
$ ./configure --prefix=$HOME/local-fx/ --build=sparc64-linux-gnu --with-gmp-include=$HOME/local-fx/include --with-gmp-lib=$HOME/local-fx/lib
$ make -j12
$ make install
```

### GMP

下記参考
* [http://d.hatena.ne.jp/fuku_dw/20111217/1324127010](http://d.hatena.ne.jp/fuku_dw/20111217/1324127010)

```
$ ./configure --prefix=$HOME/local-fx/ --build=sparc64-linux-gnu
$ make -j4
$ make check
$ make install
```

-----

## numpy

### build

```
$ CC=gcc python setup.py build --fcompiler=gfortran
```

* `CC=gcc`と先に宣言してから`setup.py`を実行しなければエラーで止まってしまう．
* `sparc64-unknown-linux-gnu-gcc`という名前で，コンパイラが存在していないとダメだったので，パスの通っているディレクトリにリンクを貼っておいた．
* BLASやLAPACKなどはマシンにインストールされているものを使えなかった．しかし，インストールはできる．おそらくnumpyの性能はあまりよくないと思われる．

### install

[ここ](https://docs.python.org/3/install/index.html#alternate-installation)を参考にして下記のように`--prefix`にインストール先を指定．
```
$ python setup.py install --prefix=$HOME/local-fx
```
こうしたら，
`$HOME/local-fx/lib64/python2.6/site-packages/`以下にインストールされた．
`lib`ではなく，`lib64`であることに注意．

### check

```
$ export PYTHONPATH=$HOME/local-fx/lib64/python2.6/site-packages:$PYTHONPATH
$ python -c "import numpy; print numpy.__file__"
/home/usr0/z41110v/local-fx/lib64/python2.6/site-packages/numpy/__init__.pyc
```

* まず，`numpy`が置かれているところへ，PYTHONPATHを通してやる．
* `import numpy`が通ることを確認．

## 注意点

* 今回のインストールでは，BLAS, LAPACKを用いずにnumpyをインストールしているので，もしかしたら思い計算は遅くなるかもしれない．
* BLAS, LAPACKをインストールしていないので，scipyのインストールはできなかった．今の所は必要としていないので，これ以上は追求しない．


