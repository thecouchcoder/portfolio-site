+++
title = 'Bandit 26'
date = 2024-07-05T06:54:10-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

# Theory

- Linux account info
- more - display the contents of a file in a terminal
- Vim
- Must be smarter than me (not hard lol)

# Solution

This one is incredibly difficult and I had to ask for help on the discord and eventually still looked up a walkthrough. Let's go through it

When we first log into the level we see we are given a private key. Easy enough, take that private key, save it on your computer somewhere and then log into bandit26 with it. I am using WSL, so my commands will look the same as a linux system.

```bash
mktemp -d
cd /tmp/tmp.Fnvm30H0RI
echo "-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----" > bandit26.sshkey
chmod 700 bandit26.sshkey
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

Okay, we're kick out immediately, but if you scroll up you can see all the normal text plus some special ASCII art.

I tried the same technique from [Level 18]({{<ref "bandit-18">}}), but it gets us stuck at the ASCII art.

```bash
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220 "pwd"
```

I tried a few more things unsuccessfuly, and this is where I started asking some question on discord. The tip they gave me was to find out as much about bandit26 from bandit25 login as possible and asked me some leading questions to help me get there. This is where I discovered the `/etc/passwd` folder where linux stores account info.

```bash
cd ../../
cat /etc/passwd | grep bandit25
bandit25:x:11025:11025:bandit level 25:/home/bandit25:/bin/bash

cat /etc/passwd | grep bandit26

bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

Comparing bandit25 entry vs bandit26 entry we can see the last bit of text is the shell. Bandit25 is using bash, but bandit26 is using something at `usr/bin/showtext`. You can read more about the format [here](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/) if you're interested, but we don't need it. Let's check it out

```bash
cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

Okay interesting. Instead of bash they are running a script, if sets the terminal to linux, then uses the `more` command to display text.txt, and then exits. So the exit is why we're logged out immediately. My first thought was to just change this script to not include the exit, but I don't have write permissions. Then I navigated to `/home/bandit26` thinking I could change `~/text/txt` to print the password somehow, but again, no write permissions. I checked with the bandit team on discord and they confirmed that changing the scripts is NOT how to solve the level. Instead I need to look exploit at the `more` command.

`more` is an alternative to `cat` for displaying text on the screen. It's especially helpful for large amounts of text, because `cat` would dump it all at once and the window would zoom past the beginning, `more` instead shows one screenful at a time and allows us to page through it. I decided to log back into bandit26 and try to use the navigation command `space` to page through what was being show, but unfortunately it didn't work (I should have realized it wouldn't since it logs us out, meaning more is done executing). This is where I got really stuck and looked up a walk through, so let's go through it together now.

The reason we can't page through more is because all the text is immediately displayed. If we shrink our window to be VERY tiny, it will instead enter into interactive mode. Try it out by sshing into bandit26 but from a very tiny terminal window. You should immediately see something like `--More--(16%)` at the bottom, showing that you're interacting with more. This is great because it means we didn't exit! You can expand the window again now.

Now that we're in more, you can read the interactive commands and figure out what to do, but it didn't click for me, so I looked it up. We need to enter vim by hitting `v`.

Okay! Now we're in a text editor - but how does that even help us? It's because Vim is a quite powerful text editor. It can actually interact with the shell. Okay the first thing we need to do is change our shell so it's NOT that script.

Click `esc`+`:` then type `set shell=/bin/bash`
Then run `esc`+`:` and type `sh` and we have a shell!

We can simply do

```bash
bandit26@bandit:~$ pwd
/home/bandit26
bandit26@bandit:~$ cat ../../etc/bandit_pass/bandit26
```

and we have the password! I told you that you needed to be smarter than me because that was so tough!
