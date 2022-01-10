---
title: "macOS: X11 forwarding request failed"
Date: 2018-12-06T16:38:52+09:00
Category: misc
Slug: x11-forwarding-request-failed
Authors: Ryo KOBAYASHI
tags: 
- "ssh"
---

When I tried to connect to macOS Mojave machine from other machine, I got the following message.
```bash
X11 forwarding request failed on channel 0
```
And when using `-v` option, I got
```bash
debug1: Remote: No xauth program; cannot forward X11.
X11 forwarding request failed on channel 0
```
So the problem seemed to be an issue of `xauth`-related, and actually this was because the remote server could not find the `xauth` program, even though I do not know the reason why..
I add a line `XAuthLocation /opt/X11/bin/xauth` to `/etc/ssh/sshd_config` on the remote server, to become as follows.
```
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost no
XAuthLocation /opt/X11/bin/xauth
```

Problem solved.

References

- https://qiita.com/yabeenico/items/61ff3e64ee95e8d9156d
- https://blog.n-z.jp/blog/2018-02-01-xquartz-xauth.html
