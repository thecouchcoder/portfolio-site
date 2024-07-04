+++
title = 'Bandit 21'
date = 2024-07-03T20:38:25-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

# Theory

# Solution

First we start by naviating to the directory mentioned and seeing what's in the level 22 file

```bash
cd ../../etc/cron.d
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

Okay so we can see it's running a script, so let's see what that is

```bash
cd ../../
cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Okay perfect we can see it's putting the bandit22 password into a file in the temp directory. Now just cat that to get the password

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
