---
title: "Display glitches when using tmux"
Date: 2017-03-02T19:13:28+09:00
Category: misc
Slug: display-glitches-when-using-tmux
Authors: Ryo KOBAYASHI
tags: 
- "mac"
- " linux"
- " tmux"
---

When I was using `tmux` on a remote server in Terminal.app and tried to type something in addition to existing text, sometimes display went wrong not shifting the existing text rather overriding the text.

This seems a problem of setting of `TERM` environment variable of the shell used. So I changed the `TERM` variable from `xterm-color` to `xterm-256color` and the problem was fixed.


