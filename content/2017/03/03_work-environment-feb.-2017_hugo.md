---
title: "2017年初頭における作業環境"
Date: 2017-03-03T22:36:38+09:00
Category: misc
Slug: work-environment-feb.-2017
Authors: Ryo KOBAYASHI
tags: 
- "mac"
- " linux"
- " research"
---

最近，近年で最もコンピュータ上での作業環境が変化したので，僕の作業環境を紹介してみる．
前提として，自分の仕事上，どのような作業が多いかを挙げておく．

* ローカルマシン（MacBook）にてプログラム作成，結果の解析，文書作成など
* 研究室にあるクラスタマシンにて小規模並列計算，パラメータ並列計算（小さな計算を大量に）
* 名大スパコンを利用して大規模並列計算，小規模大量計算

上記のように，**いくつかの異なるサーバーにて種々の異なる作業を行っている**．
そのため，どのサーバの，どのディレクトリで，どのような作業を行っているかを把握するのがかなり面倒になっていた．また，サーバに接続し直すたびにその作業ディレクトリに移動し直したりするのがかなりの時間のロスだったようだ（これは，それが当然と思っていたときには気づかないものだった）．
最近，以下の２つ技術の導入したことで，（少なくとも気分的には）かなり楽になった．

## tmux

ターミナル・マルチプレクサと呼ばれるもので，

1. 一つの画面でいくつものターミナルを立ち上げて，それらを切り替えることができる
2. `tmux`自体がデーモンのようにバッググラウンドで走っており，リモートホストで`tmux`が起動している場合，接続が切れたとしても，接続し直すと以前の作業に復帰できる．

この **2** が本当に良い機能で，使ってみないと「そんなものかね」的に思うが，一度使ったら最後，これがないリモートサーバで作業する気などなくなってしまう．そのくらい良い機能．

インストールや設定の仕方，使い方などはすでにインターネット上にいろいろと出回っているので，ここには記載しないが，いくつかのキーバインドは自分が便利と思うものを載せておく．
以下のような内容で `~/.tmux.conf` を保存しておくことで，自分の好きな設定ができている（ほとんどどこかのウェブサイトから取ってきたものだが，どこかを忘れてしまった）．

```
# Prefix変更 C-b -> C-t
set-option -g prefix C-t
bind-key C-t send-prefix
unbind-key C-b

# 設定再読込キーバインド
unbind r
bind r source-file ~/.tmux.conf \; display "Reloaded ~/.tmux.conf"

# key bind (windowの移動)
# カーソルキーで移動
bind -n left previous-window
bind -n right next-window

# key bind (paneの移動)
# Shift + カーソルキーで移動
bind -n S-left select-pane -L
bind -n S-down select-pane -D
bind -n S-up select-pane -U
bind -n S-right select-pane -R

# 256色端末を使用する
set-option -g default-terminal "tmux-256color"
# viのキーバインドを使用する
# set-window-option -g mode-keys vi

# ステータスバー設定
set -g status-interval 10
set -g status-bg colour100
setw -g window-status-current-fg black
setw -g window-status-current-bg white

# split panes using | and -
bind h split-window -h
bind v split-window -v
unbind '"'
unbind %
```

* prefixを `C-t` に変更
* 矢印の左右で，windowを切り替える
* prefix + `h` でウィンドウを横（horizontal）分割
* prefix + `v` でウィンドウを縦（vertical）分割
* `Shift`+ 上下左右でペインを切り替えるとなっているが，Shift+上下がうまく働いていないため，`C-t` + `o` で切り替えている．


## Jupyter notebook

もう一つは，jupyter notebook．
これは，ブラウザ上でプログラムを作成し，簡単に実行して結果を確認できるもの．自分の場合はたいてい python でなんらかの軽い処理をする際に重宝している．
良い点としては，

1. すぐに結果が見れる
2. カード毎に実行できるため，以前に書いた一箇所だけを実行とかできる
3. Markdownでコメントを書き込める
4. 数式も書ける
5. グラフや画像なども挿入できる
6. リモートサーバー上のnotebookをローカルのブラウザで実行できる

これらが可能なため，実際に何らかの解析作業などを行う場合，何をどうやるかやをMarkdownで書きながら実際にコードも書いて，実行した結果も一つのノートの上に表示されていく感じ．実験ノートを書きながら解析作業を行えているようなもので，後から見ても思考の過程をたどることができる．

そして， **6** ができるため，ちょっと重たい作業を行ってもローカルマシンがヒートアップしたり遅くなったりしないのが良い．

インストールは **pyenv** を入れた後に，**anaconda** をインストールすれば自動的についてくる（便利な世の中になったものだ）．

この２つの技術は全く独立しているものだが，リモートサーバー上でjupyter notebookを使うに際して，`tmux`が役に立つ．
`tmux`をリモートサーバーで一度起動しておけば，ログアウトしても，プロセスを起動し続けることができるから，jupyter notebookをリモートサーバー上で常時起動させておくことが可能になる．これはまたかなり便利（これも体験してみないとなかなか便利さが伝わらないと思うが）．


## おまけ

### [Marp](https://yhatt.github.io/marp/)

日本人が作成した，Markdownでスライドを作れるアプリ．機能としては，

- Markdownで記述する
- リアルタイム（just-in-time?）でスライドが更新される
- コード，数式，図を扱える

これでかなり楽にスライドを作れるから便利．



### [Quiver](http://happenapps.com/#quiver)

Programmer's notebookという触れ込みだが，プログラム実行のできないjupyter notebookを束ねたEvernoteのようなもので，それだけ効くと何が良いのか分からない．
何が良いかというと，

- ノートを取って後から検索できる．
- Markdownでメモを取れる．
- ノートがcell単位で成り立っており，各セルで，Markdown，コード，数式（LaTeX）などの形式を選択することができる．
- ノートカテゴリを分けたりタグを付けたりといったEvernote的なこともできる．

実際に使ってみると，プログラミングに関連したほぼすべてのことをメモするのには事足りるので，Evernoteよりも便利と思う．
