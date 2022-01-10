---
title: "Convert docx files to pdf on Mac"
Date: 2021-03-31T11:04:23+09:00
Category: misc
Slug: convert-docx-files-to-pdf-on-mac
Authors: Ryo KOBAYASHI
tags: 
- "mac"
---

昨今の状況で，オンラインでのレポートの提出が多くなったが，docx形式で提出されたものをこちらで見やすいようにpdfに変換したいということがあった．
そこで，macOSで使えるAutomatorアプリを使って，大量の異なるディレクトリ内にあるdocxファイルをpdfに変換するワークフローを作ったのでメモしておく．（年間数回しか行わない作業なのでメモしないと忘れてしまう．）

Automator.appで「ワークフロー」を開いて，下図のようなコンポーネント構成にする．
![]({static}/images/210210_automator_docx2pdf.png)

指定されたフォルダ内のサブフォルダの中身も検索するようにして，フォルダ内の「docx」もしくは「doc」ファイル全てに対してAppleScriptを実行する．
実行するAppleScriptは以下のようなもので，どこかのサイトから引用したものだが，どこのサイトか忘れてしまった（スミマセン）．
```AppleScript
use scripting additions

on run {input, parameters}
  repeat with aFile in input
  	tell application "System Events"
  		set inputFile to disk item (aFile as text)
  		set outputFileName to (((name of inputFile) as text) & ".pdf")
  	end tell
  	
  	tell application id "com.microsoft.Word"
  		activate
  		open aFile
  		tell active document
  			save as it file name outputFileName file format format PDF
  			close saving no
  		end tell
  		set defaultPath to get default file path file path type documents path
  	end tell
  	
  	tell application "System Events"
  		set outputPath to (container of inputFile)
  		set outputFile to disk item outputFileName of folder defaultPath
  		move outputFile to outputPath
  	end tell
  end repeat
  return input
end run
```

このワークフローを実行すると，ディレクトリ指定を求められ，指定したらそのディレクトリ以下の全てのdocxファイルをpdfに変換してくれる．
自分の場合，これをDropboxなんかに入れて，iPadのDocumentからpdfファイルを開くことでApple pencilで赤ペン先生のように書き込むことができ，それが終わったら自動的にDropboxに同期されてmac側でも見れる．
