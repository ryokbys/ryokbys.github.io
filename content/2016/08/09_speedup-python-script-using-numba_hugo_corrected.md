---
title: Speedup python script using numba
date: 2016-08-09 10:20:50
category: misc
slug: speedup-python-script-using-numba
authors: Ryo KOBAYASHI
---

tags: python, research
[Numba](http://numba.pydata.org)
というモジュールを利用することで，pythonスクリプトの数値演算のみからなる関数を劇的に高速化することができる．
（数値演算でない部分は高速化されない．）

ただし，高速化する関数の書き方を気をつけないとその恩恵を受けられないので注意．

# 簡単な例

``` python
import numba
import numpy as np

def matmul1(a, b):
    lenI = a.shape[0]
    lenJ = a.shape[1]
    lenK = b.shape[1]
    c = np.zeros((lenI, lenJ))
    for i in range(lenI):
        for j in range(lenJ):
            for k in range(lenK):
                c[i, j] += a[i, k] * b[k, j]
    return c

@numba.jit
def matmul_numba(a, b):
    lenI = a.shape[0]
    lenJ = a.shape[1]
    lenK = b.shape[1]
    c = np.zeros((lenI, lenJ))
    for i in range(lenI):
        for j in range(lenJ):
            for k in range(lenK):
                c[i, j] += a[i, k] * b[k, j]
    return c
```

上記のように，普通に書いた関数と `@numba.jit`
を付けた関数とを用意して，速度比較する．

``` python
ndim = 100
a = np.random.randn(ndim,ndim)
b = np.random.randn(ndim,ndim)

%timeit matmul1(a,b)
%timeit matmul_numba(a,b)
```

    1 loops, best of 3: 771 ms per loop
    1000 loops, best of 3: 1.12 ms per loop

のように， `@numba.jit`
を付けたものの方が圧倒的に高速化されていることが分かる．

# numbaによる高速化を阻害する要因

上記のように， `@numba.jit`
デコレータを付けるだけでかなりの高速化が望めるので，うれしいが，関数内にそれを阻害する要因があると，こうそくかされない．
*numba*
によるオーバーヘッドで少し遅くなるという結果に終わることもあるので注意．

例えば，下記のようなコードを考える（Coulombポテンシャルを直接法で計算する関数）．

``` python
# Compute direct sum of Coulomb sum in the cubic region
import numba
import math

@numba.jit
def direct_cube(nc=5,pos=[],chgs=[],alc=10.):
    phi = 0.0
    r = np.zeros((3,),dtype=float)
    rcut = (nc-1)*alc
    li = np.zeros((3,),dtype=float)
    ndim = len(chgs)
    for ix in range(-nc+1,nc):
        for iy in range(-nc+1,nc):
            for iz in range(-nc+1,nc):
                li[:] = [float(ix),float(iy),float(iz)]
                for pi,zi in zip(pos,chgs):
                    r[:] = (-pi[:] -li[:])*alc
                    denom = np.linalg.norm(r)
                    phi += zi/denom
    return phi,rcut
```

これの時間を測ってみると， :

    %timeit phi = direct_cube(nc=5,pos=pos,chgs=charge,alc=alc)
    1 loops, best of 3: 1.07 s per loop

ここで， `pos` や `chgs`
は原子座標(pi=\[xi,yi,zi\])と原子当たり電荷(zi)であり，原子数個の次元を持っている（ここではあまり考える意味が無いので無視してよい）．
これを `@numba.jit` なしで実行すると， :

    1 loops, best of 3: 941 ms per loop

となり，僅かに `@numba.jit` なしの方が速い．

## np.linalg.normの置き換え

まずは，最も内側のループ内の，

``` python
denom = np.linalg.norm(r)
```

の部分を，

``` python
denom = math.sqrt(r[0]*r[0]+r[1]*r[1]+r[2]*r[2])
```

のように，置き換える．すると， :

    %timeit phi = direct_cube(nc=5,pos=pos,chgs=charge,alc=alc)
    1 loops, best of 3: 565 ms per loop

しかし， `@numba.jit` なしの場合， :

    1 loops, best of 3: 493 ms per loop

に比べると，僅かに遅い．

## zip関数の置き換え

``` python
for pi,zi in zip(pos,chgs):
```

の部分を，

``` python
for i in range(ndim):
    pi = pos[i]
    zi = chgs[i]
```

のように置き換えると， :

    %timeit phi = direct_cube(nc=5,pos=pos,chgs=charge,alc=alc)
    100 loops, best of 3: 11.6 ms per loop

のように劇的に速くなる．ちなみに， `@numba.jit` なしの場合， :

    1 loops, best of 3: 501 ms per loop

のようにあまり変わらない．

# 結論

-   `zip` 関数を使わない．（ `enumerate` は高速化する）
-   最内側のループ内の関数がボトルネックの可能性があるので気をつける（今回の場合は
    `np.linalg.norm()` が問題）

ちなみに，

-   最内側の `if` 分は高速化をあまり阻害しない
-   `kv[:] = [float(x) for x in (kx,ky,kz)]`
    のようなリスト内包表記があるとJITコンパイルできないらしく，
    `kv[:] = [float(kx),float(ky),float(kz)]`
    のように書き換える必要がある．
