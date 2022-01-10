---
title: Use argsort
date: 2016-01-10 18:43:15
category: misc
slug: use-argsort
authors: Ryo KOBAYASHI
---

tags: python
Numpyパッケージにリストを並び替えるための `argsort`
という関数があるが，その使い方がちょいと難しいからメモ．

``` python
>>> a= np.array([3,1,2])
>>> b= np.argsort(a)
>>> b
array([1, 2, 0])
>>> a
array([3, 1, 2])
>>> c= np.array(a)[b]
>>> a
array([3, 1, 2])
>>> b
array([1, 2, 0])
>>> c
array([1, 2, 3])
```

この例を見ると， `a=[3,1,2]` の `argsort` で得られたリスト `b=[1,2,0]`
は， `a` をソートするとしたらこの順番ですよという `a`
の要素番号のリストとなっている． なので， `[a[b[0]],a[b[1]],a[b[2]]]`
というリストを作れば `a` を並び替えられることになる．
これを実行しているのが，

``` python
c= np.array(a)[b]
# or
c= a[b]
```

なのだが，この書き方はなんだか簡略化しすぎていて理解が難しい気がする．
おそらくNumpyだと，こういう演算が許されているということだと思うのだが．．．
