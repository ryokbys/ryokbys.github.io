---
title: flake8 personal setting for python
date: 2016-10-27 06:44:20
category: misc
slug: flake8-personal-setting-for-python
authors: Ryo KOBAYASHI
---

tags: python, emacs, mac
When I code python programs using emacs, emacs uses elpy and elpy uses
flake8 for syntax checking. But this syntax checking is kind of
annoying. For example, I would like to write like this

``` python
x = some_func(a,b,c+d)
```

instead of this

``` python
x = some_func(a, b, c + d)
```

I think this style condtains too many whitespaces which make code
lengthy.

To change the style in flake8, you can write the following lines in
`~/.config/flake8` :

    [flake8]
    ignore = E226, E231

To be honest, I forget what those E226 and E231 means (one can google
them) but at least you can change the syntax style by doing this.
