---
title: "2019年初頭における作業環境"
Date: 2019-02-22T10:25:16+09:00
Category: misc
Slug: 2019-work-environment
Authors: Ryo KOBAYASHI
tags: 
- "mac"
- "linux"
---

一昨年，昨年と年の初頭における作業環境をメモしていたので，今年も書いておく．

## tmux

これはかなり秀逸なツールなので，愛用している．リモートサーバ上での作業に関しては，これが入っていないと面倒でしかたがない．
以下のスクリプトを`~/.bashrc`もしくは`~/.bash_profile`に書いておく．
```bash
function tmux() {
    local tmux=$(type -fp tmux)
    case "$1" in
        update-environment|update-env|env-update)
            local v
            while read v; do
                if [[ $v == -* ]]; then
                    unset ${v/#-/}
                else
                    # Add quotes around the argument
                    v=${v/=/=\"}
                    v=${v/%/\"}
                    eval export $v
                fi
            done < <(tmux show-environment)
            ;;
        *)
            $tmux "$@"
            ;;
    esac
}
```
そうして，以下のコマンドを打つことで，tmux内のX windowの設定を切り替えられるので，gnuplotなどのグラフをちゃんと転送できるようになる．
```bash
$ tmux update-env
```

## Jupyter Lab

こちらも前年同様で，引き続き使用している．もはや欠かせない．前年度からの変更は，プロジェクト毎のディレクトリの構造を整理し，`project/`ディレクトリの中に，`src/codebase/`というディレクトリを用意し，その中に`__init__.py`という空のファイルと，pythonスクリプトを置いておく．それらのスクリプトは，そのプロジェクトに共通する便利スクリプト群であり，そのプロジェクトで使うすべてのjupyter notebookから参照しやすくした．jupyter notebookからそれらを利用する際には，以下のようなコードを書けば良い．
```python
import sys
projsrc = '/path/to/project/src/'
sys.path.append(projsrc)
#...There is a codebase directory in projsrc/src/
from codebase import pmd
from codebase import cspline as csp
```

## Quiver.app

昨年から引き続き使用している．しかし，最近iPad Proを使うようになってから，一部のメモはそちらに移行している．

## iPad Pro + GoodNote5

iPad Proで GoodNote5 アプリを使ってノートを取ることが増えてきた．
Quiverに比べると，以下の点でメリットがある．
* 手書き
* 画像を任意の場所に，任意のサイズで貼り付けられる
* 貼り付けた画像に書き込める

パソコン作業しているもののメモには，（iPadに持ち帰るのが面倒なので）未だにQuiverの方が便利だが，それ以外のメモなどにはiPadを使うことが多くなってきた．iPad + GoodNote5があれば，本当に論文を印刷する必要がなくなってきた．


