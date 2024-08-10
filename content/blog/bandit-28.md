+++
title = ' OverTheWire Bandit28'
date = 2024-07-06T07:29:47-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.

Clone the repository and find the password for the next level.

# Theory

- git

# Solution

Another easy one if you're familiar with git.

You'll need to start by creating a temp directory to clone into

```bash
bandit27@bandit:~$ mktemp -d
/tmp/tmp.AspgpDPLSY
bandit27@bandit:~$ cd /tmp/tmp.AspgpDPLSY
```

Now I wasn't sure how to specify a port with my git path so I decided to find out from the man page

```bash
man git clone | grep port
```

This shows that you specify it with a colon right after the `host` and before the `repo path`. Let's try it.

```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```

Nice it worked. Now we just need to grab the password

```bash
cd /repo
cat README
```
