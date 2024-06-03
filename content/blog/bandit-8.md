+++
title = 'OverTheWire Bandit 8 Walkthrough'
date = 2024-06-02T16:44:46-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

# Theory
- uniq - report or omit repeated lines
- sort - sort lines of a text file

# Solution
Admittedly, these two commands are both new to me.  Luckily, they are mentioned on the task page.  Uniq immediately popped out at me, and after looking at the man page I tried running

```bash
uniq
```

However, no luck.  It just printed a bunch of strings.  Back on the man page I realized the -u option would only print unique strings.  I was sure this was it!

```bash
uniq -u
```

Rats! Wrong again.  Then I read it more closely - **filter adjacent matching lines from input**.  Oh, that's the problem.  It's not sorted.  Well back on the task page, a command called sort popped out.  Luckily a quick look at the man page shows this one is pretty simple.

```bash
sort data.txt | uniq -u
```

Another one down!
