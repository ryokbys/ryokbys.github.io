---
title: Today stamp in python
date: 2016-01-15 16:08:01
category: misc
slug: today-stamp-in-python
authors: Ryo KOBAYASHI
---

tags: python
Pythonで実行時の日付表示のためのやり方をいつも忘れるからメモる！

``` python
from datetime import datetime as dt
d= dt.today()
print d.strftime("%Y/%m/%d")
```

これだけ．．．
