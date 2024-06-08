+++
title = 'OverTheWire Bandit 10 Walkthrough'
date = 2024-06-07T20:57:06-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
The password for the next level is stored in the file data.txt, which contains base64 encoded data

# Theory
- base64 - base64 encode/decode data and print to standard output

# Solution
This one is quite easy.  A quick look at the base64 man page shows we just need to use `-d` to decode.

```bash
base64 -d data.txt
```

We've got the flag and can keep moving.