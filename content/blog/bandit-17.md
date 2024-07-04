+++
title = 'OverTheWire Bandit 17 Walkthrough'
date = 2024-07-02T20:41:03-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

# Theory

- diff - compare files line by line

# Solution

A surprisingly easy one after the Level 16. Simply run a diff and you get the password.

```bash
diff passwords.new passwords.old
```
