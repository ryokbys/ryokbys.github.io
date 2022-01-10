---
title: Simple minimization using FIRE
date: 2016-04-27 17:36:56
category: misc
slug: simple-minimization-using-fire
authors: Ryo KOBAYASHI
---

tags: research
Bitzek et al. *Structural Relaxation Made Simple*, PRL **97**, 170201
(2006)

Damped
MDのような発想で，より効率的に，MDコードをあまり改変する必要なく構造最適化を行う方法．
論文にアルゴリズムも記載されており，良いパラメータの値も提案されているから本当に実装は簡単．
それでいて，BFGSやCGと同程度もしくはより効率的に構造最適化が可能．
