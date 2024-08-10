+++
title = ' OverTheWire Bandit27'
date = 2024-07-06T07:14:42-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

Good job getting a shell! Now hurry and grab the password for bandit27!

# Theory

- Everything you learned in 25

# Solution

This one is incredibly easily compare to 25. Start by using the same exploit on the more command you learned in the last level to get a shell.

Once you're in, you can start exploration

```bash
ls -l
```

We see there's a bandit27.do executable. Let's run it and see what happens.

```bash
bandit26@bandit:~$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id
```

You first instinct is probably to run something like this

```bash
./bandit27-do bandit27 "cat /etc/bandit_pass/bandit27"
OR
./bandit27-do bandit27
```

Either way you're going to get `env: ‘bandit27’: Permission denied`. This actually gives us a clue... `env`. This is actually a linux command! We're seeing the output of whatever linux command this executable is running. Let's look up and see how `env` command should be formatted and try that.

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

Well that was easy enough!
