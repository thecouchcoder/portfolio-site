+++
title = 'OverTheWire Bandit 1 Walkthrough'
date = 2024-05-14T21:36:04-05:00
draft = true
+++

# Task
Locate the password in a file called `-`

# Solution
We can begin by using what we learned about `cat` in the last level.
```bash
cat -

```
That definitely didn't work. Click `ctrl+c` if you're stuck.\
So what's going on?  If you read `man cat`, you can see it says that `cat -` will read from standard input.\
Well that's not what we want to do.  So a bit of googling and you'll find that you need to specify the full path.\
```bash
bandit1@bandit:~$ cat ./-
(redacted-password)
```
Success! There's the password! Time for level 2!