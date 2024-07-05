+++
title = 'OverTheWire Bandit 0 Walkthrough'
date = 2024-05-14T21:20:52-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

The password for the next level is stored in a file called readme located in the home directory.

# Theory

- `man` - This is a Linux command that will give you information about any other command your trying to run. It's short for `manual`. Try typing `man ls` and see what happens (you can press q to quit).
- `ls` - lists the contents of the current directory
- `cat` - prints file contents to standard output

# Solution

The first few challenges in Bandit are aimed at familiarizing you with the Linux command line. If you've used Linux before these levels will be a breeze.
You should still be logged in from Level 0\
Since it says the file should be located in this directory let's try looking for it\

```bash
bandit0@bandit:~$ ls
readme
```

Now that we see it's there, let's try printing it's contents\

```bash
bandit0@bandit:~$ cat readme
(redacted-password)
```

Success! We found the password! It's against the rules of the game to share the passwords, so I'll redact it in all output. You can type `exit` to leave the SSH session. Let's move on
