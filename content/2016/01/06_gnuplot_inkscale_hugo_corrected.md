---
title: From gnuplot to inkscape
date: 2016-01-06 11:39:42
category: misc
slug: gnuplot_inkscale
authors: Ryo KOBAYASHI
---

tags: mac, research, inkscape
Mac OSXのバージョンがEl Capitanになってから，Illustrator
CS4の調子がすこぶる悪く，ほぼ使えない状態なので，gnuplotなどで出力したグラフを編集してEPSやPDFにする作業が滞っていたので対策を記載．

流れとしては，

1.  gnuplotから **SVG** 出力．
2.  inkscapeで編集．
3.  inkscapeでEPSもしくはPDF出力．

具体的に必要なのは，gnuplotとinkscapeがインストールされていること．inkscapeはbrewなどでインストールできると思う．

1.  gnuplotで， :

        ...
        gnuplot> set term svg
        gnuplot> set output 'hoge.svg'
        gnuplot> replot

    として **SVG** 形式にして出力．

2.  inkscapeで編集．

3.  キャンバスサイズの調節．

    1.  すべてを選択しておく．
    2.  キャンバスサイズの変更のため， `File` \>
        `Document properties...` を開く．
    3.  開いたウィンドウの真ん中あたりの，Custum sizeのところで，Unitsを
        **px** にし，すべての辺のマージンを **10.0** とする．
    4.  `Resize page to drawing or selection`
        ボタンを押してキャンバスサイズを調節．

4.  `File` \> `Save as ...` でフォーマット（EPSかPDF）を指定して保存．

これでIllustratorがなくても仕事に支障はきたさない．
