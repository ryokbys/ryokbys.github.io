---
title: IPython notebook to Pelican blog post
date: 2016-01-13 17:16:54
category: misc
slug: ipython-notebook-to-pelican-blog-post
authors: Ryo KOBAYASHI
---

tags: pelican, python
この前のブログポストはIPython
notebookでの作業をブログに載せたものだが，ちょっと面倒な方法だった．

直接 `.ipynb`
ファイルを参照できればよいのだが，そのような手段はないらしい．
nbviewerというサイトがあるが，そこでは数式が表示されない．
基本的にはnbviewerサイトと同じことだが，以下のように `nbconvert`
を用いて :

    $ ipython nbconvert --to html hoge.ipynb

とすれば，HTMLファイルが生成されるが，数式が表示されなかったりする．

結局，ipython notebookから `.rst` 形式に出力して，Emacsで編集した．

-   改行コードの `^M` が含まれていたため，Emacsで `^M`
    を置換した．（Emacsでは，改行コード `^M` は `C-q C-m`
    で指定できる．）

-   別行だての数式が， :

        .. raw:: latex

    とかになっていたので， :

        .. math::

    に訂正し，微調整した．
