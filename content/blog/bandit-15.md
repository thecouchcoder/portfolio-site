+++
title = 'OverTheWire Bandit 15 Walkthrough'
date = 2024-07-01T21:44:15-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

# Theory

- `nmap` - Network exploration tool and security / port scanner

# Solution

Start by going through the provided resources, since they are a really good introduction to basic computer networking principles. When I saw the task I immediately knew I was going to need nmap, because I have used it before to scan my network in the past. We need to use nmap to figure out what protocol is being exposed on port 30000 and then use one of the other listed commands to actually connect. After looking at the man page I ran

```
nmap -p 30000 localhost
```

Nmap shows that the port is running TCP. The most interesting part of the man page for `nc` (which is used for TCP connections) is

```
It is quite simple to build a very basic client/server model using nc.  On one console, start nc  listen‐
       ing on a specific port for a connection.  For example:

             $ nc -l 1234

       nc is now listening on port 1234 for a connection.  On a second console (or a second machine), connect to
       the machine and port being listened on:

             $ nc -N 127.0.0.1 1234

       There  should  now  be a connection between the ports.  Anything typed at the second console will be con‐
       catenated to the first, and vice-versa.
```

So next I ran
`nc localhost 30000`

This gave an empty line, which I entered this level's password into and out came the next levels password.
