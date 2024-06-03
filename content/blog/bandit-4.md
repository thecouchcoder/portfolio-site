+++
title = 'OverTheWire Bandit 3 Walkthrough'
date = 2024-05-14T21:53:08-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
Find the password in the only human-readable file in the `inhere` directory

# Theory
- `file` will tell us the file type

# Solution
This one starts off a little weird.  What do they mean by human readable file?  Let's start by navigating to the `inhere` directory and seeing what's in it.
```bash
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```
Lots of files! We could check each one manually, but there's got to be a better way.\
Let's try out the file command on one of them.  We need to use our trick we learned earlier about files that start with `-`
```bash
bandit4@bandit:~/inhere$ file ./-file00
./-file00: data
```
With a bit of Googling we find out we can output the file type of all files in the directory
```bash
bandit4@bandit:~/inhere$  file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```
Aha!  An ASCII file! Let's check it out.
```bash
bandit4@bandit:~/inhere$ cat ./-file07
(redacted-password)
```

Success!
