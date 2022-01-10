---
title: Pythonで可変数の引数の取り方 \*args, \*\*kwds
date: 2016-03-22 18:01:15
category: misc
slug: arguments-of-function-in-python
authors: Ryo KOBAYASHI
---

tags: python
関数の引数に `*args` や `**kwds`
を指定した場合の挙動をいつも忘れてしまうので，例と共に書いておく．

``` python
def f(*args, **kwds):
    print args
    print kwds
```

上のような関数は， `args` は変数名なしの引数をタプルとして受け取り，
`kwds` は変数名付きで辞書型として受け取る．

``` python
f(1,2,3,4,a='a',b='b')
```

::: parsed-literal
(1, 2, 3, 4) {\'a\': \'a\', \'b\': \'b\'}
:::

下のように，タプルと辞書を明示的に渡してやっても，受け取り側は二つの変数名なしの変数を受け取ったと考えるので，
`*args` として `(arr,dic)` を受け取ったと解釈する．

``` python
arr = (1,2,3,4)
dic = {'a':10, 'b':20}
f(arr,dic)
```

::: parsed-literal
((1, 2, 3, 4), {\'a\': 10, \'b\': 20}) {}
:::

以下のようにすると `arr` を展開して `args` として， `dic` を展開して
`kwds` として渡すことができる．

``` python
f(*arr,**dic)
```

::: parsed-literal
(1, 2, 3, 4) {\'a\': 10, \'b\': 20}
:::
