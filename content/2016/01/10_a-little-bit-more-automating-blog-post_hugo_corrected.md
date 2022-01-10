---
title: A little bit more automating blog post
date: 2016-01-10 21:14:58
category: misc
slug: a-little-bit-more-automating-blog-post
authors: Ryo KOBAYASHI
---

tags: pelican, python, mac
以前に，Pelicanでのブログ投稿をある程度自動化するツールを作成することについて書いたが，
それでもまだいろいろと自動化する必要があったから，少しだけ修正．

-   [create_post.py](https://gist.github.com/ryokbys/e5dea8f2476a83ec2758#file-create_post-py)

このファイルをどこかパスの通ったところにおいて， `.bashrc` などに， :

    alias post='create_post.py --basedir="/path/to/pelican/" -a "Author name"'

と書いておけば， :

    $ post "new post title"

とすれば， `/path/to/pelican/content/2016/01/10_new-post-title.rst`
というファイルが作成されて，
自動的にrst形式ファイルを開くエディタがこのファイルを開いてくれる．
編集が終わったら，（ `post` を実行した）ターミナルは `/path/to/pelican/`
へ移動しているので， :

    $ make html

とすれば，ブログが更新される．
