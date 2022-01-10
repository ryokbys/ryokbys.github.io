---
title: "Mayavi on jupyter notebook"
Date: 2018-03-01T11:53:51+09:00
Category: misc
Slug: mayavi-on-jupyter-notebook
Authors: Ryo KOBAYASHI
tags: 
- "notebook"
- " python"
- " mac"
---

**Mayavi** は3Dでのデータ可視化を支援するライブラリ．
これを Jupyter notebook で使えるようにするまでのメモ（macOS High Sierra）．

## References

1. [Jupyter で 3D 表示 - Mayavi ライブラリ](https://qiita.com/tenfu2tea/items/088b750f0204debe4f7f)
2. [Tips and Tricks in Mayavi 4.5.0](http://docs.enthought.com/mayavi/mayavi/tips.html#using-mayavi-in-jupyter-notebooks)
3. [Mayavi on GitHub](https://github.com/enthought/mayavi)


## Mayavi のインストール

Mayavi自体は以下のようにして `pip` でインストールすることができる．
```
$ pip install mayavi
```

しかし，この Mayavi を jupyter notebook で動作させるためには，Qt という表示用ライブラリを Python がら利用するための **PyQt** なるものが必要みたいで，それのインストールは pip からは行うことができなかった．
そのため，Homebrew を用いて，
```
$ brew install qt
$ brew install pyqt
```
として，Qt および PyQt をインストールした．
インストール後は，
```
$ export PYTHONPATH=$PYTHONPATH:/usr/local/Cellar/pyqt
```
とするか，`~/.bashrc` もしくは `~/.bash_profile` に上記を書きこんでそれを反映させるかして，python が PyQt を使えるようにする．

Ref. [2] に従い，
```
$ jupyter nbextension install --py mayavi --user
```
というおまじないもやっておく．

## Github版Mayaviのインストール

pip からは安定版(?)が得られるが，どうもpip版は最新の VTK (8.1.0) との相性が良くないらしく，エラーで何も表示されないことがあったので，それが解消されているGithub版をインストールする方法を載せておく．

1. GithubからZIPファイルをダウンロードしてきて展開．
2. `mayavi-master/`内ディレクトリにて，

        :::bash
        $ python setup.py build
        $ python setup.py install
        or
        $ python setup.py install --prefix=~/local/

    `--prefix`オプションを指定した場合はそれ以下に `mayavi` と `tvtk` がインストールされる．
    `build` を行わないと TVTK が正常に動作しないので注意．



## Jupyter notebook での使用

```python
from mayavi import mlab
mlab.init_notebook()
```
とすることで，notebook 上で Mayavi が使えるようになり，
```python
s = mlab.test_plot3d()
s
```
で，3D表示がされれば成功．


## Issues

1. jupyter notebook では成功したが，jupyter lab では表示されなかった．

2. `init_notebook()` のところで，最初，

        An exception has occurred, use %tb to see the full traceback.
        
        SystemExit: This program needs access to the screen. Please run with a
        Framework build of python, and only when you are logged in
        on the main display of your Mac.

    のようなエラー出力が表示されるが，もう一度実行すると，

        Notebook initialized with x3d backend.

    のようになり，問題なくなる...

