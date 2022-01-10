---
title: VASP vs QuantumESPRESSO
date: 2016-08-03 15:30:54
category: misc
slug: vasp-vs-quantumespresso
authors: Ryo KOBAYASHI
---

tags: research, DFT
近年，さまざまなDFTコードが世に出ており，それぞれいろいろと特徴を出そうとしている．
擬ポテンシャルと平面波展開のコードについては，ほとんど同じことが出来るようになってきている．

最近少し使う必要性が出てきた **Quantum ESPRESSO (QE)** と VASP
を比較したサイトを検索したら，いかのようなものがあった．

-   速度：[QuantumESPRESSO vs VASP (Round
    1)](https://www.nsc.liu.se/~pla/blog/2013/02/04/qevasp-part1/)
-   精度：[Comparing Solid State DFT Codes, Basis Sets and
    Potentials](http://molmod.ugent.be/deltacodesdft)

# 感想

-   32 core くらいまでは VASP も QE も同じようなスピード
-   それ以上になると QE はほとんどサチってしまう (2013時点で band
    parallelizationなし？)
-   QE はいくつかの元素のPAW
    potentialの素性が良くないためか，計算ができなったらしい（Li2FeSiO4など？）（Liポテンシャルのcutoff
    energyが1100 eVと大きすぎ，それでは計算できないから小さいcutoff
    energyで計算したために？エラーで止まってしまったらしい）
-   QE の方が VASP
    よりも多くのメモリーを必要とする．そのため多くのノードに並列しないと計算が走らない．
-   上記サイト（速度）は2013年の結果だが，QEのメジャーバージョンアップはされていないので結果は大きく変わらないと予想される．
