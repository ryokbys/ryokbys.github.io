---
title: Disable PEP8 check when writing python code in emacs
date: 2016-03-02 11:48:27
category: misc
slug: disable-pep8-check-when-writing-python-code-in-emacs
authors: Ryo KOBAYASHI
---

tags: emacs, python
PEP check when I open a pyhton code with emacs is so annoying. So I
figured out how to disable PEP8 check, and this is how it works.

In the `.emacs.d/init.d` or appropriate file in your system,
`python-check-command` should be set `nil`. :

    (add-hook 'python-mode-hook
              '(lambda ()
                 (seq python-check-command nil)
                 ))

Then you never be bothered by too strict PEP8 checker.
