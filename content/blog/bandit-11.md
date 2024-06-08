+++
title = 'OverTheWire Bandit 11 Walkthrough'
date = 2024-06-07T21:22:44-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

# Theory
- (ROT13)[https://en.wikipedia.org/wiki/ROT13] - essentially a Casear cipher where each letter is rotate 13 characters.  hello world becomes uryyb jbeyq
- tr - translate or delete characters

# Solution
This one was quite interesting.  After reading the linked Wikipedia page, I still wasn't sure where to start, so I began glancing through the man pages of each command listed on the task page.  I settled on `tr` as being the most likely candidate for solving this.  After reading the man page further, I still wasn't positive how to use it so I looked up an example online and then started playing around.

You can convert all letters to uppercase using:
```bash
bandit11@bandit:~$ echo hello world | tr [a-z] [A-Z]
HELLO WORLD
```

Then I tried simple letter exchanges (swapping h and w)
```bash
bandit11@bandit:~$ echo hello world | tr hw wh
wello horld
```

Definitely looks like what we need!
The wiki has the ROT13 cipher that we can copy and even an example of a cipher text, so I tested it out with our new `tr` knowledge.

```bash
bandit11@bandit:~$ echo "Gb trg gb gur bgure fvqr!" | tr NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
To get to the other side!
```

Came out perfect! Now to try it with our key -

```bash
cat data.txt |  tr NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```

Awesome!