+++
title = 'OverTheWire Bandit 2 Walkthrough'
date = 2024-05-14T21:40:55-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
This time we need to find the password in a file called `spaces in this filename`

# Solution
Another one that's not to bad.  We just need to utilize what we learned in the level 1 and use quotes when we specify the file name.
```bash
bandit2@bandit:~$ cat ./"spaces in this filename"
(redacted-password)
```

Another one done!