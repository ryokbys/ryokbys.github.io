---
title: Math font in sphinx-document
date: 2016-08-11 15:53:45
category: misc
slug: math-font-in-sphinx-document
authors: Ryo KOBAYASHI
---

tags: sphinx
SphinxドキュメントをHTML出力する際の数式フォントを変更したい．
ここでは，数式をPNGで出力する **pngmath** が選択されているとする．（
**mathjax** の場合は別の話らしい．）

`conf.py` 内に .. code:: python

> extensions = \[\'sphinx.ext.pngmath\'\]

と書かれていたら， *pngmath* を使う設定になっている．
この場合，sphinxはコンパイル時にlatexを呼び出して数式のpngファイルを作成する．
そのlatexのためのプリアンブル（preamble）を以下のように `conf.py`
に追加すればよい．

``` python
pngmath_latex_preamble = r"""
\usepackage{eulervm}
\usepackage{amsmath,amssymb}
\newcommand{\degree}{$^{\circ}$}
\newcommand{\temp}{${}^\circ$C}
\newcommand{\rmd}{{\rm d}}
\newcommand{\rme}{{\rm e}}
"""
```

そうするとめでたく数式フォントの変更や自作コマンドなんかを使えるようになる．
