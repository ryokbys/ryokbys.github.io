---
title: "Stylish Jupyter Notebook with jupyter-themes"
Date: 2020-03-24T10:24:48+09:00
Category: misc
Slug: jupyter-themes
Authors: Ryo KOBAYASHI
tags: 
- "notebook"
- " python"
---

Appearance of Jupyter Notebook can be modified using `jupyter-themes`.

See the following websites for details,

* [jupyterの表示拡張機能、jupyter-themesを徹底紹介する](https://www.hirayuki.com/zakki/jupyter-notebook-dark-thema)
* [jupyter-themes の fontについて](https://qiita.com/koikoi_jam/items/29d9ef4e16a42038325c)

Chrome extension `Stylish` should be installed in addition to `jupyter-themes`.

My current setting is provided by the following command.
```bash
$ jt -t grade3 -f code -fs 12 -cellw 95% -lineh 140 -T -N -tf droidsans -tfs 12 -nf ptsans -nfs 12 -ofs 10
```

It looks good ;)

![]({static}/images/200324_jupyter-themes.png)
