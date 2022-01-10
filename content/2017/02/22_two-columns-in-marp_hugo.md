---
title: "Two columns in Marp"
Date: 2017-02-22T14:38:58+09:00
Category: misc
Slug: two-columns-in-marp
Authors: Ryo KOBAYASHI
tags: 
- "Marp"
- " mac"
---

Markdownでスライドを作成できる[Marp](https://yhatt.github.io/marp/)という日本人が作ったアプリがある．

僕が思うこのアプリの良いところは，

- Markdownで作成できる
- リアルタイムで表示される
- 図も挿入できる
- 数式もLaTeXライクに扱える
- PDFで出力できる

このアプリはすごい良いと思うが，Markdownで２コラムにすることができないため，
左に図，右に説明のようなことができない．

直接的にHTMLの`table`タグを以下のように差し込むことで一応２コラムにすることができる．
```
<table>
<tr style="border: 0px;">
<td style="border: 0px; width=60%;">
<center>
<img src="./figure.png" width="600">
</center>
</td>
<td  style="border: 0px;">

- この部分は
- 右側に表示される

</td>
</tr>
</table>
```
`style="border: 0px;"`というおまじないを`tr`と`td`タグに追加しないと，枠線が表示されてしまう．

この２コラム化をMarkdownだけでできたらかなり良いと思うんだけどな．．．

