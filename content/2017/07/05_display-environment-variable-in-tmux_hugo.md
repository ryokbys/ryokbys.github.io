---
title: "DISPLAY environment variable in tmux"
Date: 2017-07-05T14:19:06+09:00
Category: misc
Slug: display-environment-variable-in-tmux
Authors: Ryo KOBAYASHI
tags: 
- "tmux"
---

`tmux` is a great command when using remote servers for work. Mainly because once I turn on `tmux` on the remote server, I can recover the session whenever I reconnect the server again. This feature is greater than it sounds. If you have not tried `tmux` yet, and you often work on the remote server, you definitely should use `tmux`.



But when I try to use `gnuplot` or something that needs X11 forwarding through SSH, sometimes you need to change `DISPLAY` environment variable in `tmux` session. Because the `DISPLAY` variable has been set when you turned on the `tmux` session first, and the variable could change when you reconnect to the server, but the variable in the `tmux` session is still the one set before. So when you try to run some X11 session in the `tmux` session, it will fail because of the `DISPLAY` variable of the SSH session and `tmux` session are different.

To solve the problem, the `DISPLAY` environment variable of SSH session should be saved every time you connect with SSH like, and you can set the `DISPLAY` variable using the saved value. You can achieve this by writing the following line in `~/.bashrc`,
```
if [ -z "$TMUX" ]; then
    echo $DISPLAY > ~/.display.txt
fi

function set_display(){
    export DISPLAY=$(cat ~/.display.txt)
}
```

This will save the `DISPLAY` variable to `~/.display.txt` when you connect to the remote server which is not the `tmux` session.
Then you can set the `DISPLAY` variable in the `tmux` session by doing
```
$ set_display
```
It still requires you to run the one line command and not completely automatic. If you really want to make it fully automatic, I think currently it is only possible by resetting the `DISPLAY` variable everywhen you run any command.


### References

- [Unbounded Above](http://alexteichman.com/octo/blog/2014/01/01/x11-forwarding-and-terminal-multiplexers/)

