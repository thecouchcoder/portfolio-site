+++
title = 'OverTheWire Bandit 3 Walkthrough'
date = 2024-05-14T21:46:44-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
Find the password in the hidden file in the directory called `inhere`

# Theory
- `cd` allows us to navigate between directories
- `ls` has a flag `-a` to show files that start with `.`

# Solution
Let's start by getting a lay of the land
```bash
bandit3@bandit:~$ ls
inhere
```
Okay, there's the directory we're looking for.  Let's use `cd` to navigate to it.
```bash
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$
```
Hmmm okay - we're in the `inhere` directory, but there's no contents.  Well they did mention the file is hidden.\
Let's try looking for hidden files
```bash
bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden
```
Ahh found it.  Now to grab the key

```bash
cat .hidden
(redacted-password)
```
Bingo