---
title: "AtomEye on macOS Catalina"
Date: 2020-04-14T17:18:08+09:00
Category: misc
Slug: atomeye-on-macos-catalina
Authors: Ryo KOBAYASHI
tags: 
- "mac"
- " research"
---

## AtomEye

AtomEyeは原子・分子シミュレーションの構造を可視化するツール．コマンドラインに特化しているようで使い勝手が良いソフトウェアではないと思うが，動作も軽いし，原子のグラフィックスがきれいなので以前は結構よく使っていた．

最近少し事情があってAtomEyeを使わなければいけない状況になったので，macOS Catalinaで使おうとしたら困ったのでメモを残しておく．

AtomEyeのHP[1]でmac用バイナリが提供されているけど，どうも指定されているライブラリの位置がCatalineの中のライブラリの場所と異なっていたのが問題だった．
AtomEyeはコマンド名は`A`なことに注意．

まず，バイナリファイル内でリンクしている動的ライブラリへのリンクを調べる．
```bash
$ otool -L ~/bin/A
/Users/kobayashi/bin/A:
    /System/Library/Frameworks/vecLib.framework/Versions/A/vecLib (compatibility version 1.0.0, current version 325.4.0)
    /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 159.1.0)
    /usr/X11/lib/libXpm.4.dylib (compatibility version 16.0.0, current version 16.0.0)
    /usr/X11/lib/libXext.6.dylib (compatibility version 11.0.0, current version 11.0.0)
    /usr/X11/lib/libX11.6.dylib (compatibility version 10.0.0, current version 10.0.0)
```
しかし，Calatinaでは（もしくは自分の環境では）`/usr/X11`は空ディレクトリへのsymlinkで，`X11`は`/opt/X11`に存在していた．なので，上記の動的ライブラリのリンクを以下のように変更した．
```bash
$ install_name_tool -change /usr/X11/lib/libXpm.4.dylib /opt/X11/lib/libXpm.4.dylib ~/bin/A
$ install_name_tool -change /usr/X11/lib/libXext.6.dylib /opt/X11/lib/libXext.6.dylib ~/bin/A
$ install_name_tool -change /usr/X11/lib/libX11.6.dylib /opt/X11/lib/libX11.6.dylib ~/bin/A
```

他に，`/usr/X11`を`/opt/X11`へのsymlinkに変更するという手段も考えられるが，Catalinaではそれはできなかった．以前のmacOSでは，SIPなるものを無効にするとできたようだが，少なくとも自分の環境ではそれはできなかった．
ちなみに，ソースコードも取ってくることはできるらしいので，それをコンパイルすることは可能とは思う[2]．

## Reference

1. [AtomEye](http://li.mit.edu/Archive/Graphics/A/)
2. [Atomeyeをビルドする @Qiita](https://qiita.com/tkmtSo/items/93656c75b60c6728f9eb)
