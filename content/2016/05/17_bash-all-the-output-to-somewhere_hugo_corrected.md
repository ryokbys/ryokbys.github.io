---
title: "Bash: all the output to /dev/null"
date: 2016-05-17 10:31:56
category: misc
slug: bash-all-the-output-to-somewhere
authors: Ryo KOBAYASHI
---

tags: linux
I wanted to run jupyter notebook background without showing any outputs
from it. So I tried the following command, :

    $ jupyter notebook hoge.ipynb 2>&1 > /dev/null &

But it still showed some outputs on the screen and interrupted me typing
other commands. This is because, `2>&1` means that standard error output
goes to where the standard output currently pointing to, which is not
`/dev/null`. So I had to switch the order from `2>&1 > /dev/null` to
`1>/dev/null 2>&1` to make the error output go to `/dev/null`. :

    $ jupyter notebook hoge.ipynb >/dev/null 2>&1 &
    or
    $ jupyter notebook hoge.ipynb 1>/dev/null 2>&1 &

Reference:

-   [How to redirect all output to
    /dev/null](http://stackoverflow.com/questions/18012930/how-to-redirect-all-output-to-dev-null)
