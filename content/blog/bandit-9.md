+++
title = 'OverTheWire Bandit 9 Walkthrough'
date = 2024-06-02T21:41:26-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

# Theory
- strings - print the strings of printable characters in files

# Solution
Starting with `cat data.txt`, we can see there's a lot of nonsensical characters in the file.  After learning about the `strings` command, we can combine that with what we learned in the past few levels to come up with this command.

```bash
cat data.txt | strings | grep ==
```

Since the prompt said the password would be preceded by serveral `=` signs, we were able to use that to narrow down the human readable strings, and successfully find the password!