---
title: Python utility for creating new post
date: 2016-01-03 21:20:43
category: misc
slug: create_post
authors: Ryo KOBAYASHI
---

tags: python, pelican
Pelicanでブログポストを作るときに，作成するファイル自体をtinkererで :

    $ tinker --post

とした時のように，日付で整理されたディレクトリ内に原稿テンプレートを作成したかったので，pythonスクリプトを作成した．

-   [create_post.py at
    Gist](https://gist.github.com/ryokbys/e5dea8f2476a83ec2758)

Pelicanのルートディレクトリ（ `content`
ディレクトリが存在するディレクトリ）で， :

    $ python /path/to/create_post.py -s create_post 'Utility for creating new post'

のように `slug` と `TITLE` を指定してやると， :

    ./content/2016/01/03_create_post.rst was written.

のようにファイルが作成される．
ファイル中身にはPelican用の次のようなテンプレートが記載されている． :

    :title: Utility for creating new post
    :date: 2016-01-03 21:20:43
    :category: misc
    :slug: create_post
    :authors: None
    :tags: 

これを編集して，そのディレクトリで， :

    $ make html

とすればブログポスト完了．
