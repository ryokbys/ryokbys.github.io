---
title: Minimization algorithms
date: 2016-07-13 15:58:45
category: misc
slug: minimization-algorithms
authors: Ryo KOBAYASHI
---

tags: research, paper
CGやBFGSや，ニューラルネットの世界ではSGDなどの様々な最適化手法があるが，どのようなケースがどれがどれだけ効率的なのかを系統的に調べたケースがあまり見当たらない．

[Reddit](https://www.reddit.com/r/MachineLearning/comments/4bys6n/lbfgs_and_neural_nets/)
にて質問があって，質問者自身がいろいろと論文などを掲載してくれている．

-   [\"On Optimization Methods for Deep
    Learning\"](http://machinelearning.wustl.edu/mlpapers/paper_files/ICML2011Le_210.pdf)

上記論文では，基本的にはSGDよりもL-BFGSやCGの方が効率的で，変数が少ないとL-BFGSが，変数が多いとCGの方が優位になる傾向のようだ．

しかし，どうも自分の経験とは異なる．
CGは常にBFGSよりも効率が悪いと印象なのだが，大きな問題になると変わってくるのだろうか？
