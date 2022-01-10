---
title: Use logger in python
date: 2016-01-07 18:38:11
category: misc
slug: logger
authors: Ryo KOBAYASHI
---

tags: python
# 使用例

Pythonでのloggerの使い方を学んだからメモしておく．

``` python
import logging
logger= logging.getLogger(__name__)
console = logging.StreamHandler()
console.setLevel(logging.INFO)
logger.setLevel(logging.INFO)
formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')
console.setFormatter(formatter)
logger.addHandler(console)

logger.debug('this is debug')
logger.info('this is info')
logger.warn('this is warn')
logger.error('this is error')
logger.critical('this is critical')
```

のようにして `logger`
オブジェクトから出力を吐いていくことでログを作成できる．

上の例では，コンソールに出したいために `StreamHandler`
をloggerに対して加えているが， `FileHandler`
でファイルを指定したものをさらにloggerに追加してやれば，ファイルに出力することも可能．

# 良い点

-   ロガーの出力レベルを変更することで出力されるかどうかが決まるから，例えばデバッグしたいときにコードを変更したり，デバッグ終わったから出力コードを削除したりしなくてよい．
-   コンソール用とログ用の出力を別々にコードしなくてよい．

# 以下参考

-   <http://victorlin.me/posts/2012/08/26/good-logging-practice-in-python>
-   <http://qiita.com/amedama/items/b856b2f30c2f38665701>
