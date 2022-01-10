---
title: Inheritance in python
date: 2016-01-08 18:12:45
category: misc
slug: inheritance-in-python
authors: Ryo KOBAYASHI
---

tags: python
Pythonでクラスの継承の仕方を練習したのでメモしておく．

以下のようにして， `ClassA` と `ClassB` を定義する．

``` python
class ClassA(object):
    def __init__(self,x):
        self.x= x

    def multi(self):
        return self.x *self.x

class ClassB(ClassA):
  def multi(self):
      return super(ClassB,self).multi() *self.x
```

これを実行すると，以下のように狙い通りクラスの継承およびオーバーライドができていることが分かる．
:

    >>> a= ClassA(10)
    >>> print a.x
    10
    >>> print a.multi()
    100
    >>> b= ClassB(5)
    >>> print b.x
    5
    >>> print b.multi()
    125

注意すべきは，

-   上書きされた関数 `multi()` のみが変更を受けている．
-   `ClassB` において変更する必要のない関数は (`__init__` であっても)
    書く必要はない．勝手に継承される．
-   元となる `ClassA` で `object` を継承しないと `super` が使えない．
-   python 3 以降では，単に `super()`
    とすればよいらしいが，2.7とかでは上のように冗長的に書く必要があるらしい．第二引数の
    `self` はレシーバと呼ばれるらしい．
