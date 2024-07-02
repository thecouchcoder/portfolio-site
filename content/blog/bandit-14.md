+++
title = 'OverTheWire Bandit 14 Walkthrough'
date = 2024-07-01T07:41:02-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

# Theory

SSH
Public/Private Key Encription

# Solution

This is another tough one. We know where the password is, but we can't read it. There is a private SSH Key we can use to log into the next level instead.

You should be familiar with SSH, since that's how we've logged into every previous level. Bandit links us to a helpful article that goes into alot more detail about SSH and talks about how there is actually 2 ways to login into SSH - password or public/private key. If you're not familiar with public/private key encryption, now is probably a good time to go watch a video or find an article explaining it. I've worked with it in the past, so I was familiar already.

So one thing we learn from the article and the man page, is that SSH is going to look for our private key at ~/.ssh . When we log into Bandit 13 we see there is a private key in our current directory `/home/bandit13`. I started out by trying to navigate to the `.ssh` directory, but it doesn't exist and when you try to create it, you don't have permissions. So I did a quick Google to see if it's possible to specify a different location for where you private key will be, and it turns out that's exactly what the `-i` flag does.

I was fairly confident I could SSH in now, so I entered

```bash
ssh -i sshkey.private bandit14@localhost

bandit14@localhost: Permission denied (publickey).
```

But as you can see, I was denied. I got stuck here and couldn't figure out why. I was confident I was SSHing in correctly, but eventually I came across an old post where someone else was having the same exact issue with this level and the responder pointed out that Bandit exposes SSH over port 2220 isntead of the default 22. I should have known that because every level logged into so far has used `-p 2220` as an option.

So trying again

```bash
ssh -i sshkey.private bandit14@localhost -p 2220
cat ../../etc/bandit_pass/bandit14
```

Just like that, since we successfully SSH-d into the server, we can cat the password from the location we know it's stored and we're done with this level.
