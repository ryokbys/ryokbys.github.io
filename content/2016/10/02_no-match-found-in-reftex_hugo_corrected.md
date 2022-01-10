---
title: No match found in Reftex
date: 2016-10-02 21:52:48
category: misc
slug: no-match-found-in-reftex
authors: Ryo KOBAYASHI
---

tags: emacs
Sometimes when writing a LaTeX document with Emacs.app in MacOS and I
tried to put citation in the document by typing `Ctl-C + [`, emacs fails
to search references I am looking for. I do not know the reason exactly,
but I seems that emacs does not store the reference information in it.

So the solution is to reload the reference file by typing
`M-x Reftex-parse-all`. Then emacs will reload bibtex database and
reftex by `Ctl-C + [` begins to work again, perhaps.
