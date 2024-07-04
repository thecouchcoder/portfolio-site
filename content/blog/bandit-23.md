+++
title = 'Bandit 23'
date = 2024-07-04T06:51:56-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

# Theory

- Vim
- [Linux File permissions](https://www.cbtnuggets.com/blog/technology/system-admin/deciphering-linux-file-directories-and-permissions-a-guide)
- [chmod](https://www.cbtnuggets.com/blog/technology/system-admin/when-to-use-chmod-vs-chown)

# Solution

Alright, our normal start

```bash
cd ../../
cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

So let's figure out what the script does

```bash
bandit23@bandit:/$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

Okay even if you don't understand any of the code, you can see from the echo statement that it says it's executing and deleting all scripts in a location. Let's try to navigate there.

So there was no bandit23 location in that directory, but I did find a bandit24 directory `/var/spool/bandit24`

Let's try adding a script and see what happens. We will use `vim` to write the script. Just know when you are done writing you can click `esc`, then `:`, which tells vim you are issuing a command as opposed to typing. Then type `wq` for write and quit.

We will mimick our script like the one from the last level since we essentially want the same thing, but we can do away with all the variables and create our own tmp directory first using `mktemp -d` adn write into that directory so our file doesn't get deleted.

```bash
mktemp -d
cd cd /tmp/tmp.z7yOZ49ykk
vim b24pass.sh
```

Now, in vim, type:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tmp.z7yOZ49ykk/pass
```

Then click `esc`, then type `:wq`
Now we just wait 1 minute for our script to be executed by the cron job and run the comannd from last level to figure out the location.

Now we need to copy the file to the cron job directory we found, however if we only copy then nothing happens because we also need to set the correct file permissions. If you're not familiar with linux file permissions and chmod, read the articles linked in the theory section.

```bash
chmod 777 b24pass.sh
chmod 777 /tmp/tmp.z7yOZ49ykk
cp -f b24pass.sh /var/spool/bandit24/foo/b24pass.sh
```

Now wait a minute for the cron job to execute then run this to get the password.

```bash
ls
cat pass
```
