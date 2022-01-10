---
title: "My Obsidian workflow"
Date: 2020-08-03T11:39:17+09:00
Category: misc
Slug: my-obsidian-workflow
Authors: Ryo KOBAYASHI
tags: 
- "mac"
- " Obsidian"
- " zettelkasten"
---

[Obsidian](https://obsidian.md)というnote-takingアプリがなかなか良く出来たアプリで，今流行りの[Zettelkasten](https://zettelkasten.de)が実装できるというので一ヶ月ほど使っている．

アプリの紹介は「[第２の脳アプリObsidianを使って日次メモを書く方法](https://note.com/takibayashi/n/nd6250964f0a7)」なんかを参照してもらうとして，自分の使い方をメモを兼ねて紹介しておく．
ちなみに，このやり方は [nickmiloのIMF-v3](https://github.com/nickmilo/IMF-v3) を参考にしている．

## ディレクトリ構造

- ノートをカテゴリしてディレクトリに分けるのは良くないと言われるのを良く耳にするので，基本的にはノートはディレクトリで分けたりせず，全て（Obsidian vaultの）ルートディレクトリ内に直に保存する．
- 画像ファイル，PDFなどの添付ファイルはルートディレクトリに一緒に置くとごちゃつくので`00_assets/`というディレクトリに保存することにする．
- 日々のノートは個別のノートとは少し性格がことなるので，`01_daily_notes`というディレクトリに保存する．


## 日々のノート（Daily notes）

- 日々のメモノートとそれ以外は少し用途というか考え方が異なるので，日々のノートは`01_daily_notes`という保存し，Obsidianの「Daily notes」plugin機能を使って，`YYYY-MM-DD`というファイル名で作成することにする．
- Daily notes pluginでは，テンプレートファイルを設定することができるので，`01_daily_notes/template`という以下のようなファイルを作り，それをテンプレートとする．
```Markdown
tags: #diary

# Tasks
- [ ] something

---

# Notes

```
- daily notesなのに，タグとしては`#diary`となっているのはご愛嬌．．．
- 最初にdaily noteのTasksに，今日やることをリストアップして，チェックしていくという流れ．
- `# Notes`以下にメモを書いていく．一つのノートとして独立するような内容はコピペで独立したノートを作り，リンクする．


## Index and Map of Contents (MOC)

全てのノートがルートディレクトリ内に一様に保存されているので，何かしらのインデックス・ノートなり各ノートを参照する機構がないと，個々のノートが単独で保存されてしまい，どこからも行き着くことの出来ない「dead note」になってしまう．そのため，[nickmiloのIMF-v3](https://github.com/nickmilo/IMF-v3)に従って，indexノートとMOCノートを作る．

### indexノート `000 Index`

```Markdown
# 000 Index

## Index Map
Each index file contains **Map Of Contents (MOC)** information.
- 000s -- [[000 Index]]
- 010s -- [[010 Personal Life MOC]] 自分・家族・生活に関すること
- 030s -- [[030 Computer MOC]] 関連，使い方，ソフトウェア
- 040s -- [[040 Finance MOC]] 資産運用，投資など
- 050s -- [[050 Productivity MOC]] - note-taking, project managements, productivity
- 090s -- [[090 Learning MOC]] 学習のことなど
- 00_assets/ -- テキスト以外のファイル格納庫
- 01_daily_notes/`YYYY-MM-DD.md` -- 日記
- 99_archive/ -- もう更新されないノート格納庫

## Tags

- 000s -- #IMF #MOC #Zettelkasten 
- 010s -- 
- 030s -- #computer #software #commands #スパコン 
- 040s -- #株 #資産運用 
- 050s -- #obsidian #note-taking #diary #productivity #GTD
- 090s -- #learning

## Rules

- 全てのノートはルートディレクトリに保存．ディレクトリによるカテゴリ分けはしない．タグとリンクでつなげる．
- ノートは何かのノートからwiki風リンクで派生させる形で作成する．
- 上記インデックスファイルからMOC noteにリンクして，MOCからさらにリンクしていく
- 会議・ミーティングは`MTG YYMMDD 会議名.md`というノートに．
```
- `000 Index`というファイル名にすることで，ソートすると一番上に表示される．
- ここで`[[...]]`のような記述は，wiki風のリンクでObsidianの中でリンクとして管理されて，backlinkなんかもつくので便利．
- 各カテゴリーのMOCに関する説明と，そのMOCファイルへのリンク，および関連するタグなどを記載しておく．
- Obsidianノート管理のルールが書いてある（これは別ノートに書いても良いかも）

###  MOCノート

各カテゴリーやトピックに関連するノートへのリンクをまとめておくノート．

例えば，`000 Index`ノートにある`[[050 Productivity MOC]]`からは下記のようなノートに飛ぶようになっている．
```Markdown
# 050 Productivity MOC
#note-taking #obsidian #MOC #IMF #Zettelkasten #productivity 

ノートのとり方，情報のまとめ方，情報の引き出し方など．文章の書き方なども含めても良い．

---

## Notes

- [[Article_Khan_Zettelkasten]] -- long blog post by Al Kahn
- [[Flow creation]] -- from nickmilo's IMF v3
- [[How To Take Smart Notes]] -- by TIAGO FORTE
- [[Marp CLI]] -- markdown-to-slide tool made by some Japanese guy

## Writing my blog
- [[pelican_blog_workflow]]

--- 
## Useful notes in Obsidian

- [[LaTeX preambles]] -- ここのpreambleをコピペしてノートの最初に書いておくとLaTeXを書くのが便利になる
```

### Hotkeys

Obsidianではキーボードショートカットをかなり自由に設定できるので，以下のような自分仕様の変更を加えている．

- `Cmd + T` -- 今日のDaily noteを開く
- `Cmd + 1` -- 左サイドバーの表示／非表示
- `Cmd + 2` -- 右サイドバーの表示／非表示

他に，特に変更していないが良く使うものをあげておく．

- `Cmd + O` -- ノートをタイトルで検索して開く．最近開いたノートも表示される．
- `Cmd + Shift + F` -- 全ノートの文字列検索．

