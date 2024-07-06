+++
title = 'Bandit 29'
date = 2024-07-06T12:15:54-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.

# Theory

- git

# Solution

Another very easy one if you're familiar enough with git and have worked in software for any period of time.

First we clone the repo just like in the previous level.

```bash
bandit28@bandit:~$ mktemp -d
/tmp/tmp.pYzExtfWcP
bandit28@bandit:~$ cd /tmp/tmp.pYzExtfWcP
bandit28@bandit:/tmp/tmp.pYzExtfWcP$ git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
bandit28@bandit:/tmp/tmp.pYzExtfWcP$ ls
repo
bandit28@bandit:/tmp/tmp.pYzExtfWcP$ cd repo
bandit28@bandit:/tmp/tmp.pYzExtfWcP/repo$ ls
README.md
bandit28@bandit:/tmp/tmp.pYzExtfWcP/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

Now what is the oldest mistake in the book by software engineers? **Committing your passwords**. Let's check out the history of this git repo

```bash
git log
commit ad9a337071c5e3d4509559d36128b38a0e5571f1 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Jun 20 04:07:12 2024 +0000

    fix info leak

commit 229f6001e1ff407bb935b82a94c6749e41a0693e
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Jun 20 04:07:12 2024 +0000

    add missing data

commit ea882192c25642e69627b13179f9fb98f409ed5d
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Jun 20 04:07:12 2024 +0000

    initial commit of README.md
```

`fix info leak` sounds like what we want. You can revert to a previous time in git history using the commit hash. We want the commit BEFORE the leak was fixed.

```bash
git checkout 229f6001e1ff407bb935b82a94c6749e41a0693e
cat README.md
```

Nailed it.
