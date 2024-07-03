+++
title = 'Bandit 15'
date = 2024-07-02T06:45:34-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

# Theory

[SSL](https://www.youtube.com/watch?v=j9QmMEWmcfo)\
OpenSSL - The openssl program is a command line program for using the various cryptography functions of OpenSSL's crypto library from the shell\
s_client - according to the OpenSSL man page, This implements a generic SSL/TLS client which can establish a transparent connection to a remote server speaking SSL/TLS. It's intended for testing purposes only and provides only rudimentary interface functionality but internally uses mostly all functionality of the OpenSSL ssl library.\
[OpenSSL Cookbook](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/index.html)

# Solution

I started by using our command we learned in the last level to see what was running on this port.

```bash
nmap -p 30001 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-02 11:45 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00011s latency).

PORT      STATE SERVICE
30001/tcp open  pago-services1

```

It looks very similar to the last level, so I decided to connect without SSL (exactly as we did last time), and what happens, but we get absolutely no output. Next, I looked at the resources OTW provided, but I didn't really find them useful for understanding SSL. Instead I went and watched [this video by ByteByteGo on SSL](https://www.youtube.com/watch?v=j9QmMEWmcfo) - Note that TLS is just an improved version of SSL. Now I went back to the OpenSSL Cookbook provided by OTW to figure out how to use OpenSSL. A few pages into the OpenSSL Cookbook, I found the following command for connecting and tried it out

```bash
openssl s_client -crlf -connect localhost:30001 -servername localhost
```

We got a whole bunch of text that looked successful (to understand what it all is, read the OpenSSL Cookbook), so I pasted the current level's key and out came the key for the next level!
