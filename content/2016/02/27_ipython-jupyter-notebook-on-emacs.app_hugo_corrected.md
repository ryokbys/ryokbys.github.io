---
title: Use ipython/jupyter notebook on Emacs.app
date: 2016-02-27 19:47:42
category: misc
slug: ipython-jupyter-notebook-on-emacs.app
authors: Ryo KOBAYASHI
---

tags: python, emacs, mac
# Setup

To use ipython/jupyter notebook in Emacs, you need `elpy` and `ein`
(emacs ipython notebook) packages. So write those dependencies in
`.emacs.d/Cask` file (here I assume that the package management system
`cask` is installed), :

    (depends-on “ein")
    (depends-on “elpy”)

# Usage

## Boot notebook server

At first, ipython/jupyter notebook server should be booted before using
`ein` in Emacs. Otherwise Emacs cannot connect to the notebook server
and it just does not work.

## Call *ein* in Emacs

`ein` can be booted with `M-x ein:notebooklist-open` command. You can
open existing notebook or create new one.

::: note
::: title
Note
:::

If you get an error message like `Invalid slot ...` when you try to
start *ein*, it may be caused by *eieio* package additionally installed
with some packages, not included in Emacs.app itself. You may have to
remove it from `.emacs.d` directory.
:::

As you can see the following picture, graphs and pictures can also be
shown in Emacs.app.

![](https://farm2.staticflickr.com/1482/25313177185_35009373ae_o.png){width="400px"}

-   Markdown text cannot be shown as fancy decorated look.
-   emacs in Terminal.app does not show graphs either.

## Keybindings

Keybindings are different from using the notebook on browsers.

`C-c C-c`

:   Execute the current cell

`C-c C-b`

:   Insert a cell below

`C-c C-a`

:   Insert a cell above

`C-c C-k`

:   Kill the current cell.

`C-c C-n`

:   Move to the next cell

`C-c C-p`

:   Move to the previous cell

`C-c C-s`

:   Divide/separate the current cell into two.

`C-c C-m`

:   Merge two cells into one.

`C-c C-t`

:   Change cell type (ipython, markdown, raw)

# References

-   [Emacs - the best python
    editor?](https://realpython.com/blog/python/emacs-the-best-python-editor/)
-   [Emacsでipython
    notebookを使う](http://nullbyte.hatenablog.com/entry/2015/06/25/024311)
    (Japanese)
