+++
title = 'OverTheWire Bandit 18 Walkthrough'
date = 2024-07-02T20:52:54-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

# Theory

- ssh

# Solution

Another very easy one. Trying to login immediately logs us out, just like the the task mentioned. So I decided to find out if there's a way to send a command to execute with the ssh command. I found this [post](https://stackoverflow.com/questions/18522647/run-ssh-and-immediately-execute-command), then ran

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```
