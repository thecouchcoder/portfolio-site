+++
title = ' OverTheWire Bandit31'
date = 2024-07-06T12:36:13-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.

Clone the repository and find the password for the next level.

# Theory

- git
- [git internals](https://git-scm.com/book/en/v2/Git-Internals-Git-References)

# Solution

Once again, nothing new about our start.

```bash
bandit30@bandit:~$ mk temp -d
mk: command not found
bandit30@bandit:~$ mktemp -d
/tmp/tmp.eFsGUX9kcR
bandit30@bandit:~$ cd /tmp/tmp.eFsGUX9kcR
bandit30@bandit:/tmp/tmp.eFsGUX9kcR$ git clone  ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
```

This time the log doesn't reveal anything interesting and there also aren't any remote branches to explore. Let's use what we learned at the beginning of bandit to look for hidden files

```bash
bandit30@bandit:/tmp/tmp.eFsGUX9kcR/repo$ ls -la
total 16
drwxrwxr-x 3 bandit30 bandit30 4096 Jul  6 17:37 .
drwx------ 3 bandit30 bandit30 4096 Jul  6 17:37 ..
drwxrwxr-x 8 bandit30 bandit30 4096 Jul  6 17:37 .git
-rw-rw-r-- 1 bandit30 bandit30   30 Jul  6 17:37 README.md
```

This reveals a hidden folder called `.git` that stores all the internals of a git repo. Now is a great time to stop and read about [git internals](https://git-scm.com/book/en/v2/Git-Internals-Git-References) because we are going to need it.

Here, I just started looking through all the files at the top level until I came to

```bash
bandit30@bandit:/tmp/tmp.eFsGUX9kcR/repo/.git$ cat packed-refs
# pack-refs with: peeled fully-peeled sorted
49ebc0513539a56d3626f3121ff4e72585064047 refs/remotes/origin/master
84368f3a7ee06ac993ed579e34b8bd144afad351 refs/tags/secret
```

`refs/tags/secret` looks VERY interesting. Another way to find this would have been to run `git tag`. Running the command from the git internals documentation outputs the password

```bash
git cat-file -p 84368f3a7ee06ac993ed579e34b8bd144afad351
```
