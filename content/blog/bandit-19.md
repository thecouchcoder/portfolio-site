+++
title = 'OverTheWire Bandit 19 Walkthrough'
date = 2024-07-03T06:54:34-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

# Theory

setuid - I found [this link](https://www.cbtnuggets.com/blog/technology/system-admin/linux-file-permissions-understanding-setuid-setgid-and-the-sticky-bit) much more helpful in understanding it

# Solution

This one is simple, but took me way too long. I confused the home directory and the root directory and was looking in the wrong place for the binary. Stay in the same directory that you initially logged into. Look at what is there and then try to run it.

```bash
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id

```

Now run with the command to cat the password

```bash
./bandit20-do cat ../../etc/bandit_pass/bandit20
```
