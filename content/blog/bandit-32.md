+++
title = 'Bandit 32'
date = 2024-07-07T07:27:21-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.

Clone the repository and find the password for the next level.

# Theory

- git
- [git internals](https://git-scm.com/book/en/v2/Git-Internals-Git-References)

# Solution

Will start the same as the previous few levels

```bash
bandit31@bandit:~$ mktemp -d
/tmp/tmp.EzhaB5bKwO
bandit31@bandit:~$ cd /tmp/tmp.EzhaB5bKwO
bandit31@bandit:/tmp/tmp.EzhaB5bKwO$ git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
bandit31@bandit:/tmp/tmp.EzhaB5bKwO$ ls
repo
bandit31@bandit:/tmp/tmp.EzhaB5bKwO$ cd repo/
bandit31@bandit:/tmp/tmp.EzhaB5bKwO/repo$ ls
README.md
bandit31@bandit:/tmp/tmp.EzhaB5bKwO/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

Okay so it tells us what to do this time.

```bash
bandit31@bandit:/tmp/tmp.EzhaB5bKwO/repo$ echo "May I come in?" > key.txt
```

Next I ran `git status` before committing and noticed there was nothing being tracked. My immediate thought was that it must be ignored in the .gitignore. I ran this

```bash
bandit31@bandit:/tmp/tmp.EzhaB5bKwO/repo$ ls -a
.  ..  .git  .gitignore  key.txt  README.md

bandit31@bandit:/tmp/tmp.EzhaB5bKwO/repo$ nano .ignore
```

and removed the line for `\*.txt`

Now go through the steps to commit again

```bash
git status
git add .
git commit -m "add key"
git push origin master
```

The commit failed, but only because it was designed to do so. The pre commit message gives us the password if we've done everything correctly.
