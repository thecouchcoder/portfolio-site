+++
title = 'OverTheWire Bandit 22 Walkthrough'
date = 2024-07-03T20:53:41-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

# Theory

# Solution

We can start this on in the same way as last time

```bash
cd ../../
cat /etc/cron.d/cronjob_bandit23
bandit22@bandit:/$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

When we cat the file it points us to, we can see it's going to take our username `bandit22` from the whoami command, turn that into a string using md5sum, and then copy the password into the /tmp/file at that string and print the name fo the temp file.

```bash
bandit22@bandit:/$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:/$ /usr/bin/cronjob_bandit23.sh
Copying passwordfile /etc/bandit_pass/bandit22 to /tmp/8169b67bd894ddbb4412f91573b38db3
```

When we cat the created file though, we just get the password for the current level.

```bash
bandit22@bandit:/$ cat /tmp/8169b67bd894ddbb4412f91573b38db3
```

What went wrong? Well looking at the script again
`echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"`
and $myname is `myname=$(whoami)`, so we're copying the bandit22 password. We need to copy bandit23!

So how can we do it? Well we know from the task that this is already being executed as a cron job. So technically we don't need to run it, we just need to figure out the name of the file that the password is being saved in. Looking at the script again

```bash
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
```

We could just generate $mytarget with bandit23 hardcoded instead of using the $myname variable.

So to get the password just use

```bash
bandit22@bandit:/$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```
