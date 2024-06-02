+++
title = 'Bandit 7'
date = 2024-06-02T16:34:40-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
The password for the next level is stored in the file data.txt next to the word millionth

# Theory
- grep - print a line that matches a pattern
- | - also known as "pipe", send the output of one command as the input of another

# Solution
Even if you get nothing else out of doing OverTheWire, you should make sure you know how to grep.  Easily, one of the most useful command line commands.  Grep will let you search following a pattern.

In this case we can start by outputing the file
```bash 
cat data.txt
```

That's quite alot of data to sift through, but thankfully grep comes to the rescuse.  Pipe that output of cat into grep and we've got the answer!

```bash
cat data.txt | grep millionth
```