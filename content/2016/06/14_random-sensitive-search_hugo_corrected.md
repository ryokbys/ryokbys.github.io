---
title: Random structure search
date: 2016-06-14 14:25:38
category: misc
slug: random-sensitive-search
authors: Ryo KOBAYASHI
---

tags: research
EPFLでの会議PASC16にて，Chris J Pickardの発表を聴いた． そこで **Ab
initio random structure search (AIRSS)**
を紹介していたので下記論文を読んでみた．

-   Pickard, C. J., & Needs, R. J. (2011). Ab initio random structure
    searching. Journal of Physics: Condensed Matter, 23(5), 053201.
    <http://doi.org/10.1088/0953-8984/23/5/053201>

これまでにいくつかの構造探索アルゴリズムが提案されているが，ランダム探索といくつかのバイアス（原子間の結びつきや対称性）を加えるというシンプルなものが良いよという論文．
結局，以下に挙げられるようなどんなグローバル探索手法でも一長一短でこれが良いという決定打はない．ランダム検索でないと最安定構造を逃すことも多い．

しかし，当然ランダム探索では **curse of dimensionality (次元の呪い)**
によって扱えないクラスの問題もあるようで，電池材料のような元素がたくさん絡んでくるものはまだ難しいようだ．

# 構造探索手法

-   ランダム探索
-   Simulated anealling (SA)
-   Ensemble SA: いくつかの異なる初期値から並列にSA
-   Parallel tempering-like SA: いくつかの異なる温度でのSA
-   Particle swarm: ムクドリの大群を模擬したような手法
-   Evolutional algorithm: 進化論を模擬したような手法
-   Basin/minima hopping: local
    minimaをホッピングしていく手法（良く知らない）
-   Metadynamics:
    既に調べた探索空間にポテンシャルを加えていき新たな空間を探索
