+++
title = 'Bandit 20'
date = 2024-07-03T19:38:30-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

# Theory

- [tmux](https://www.youtube.com/watch?v=nTqu6w2wc68)
- netcat

# Solution

This is a fun one. Essentially, there is a binary we can run connect to a port and expects that whatever is running their to send back the current level's password. If it does, it will send the next level's password.

So we know that we need to expose a server on an arbitrary port that returns the current level's password. Then we simply need to run the binary in the home directory. We can accomplish this with `nc`. If you look at its man page you can see how to create a simple client server by running
`nc -l 1234`
and
`nc -N localhost 1234`

These 2 command will create a basic chat application. The problem though, if you can only run one command at a time - so if you start the server, how are you supposed to start the client? This is where _tmux_ comes in. I've provided a great video in the theory section showing how to use tmux, but essentially it allows you create multiple panes to for your terminal so you can run more than 1 command (it also does other things, but this is the piece we're interested in).

With this new tmux knowledge, I created a 3 pane terminal - One for the server, one for the client and one for the nc man page.

A quick Google told me I can run commands on an nc server using the `|`, so to start the server I ran

```bash
echo "password-here" │             $ nc -l 1234
| nc -l 1234
```

Then for the client, I simply connected to that port with the binary

```bash
./suconnect 1234
```

Here's a screenshot of my tmux setup.
![tmux](/images/bandit-20-tmux.png)
