+++
title = 'Bandit 30'
date = 2024-07-06T12:21:34-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.

Clone the repository and find the password for the next level.

# Theory

- git

# Solution

Alright same start as always

```bash
bandit29@bandit:~$ mktemp -d
/tmp/tmp.7p90HITnVt
bandit29@bandit:~$ cd /tmp/tmp.7p90HITnVt
bandit29@bandit:/tmp/tmp.7p90HITnVt$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
```

Checking `git log` shows our trick from last time won't work again. Again, this is just intuition since I've spent alot of time working with git, I decided to check if there were any more branches for the repository

```bash
git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

What do you know there are. The one called sploits-dev looks kind of interesting.

```bash
git switch sploits-dev
```

That lead to a dead end, but when I read the README again something clicked... No passwords **in production**! Quickly switched to dev

```bash
git switch dev
cat README.md
```

Got the password
