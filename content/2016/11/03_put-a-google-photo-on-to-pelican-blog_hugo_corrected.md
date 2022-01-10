---
title: ペリカンブログにグーグルフォトを載せる
date: 2016-11-03 10:55:38
category: misc
slug: put-a-google-photo-on-to-pelican-blog
authors: Ryo KOBAYASHI
---

tags: pelican
最近，スマホで撮った写真は自動的にグーグルフォトに同期されるようになっていて，そこから写真のURLを取得するだけでブログに写真を載せることができる．

備忘録として記載しておく．

# 画像の貼り付け方（ReSTで共通？）

    .. figure:: http://......
      :target: http://target.url.here
      :align: right
      :width: 400px
      :alt: Alternative text

のようにして貼り付ける．

この `figure`
の右にグーグルフォトで取得した写真のURLを貼り付ければいい．

# グーグルフォトから写真のURLを取得

どうもグーグルフォトの写真を右クリックして「イメージのアドレスをコピー」としただけでは，そのURLは一時的なものもしくはグーグルフォトにログインしていない人には見れないなどの問題があるらしい．
Picasa
web（こちらもgoogleの写真サービス）のアルバムのアクセス設定を，アドレスを知っている人のみに制限しつつ公開する．Picasa
webでグーグルフォトの写真が見れたらその写真を右クリックして写真のURLを取得すればよいらしい．

参考サイト：

-   [Googleフォトの画像URLを取得](http://ameblo.jp/wantan-52/entry-12119213036.html)
-   [How to get direct permanent link to public image on Google
    Photos? - stack
    overflow](http://stackoverflow.com/questions/39508631/how-to-get-direct-permanent-link-to-public-image-on-google-photos)

グーグルフォトサイトへのリンクを加えたい場合は，
グーグルフォトサイトにて写真の上の方にある「共有ボタン」をクリックして，「リンクを取得」をクリックするとサイトへのリンクが得られるので，それを上の
`:target:` の箇所に貼り付ける．

結局，下のようなコードとなり，さらに下のように写真が表示されるはず． :

    .. figure:: https://lh3.googleusercontent.com/la0N8s4s7XBBc2SKwDWJC4Q4yDr96EXQ7F8i2mnyFtpv-LuX_k_RSDv6Vgb9a5rwtqr3W7w0WhOJ4T0=w1440-h900-no
      :align: center
      :width: 600px
      :alt: Running course along Lake Geneva

![](https://lh3.googleusercontent.com/la0N8s4s7XBBc2SKwDWJC4Q4yDr96EXQ7F8i2mnyFtpv-LuX_k_RSDv6Vgb9a5rwtqr3W7w0WhOJ4T0=w1440-h900-no){.align-center
width="600px"}

# まだ問題あり？

どうもグーグルフォトもしくはPicasaから取得した写真のURLはPermanent
linkではないみたいで，いつ変わってしまうか分からない． **Picasa (Google
album archive)からのpermanet
linkの取得さえもうまく行っていない？上記サイトに記載されているやり方でやっていても時が経つと画像が表示されなくなってしまう．．．**
googleが仕様を変更した瞬間にリンクしていた画像は全て見えなくなってしまうわけで，画像をグーグルフォトから取ってきてブログに貼るという方法は便利だが，不確定性が大きいのかもしれない．
