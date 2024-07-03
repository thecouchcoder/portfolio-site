+++
title = 'Bandit 16'
date = 2024-07-02T07:25:13-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

# Theory

- nmap\
- openssl\
- mktemp\
- chmod\

# Solution

Okay, so this level brings together alot of what we've learned already. Let's start by using nmap, to see what ports are even open. With nmap we can actually specify a range of ports to scan.

```bash
nmap -p 31000-32000 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-02 12:27 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00033s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

Great! We just lowered our possible ports to check from 1000 to 5! I figured their must be a way to use nmap to find out if a port uses SSL or not, and a quick Google brought me to [this page](https://nmap.org/nsedoc/scripts/ssl-enum-ciphers.html).

Before running the sample command, I decided to find out what the `-sV` flag does. The man page says `-sV: Probe open ports to determine service/version info`. It still wasn't super clear to me, so I decided to just run my last command with the -sV flag and see what changes.

```bash
nmap -sV -p 31000-32000 localhost

PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

Okay, that was super helpful actually. Now we can see three of these ports are simply running echo, one is running echo with SSL, and one is running SSL with something unknown. I'd bet money that that unknown is what we're looking for.

A quick connect just like we did from on the last level gives us `KEYUPDATE`

```bash
openssl s_client -crlf -connect localhost:31790 -servername localhost
```

I don't completely understand what that means, but a quick Google tells me to add an additional flag. Adding the flag outputs the password in the form of an RSA key.

```bash
openssl s_client -crlf -connect localhost:31790 -servername localhost -ign_eof
```

Next I utilized the `mktemp -d` we learned in [level 12]({{<ref "bandit-12">}}) to create a temporary directory that I could write into, and then copy and pasted this SSH and piped it into a file in my directory and change the permissions.

```bash
mktemp -d
echo "-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----" > /tmp/tmp.D7QmVYzTo9/key.private

chmod 700 /tmp/tmp.D7QmVYzTo9/key.private
```

Finally we can use what we learned in [level 13]({{<ref "bandit-13">}}) to ssh into the next level with our private SSH key and cat the password

```bash
ssh -i /tmp/tmp.D7QmVYzTo9/key.private bandit17@localhost -p 2220
cat ../../etc/bandit_pass/bandit17
```
