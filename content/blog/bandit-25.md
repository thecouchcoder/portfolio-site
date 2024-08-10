+++
title = ' OverTheWire Bandit25'
date = 2024-07-04T12:12:37-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

# Theory

more

# Solution

Okay so we're dropped into a server with very little direction. I started out by running a few different commands

```bash
bandit25@bandit:~$ ls
bandit26.sshkey
bandit25@bandit:~$ pwd
/home/bandit25
bandit25@bandit:~$ id
uid=11025(bandit25) gid=11025(bandit25) groups=11025(bandit25)
```

Okay since we have a private ssh key, let's try logging in.

```bash
ssh -i bandit26.sshkey bandit26@localhost -p 2220
```

We get in, but it immediately kicks us out. We've seen this on a previous level, so let's try getting the password using cat. I'm starting to realize what they mean by the directory isn't `bin/bash`. We need to figure out where we are so we can cat the file.

```bash
ssh -i bandit26.sshkey bandit26@localhost -p 2220 "pwd"
```
