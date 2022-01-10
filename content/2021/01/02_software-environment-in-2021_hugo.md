---
title: "2021年初頭における作業環境"
Date: 2021-01-02T00:46:38+09:00
Category: misc
Slug: software-environment-in-2021
Authors: Ryo KOBAYASHI
tags: 
- "mac"
- "linux"
---

ここ数年は年の初頭に作業環境についてメモしているので，2021年も元旦における作業環境についてメモしておく．昨年から変わっていないことも多いが，重複していても端折らずに書くことにする．

## Mac

Macにおいて使っているソフトを列挙する．

- macOS Big Sur (11.0.1)：一つ前のOSから見た目がファンシーになったが，ほとんどのソフトウェアは問題なく利用できている．
- Emacs：[GNU EmacsのHP](https://www.gnu.org/software/emacs/)からインストールした version 27.1 (9.0) を使っている．
- ターミナル：昨年は iTerms を使っていたようだが，いつからかOS純正のターミナルに切り替えた．理由は忘れてしまった．
- Google日本語入力：OS標準の「ことえり」ではなくこちら．これも理由は忘れてしまった．
- [Obsidian](https://obsidian.md)：昨年初頭時点では Quiver というメモアプリを使っていたが，昨年途中から Obisidian に切り替えた．Quiver よりも ZettelKasten というノートシステムに近いやり方で，ノート間のリンク・バックリンクを使うのがやりやすい．また，ノート一枚一枚が単なるローカルの Markdown ファイルだから，このアプリを使うのをやめてもノートはそのまま他のアプリで使えるから良い．
- Microsoft Teams：以前は Slack を使っていたが，コロナの影響で職場内で Teams を使うようになってきたから，研究室内で Teams を使うようにした．やはり MS 製ということでかなり使いづらい．．．
- WorkFlowy：TODOアプリとしてシンプルなこれを使用．人にすすめると月間のアイテム作成量を増やすことができ，現在500件/月だが，あまり頻繁には使っていないので問題ない．「いつまで使うか怪しい．．．」と昨年書いていたが，未だに使っている．
- [Paperpile](https://paperpile.com)：文献管理を Mendeley からこちらに変更．Mendeleyは文献PDFをiPadで読みながら書き込みなどをするのを簡単には出来ない．一方，PaperpileはGoogle製なのでGoogle Driveと相性が良く，Paperpileでスターを付けた文献PDFはGoogle Drive内の「Paperpile/Starred Papers/」にあるので，そのPDFファイルをiPad側のDocumentsアプリか何かで書き込むとそのままそれが反映される．
- PlainCalc：電卓ソフト．計算途中も結果も見えているのでOS標準より良い．
- JupyterLabからJupyter notebookに戻った．JupyterLabだとinteractive系の動作が不安定というより動作しなかったりするので，notebookに戻った．


## iPad Pro

- Goodnote5：「Apple Pencilでのノートアプリは結局これが一番使いやすかった気がする．」と昨年時点で書いていたが，未だにノート書くのにはこれを使っている．
- Documents：Goodnoteがノートを取るアプリとすると，Documentsは文献を読むアプリで，読みながら書き込むことができる．これはGoodnoteでもできるが，Documentsの良いところはDropboxやGoogleDriveのディレクトリをそのまま参照でき，そこに直接書き込むことが可能のようなUIとなっていて便利．


## Linux

メインの計算を行うのはもっぱらLinuxなので，こちらの環境もメモしておく．

- tmux：数年前から使っている．リモートの作業には必須．リモートマシンでこれを起動してそこで作業すると，ログアウトしてもそのプロセスが残るから，次にログインした際にはtmuxのプロセスにつなげ直すと以前接続した時のディレクトリだとかコマンドの続きだとかを引き継げるから便利．
- ソフトではないが作業環境として，すべてのリモートにおいてもローカルと同じディレクトリ構造にした．rsyncを使ってデータをやりとりする際にディレクトリ構造が一緒だと楽．さらに [rsync_wrapper](https://github.com/ryokbys/rsync_wrapper) というスクリプトを使うと，リモートマシンとローカルマシンのファイルなんかを一つのコマンドで同期することが可能で非常に便利．



