+++
title = 'Bandit 33'
date = 2024-07-08T06:59:41-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

After all this git stuff, it’s time for another escape. Good luck!

# Theory

# Solution

We've made it to final level!

Upon logging in we're greeted by `WELCOME TO THE UPPERCASE SHELL` and every command I type gets `Permission denied`. I headed back to level 32, to do some exploration from there.

```bash
bandit31@bandit:~$ cd ../../
bandit31@bandit:/$ cat /etc/passwd | grep bandit32
bandit32:x:11032:11032:bandit level 32:/home/bandit32:/home/bandit32/uppershell
bandit31@bandit:/$
```

okay, similar to Level 26, our shell is not /bin/bash, but something else.

When trying to read that file, we get permission denied

```bash
bandit31@bandit:/$ cat /home/bandit32/uppershell
cat: /home/bandit32/uppershell: Permission denied
bandit31@bandit:/$ ls -l /home/bandit32
total 16
-rwsr-x--- 1 bandit33 bandit32 15136 Jun 20 04:07 uppershell
```

The group has read and execute permissions, but others do not. My assumption was we need some command we can run that only involves uppercase characters. I explored the `/bin` folder to see if there are any uppercase only commands we could use and I found `NF`. It lead to a dead end, but it did confirm that if we have an all upercase command it works in this shell. I went down a rabbit hole trying to figure out how I could write my own custom linux command that was all uppercase, but I was never able to find a way to save it somewhere where bandit 33 would be able to execute it, since I don't have root access. This is where I got stuck and had to get a hint.

With a hint, it becomes very easy. Apparently on Linux, if you type $0 it will give you a shell. So this becomes as simple as

```bash
$0
cat ../../bandit_pass/bandit32
```

Congratulations, you have beat Bandit!
