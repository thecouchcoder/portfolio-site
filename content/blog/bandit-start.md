+++
title = 'OverTheWire Bandit'
date = 2024-05-14T21:02:52-05:00
+++

# Introduction

If you have an interest in cybersecurity, you've probably heard of Capture the Flag.  Capture the Flag, or CTF for short, is a cybersecurity challenges to help you improve your skills.  Participants are challenged to find a "flag" (usually a simple text string), which are hidden somewhere on the vulenerable server.  [OverTheWire.org](https://overthewire.org/wargames/) is a website that hosts free challenges that teaches beginners the basics of CTF.  It is organized into different "war games", each with levels that increase in difficulty.  The recommended place to start is with their game called [Bandit](https://overthewire.org/wargames/bandit/), which is aimed at absolute beginners. I have been fascinated by CTF since learning about it in college, so I have decided to finally dive into Bandit. The purpose of these write ups is to help me solidify my knowledge and understanding as I progress.

# Chapters
[Level 0]({{<ref "bandit-0">}})\
[Level 1]({{<ref "bandit-1">}})\
[Level 2]({{<ref "bandit-2">}})\
[Level 3]({{<ref "bandit-3">}})\
[Level 4]({{<ref "bandit-4">}})\
[Level 5]({{<ref "bandit-5">}})\
[Level 6]({{<ref "bandit-6">}})\
[Level 7]({{<ref "bandit-7">}})\
[Level 8]({{<ref "bandit-8">}})\
[Level 9]({{<ref "bandit-9">}})\
[Level 10]({{<ref "bandit-10">}})\
[Level 11]({{<ref "bandit-11">}})\
[Level 12]({{<ref "bandit-12">}})\
[Level 13]({{<ref "bandit-13">}})
<!-- [Level 14]({{<ref "bandit-14">}})
[Level 15]({{<ref "bandit-15">}})
[Level 16]({{<ref "bandit-16">}})
[Level 17]({{<ref "bandit-17">}})
[Level 18]({{<ref "bandit-18">}})
[Level 19]({{<ref "bandit-19">}})
[Level 20]({{<ref "bandit-20">}})
[Level 21]({{<ref "bandit-21">}})
[Level 22]({{<ref "bandit-22">}})
[Level 23]({{<ref "bandit-23">}})
[Level 24]({{<ref "bandit-24">}})
[Level 25]({{<ref "bandit-25">}})
[Level 26]({{<ref "bandit-26">}})
[Level 27]({{<ref "bandit-27">}})
[Level 28]({{<ref "bandit-28">}})
[Level 29]({{<ref "bandit-29">}})
[Level 30]({{<ref "bandit-30">}})
[Level 31]({{<ref "bandit-31">}})
[Level 32]({{<ref "bandit-32">}})
[Level 33]({{<ref "bandit-33">}})
[Level 34]({{<ref "bandit-34">}}) -->

# Task
The goal of this level is for you to log into the game using SSH\
Server: bandit.labs.overthewire.org\
Port: 2220\
Username: bandit0\
Password: bandit0\

# Solution
To start Bandit, you need to login.  They want you to connect via SSH (Secure Shell Protocol), which is a way to remotely connect to another computer SSH is a UNIX program, so to use it on Windows you'll need to install an SSH client like Cygwin or Putty.  I will use WSL since SSH is preinstalled.  Logging in with SSH is as simple as\
`ssh <username>@<server> -p <port>`\
so in our case we can use\
`ssh bandit0@bandit.labs.overthewire.org -p 2220`\
enter the password and congratulations you have beat level 0!

You'll want to make sure you memorize this command because you'll be using it on every subsequent level to login.