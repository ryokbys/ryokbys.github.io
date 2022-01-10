---
title: "新しいMacBookProへの移行の記録"
Date: 2017-01-31T22:54:48+09:00
Category: misc
Slug: move-to-new-macbookpro
Authors: Ryo KOBAYASHI
tags: 
- "mac"
---


## 移行アシスタントは使えなかった．

旧Mac（MacBookAir）と接続して，直接移行することをトライ．（一回目は，MBAの方はEl Capitanだったが，二回目はアップグレードしてSeirra同士での移行）
なぜだか最後，残り1時間40分から先に進めなくなる．
２回やってどちらもほぼ同じ症状（再現性あり）なので，ダメなのだろう．
ということで，否応無く__手動インストール__を余儀なくされた．

### macOS sierraのクリーンインストール

1. `Cmd+r`を押しながら起動．アップルマークが表示されたら`Cmd+r`を離して良いみたい．
2. ディスクユーティリティを開いてディスクを消去およびフォーマット
3. OSの再インストール

これ自体は時間は少々かかるが，スムーズに進む．



## Xcode

App Storeからダウンロード．
一度Xcodeを起動して認証(?)しないと，のちのhomebrewインストールで引っかかる．


## Terminal.appの設定

[Solarized](http://ethanschoonover.com/solarized) から，ファイルをダウンロード．
Terminal.appの環境設定「プロファイル」で，「読み込み...」からsolarizedの「Light」と「Dark」を読み込んでおいた．
フォントサイズを11ptから14ptへ変更．
ウィンドウサイズも変更．


## Mailデータ移行


[ここ](http://blog.skeg.jp/archives/2016/11/mail-backup-sierra.html)を参考にして，以下のファイル群を旧macからコピーした．

### インターネットアカウントの復元
```
/Users/UserName/Library/Accounts
```

### メールデータの復元
```
/Users/kazuto/Library/Mail/
/Users/UserName/Library/Mail Downloads/
/Users/UserName/Library/Containers/com.apple.mail/
/Users/UserName/Library/Preferences/com.apple.accounts.plist
/Users/UserName/Library/Preferences/com.apple.accountsd.plist
/Users/UserName/Library/Preferences/com.apple.mail-shared.plist
/Users/UserName/Library/Preferences/com.apple.MailMigratorService.plist
/Users/UserName/Library/SyncedPreferences/com.apple.mail-com.apple.mail.vipsenders.plist
/Users/UserName/Library/SyncedPreferences/com.apple.mail.plist
```
キーチェーン（/Users/UserName/Library/Keychains）は，iCloudで同期されていれば，復元する必要なし．


## Emacs

参考
[http://keisanbutsuriya.hateblo.jp/entry/2016/04/10/115945](http://keisanbutsuriya.hateblo.jp/entry/2016/04/10/115945)

```
$ brew tap railwaycat/emacsmacport
$ brew install emacs-mac
$ brew linkapps
```

### 設定ファイルのコピー
旧macからemacs用の設定ファイルを転送する．
```
$ rsync -avh /Volumes/kobayashi/.emacs.d ~/
```


### Caskのインストール
```
$ brew install cask
```

### ipython
emacs起動時にipythonコマンドが見つからないと言われる場合は，pythonのインストールがまず必要．



## python

pythonの新規インストール時にはプロジェクトごとにバージョンを切り替えられるpyenvをインストールしておいた方が良いらしい．

### pyenv

どうもpyenvだとかvirtualenvだとかさまざまあるらしいが，ここではpyenvとvirtualenvプラグインをインストールする方法を採用する．下記参考．
[http://cocodrips.hateblo.jp/entry/2014/09/02/171127](http://cocodrips.hateblo.jp/entry/2014/09/02/171127)
```
$ CFLAGS="-I$(xcrun --show-sdk-path)/usr/include" pyenv install 2.7.13
```
`CFLAGS`を設定しないと，macOS sierraではうまいことインストールできない．
参考
[http://kazscape.com/2014/12/kz-pythonインストール覚書/](http://kazscape.com/2014/12/kz-pythonインストール覚書/)

どうも，anacondaというものをインストールすると，numpy，scipyだとかipython，jupyterなどがインストールされるらしくてかなり便利らしい．
```
$ CFLAGS="-I$(xcrun --show-sdk-path)/usr/include" pyenv install anaconda-4.0.0
$ pyenv versions
  system
* 2.7.13 (set by /Users/kobayashi/.pyenv/version)
  3.5.3rc1
  anaconda-4.0.0
$ pyenv global anaconda-4.0.0
$ pyenv versions
  system
  2.7.13
  3.5.3rc1
* anaconda-4.0.0 (set by /Users/kobayashi/.pyenv/version)
```
とした．`CFLAGS`の設定がここでも必要かどうか知らないが，上で必要だったのだからおそらく必要だろう．

非常に便利なので`docopt`もインストールしておく．
```
$ pip install docopt
```

### matplotlib.pyplotの問題

このバージョンのmatplotlib (1.5.1)では，pyplotをimportしようとすると，下記のようなwarningが出て先に進まない（macOS Sierra 10.12.2）．
```
/Users/kobayashi/.pyenv/versions/anaconda-4.0.0/lib/python2.7/site-packages/matplotlib/font_manager.py:273: UserWarning: Matplotlib is building the font cache using fc-list. This may take a moment.
  warnings.warn('Matplotlib is building the font cache using fc-list. This may take a moment.')
```
  
* [http://stackoverflow.com/questions/34771191/matplotlib-taking-time-when-being-imported](http://stackoverflow.com/questions/34771191/matplotlib-taking-time-when-being-imported)
* [http://stackoverflow.com/questions/17490444/import-matplotlib-pyplot-hangs](http://stackoverflow.com/questions/17490444/import-matplotlib-pyplot-hangs)

上記でいろいろと解決策が議論されているが，どれも自分の場合は有効でなかったため，下記のような対策をとった．これは`matplotlib/font_manager.py`を書き換えることになるので注意が必要．

これはどうも，上記`font_manager.py`の273行目
```
        pipe = subprocess.Popen(['fc-list', '--format=%{file}\\n'],
                                stdout=subprocess.PIPE,
                                stderr=subprocess.PIPE)
```
という箇所が問題なようだ．`%{file}`という部分がバグっているように思われる．
ここは，font-listを`fc-list`を用いて取得する部分なのだが，その部分をごっそり無視するため，
font_manager.py:325,326行目の
```
            for f in get_fontconfig_fonts(fontext):
                fontfiles[f] = 1
```
の部分をコメントアウトする．
別にこの部分がなくとも，その２〜３行前で十分にフォントリストは取得できていると期待できるので，大きな問題は生じないと期待する．（動かないよりは良い！）


## tmux

端末仮想化コマンドらしい．いくつもの端末を起動しなくてもそれらを切り替えられ，通信が切れても後から復活できるらしい．かなり便利ツールで，unix/linuxユーザの間では常識ツールらしい．

* [http://kanjuku-tomato.blogspot.jp/2014/02/tmux.html](http://kanjuku-tomato.blogspot.jp/2014/02/tmux.html)
* [http://qiita.com/michiomochi@github/items/4bf8e34a91bbf3d9af20](http://qiita.com/michiomochi@github/items/4bf8e34a91bbf3d9af20)


## gnuplot

XQuartzを先にインストールしなくてはいけない．
[https://www.xquartz.org](https://www.xquartz.org)
その後，
```
$ brew install gnuplot —with-x11
```
ここで，`--with-x11`というオプションを指定してx11を使うことを宣言してインストールしなくてはならない．



## iTunes

参考
[https://digitalfan.jp/103717](https://digitalfan.jp/103717)

`~/Music/`フォルダ内の「iTunes」フォルダを旧MacからコピーしてくればOK．


## LaTeX

[http://qiita.com/hideaki_polisci/items/3afd204449c6cdd995c9](http://qiita.com/hideaki_polisci/items/3afd204449c6cdd995c9)
上記サイトに従ってインストールした．
今のところ問題なく動作しているように見える．


## homebrew-cask

homebrewだけでなく，homebrew-caskというのも使うと，よりインストールが楽になるらしい
```
$ brew install caskroom/cask/brew-cask
$ brew cask search
```
おそらく，unix系のコマンドラインだけでなく，ふつうのmac用アプリもこれでインストールできるようになるみたい．

## Google日本語入力

OS標準のことえりを一ヶ月ほど試してみたが，やはりGoogle日本語入力の方が少々頭が良い．

Google日本語入力をダウンロードしてきて，インストールし，「キーボード」環境設定の「入力ソース」で

* ひらがな (Google)
* 英数 (Google)

の２つを選択．Google入力ソースとして「カタカナ」，「半角カナ」，「全角英数」があるが，ほぼ使わないので入力ソースとして登録するよりは必要に応じて変換する方が良いと思う．

Emacsで `Control + Space` がマークセットに設定されている場合，これがOSのキーボードショートカットの「前の入力ソースを選択」とバッティングすると厄介なので，OSの設定を解除しておくとよい．

