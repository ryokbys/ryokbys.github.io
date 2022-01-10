---
title: Install python on FX100 in nagoya-u
date: 2016-05-17 09:25:57
category: misc
slug: install-python-on-fx100
authors: Ryo KOBAYASHI
---

tags: linux, research
名大スパコン **FX100**
で作業することが多いが，作業に必要なツールがデフォルトでは入っていないので自分でpythonをインストールすることから始めた．

普通にpythonをインストールすると，後々numpyをインストールしようとしたときにhashlibがないことが問題になるようなので，解決する必要がある．

OSに入っているopensslに問題があるらしいので，opensslを取ってきてインストールしようとすると，
OSに入っているperlに問題があるらしいので，perl-5.22.2を取ってきてインストール．

ただし，以下のインストールはログインノードでのpythonおよびnumpyの使用を仮定している．
計算ノードでの実行を考える場合には，マシンが異なるので，別途コンパイルしなければならない．
おそらく，これよりもより多くの問題があるだろうし，ネット情報が少ないだろうから大変になると予想される．

# Install perl-5.22.2

どうもopensslのインストールにperlのバージョンの問題があるらしかったから，最新版perlをインストール．

``` bash
$ ./Configure -des -Dprefix=$HOME/local
$ make -j8 2>&1 | tee log.make
$ make install
```

# Install openssl

[OpenSSL Homepage](https://www.openssl.org)
からソースパッケージをダウンロードしてきて，展開し，以下のようにしてコンパイルおよびインストールする．

``` bash
$ ./config shared --openssldir=$HOME/local/openssl --prefix=$HOME/local/openssl
$ make depend
$ make -j12 2>&1 | tee log.make
$ make install
```

最新のバージョンだと何故かconfigureの結果がだいぶ異なっているらしく，configure後に
`make depend` を要求されない．
しかし，最新バージョンでは後々のpythonコンパイルでいろいろとエラーが出てコンパイルできないので，
`openssl-1.0.1t.tar.gz` をインストールした．

# Install python-2.7.11

上のopensslのインストールが成功すれば，下記のようにしてインストールできる．

``` bash
$ LDFLAGS="-L$HOME/local/openssl/lib" CFLAGS="-I$HOME/local/openssl/include" ./configure --prefix=$HOME/local
$ export LD_LIBRARY_PATH=$HOME/local/openssl/lib:$LD_LIBRARY_PATH
$ make -j12 2>&1 | tee log.make
$ make install
```

makeの前に， `LD_LIBRARY_PATH`
にopensslの共有ライブラリの場所を指定しておかないと， `_hashlib` と
`_ssl` のビルドに失敗する． この際， `LD_LIBRARY_PATH`
の先頭にopensslの共有ライブラリの場所がないといけないのかしらないが，makeの直前に直接exportしないと
`libssl.so.1.0.0` や `libcrypto.so.1.0.0` などのリンクに失敗するようだ．

# Install numpy-1.8.1

``` bash
$ python setup.py build --fcompiler=gfortran
$ python setup.py install
```

とすることでインストールされる． 以下のようにして確認すべし．

``` bash
$ python -c "import numpy; print numpy.__version__"
```

# Install scipy-0.60.0

こちらはnumpyとは異なり， `libblas` や `liblapack`
が存在しないとインストールできないっぽい．
BLASやLAPACKはどこかの説明を読んでインストールする． `~/local/lib/`
内にそれらがインストールされたとしたら，そこへのPATHを `LD_LIBRARY_PATH`
に追加しておく．

``` bash
$ python setup.py build --fcompiler=gfortran
$ python setup.py install
```

以下のようにして確認すべし．

``` bash
$ python -c "import scipy; print scipy.__version__"
```

# Install docopt

``` bash
$ cp docopt ~/local/lib/python-2.7/site-packages/
```

# Install ase

パッケージをダウンロードしてきて，FX100にコピーし，そのディレクトリへのパスを以下のように，
`~/.bashrc.local` にいくつか設定すれば良い．

``` bash
export PYTHONPATH=${PYTHONPATH}:~/src/ase
export PATH=${PATH}:~/src/ase/tools
export LAMMPS_COMMAND=/usr/local/bin/lammps
export VASP_PP_PATH=~/local/vasp/
```
