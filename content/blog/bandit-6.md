+++
title = 'OverTheWire Bandit 5 Walkthrough'
date = 2024-05-14T22:28:23-05:00
+++

# Introduction
This post is part of a series walking through OverTheWire Bandit.  You many want to start at the [beginning of the series]({{<ref "bandit-start">}}).  The posts are my unedited, informal thought process while solving levels. 

# Task
Find the password **somewhere on the server** with the following properties\
- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

# Theory
- pwd - print the name of the current directory
- /dev/null - anything written to here will be discarded

# Solution
Let's start out by seeing what we have to work with and then using our find command from the last level
```bash
bandit6@bandit:~$ ls -a
.  ..  .bash_logout  .bashrc  .profile
bandit6@bandit:~$ find -size 33c
bandit6@bandit:~$
```

Nothing... they did mention explicitly that the password would be **somewhere on the server**\
Let's try navigating **backwards** and figuring out where we are.
```bash
bandit6@bandit:~$ cd ../
bandit6@bandit:/home$ ls
bandit0   bandit15  bandit21  bandit27-git  bandit30-git  bandit6    drifter12  drifter5     formulaone2  krypton4
bandit1   bandit16  bandit22  bandit28      bandit31      bandit7    drifter13  drifter6     formulaone3  krypton5
bandit10  bandit17  bandit23  bandit28-git  bandit31-git  bandit8    drifter14  drifter7     formulaone5  krypton6
bandit11  bandit18  bandit24  bandit29      bandit32      bandit9    drifter15  drifter8     formulaone6  krypton7
bandit12  bandit19  bandit25  bandit29-git  bandit33      drifter0   drifter2   drifter9     krypton1     ubuntu
bandit13  bandit2   bandit26  bandit3       bandit4       drifter1   drifter3   formulaone0  krypton2
bandit14  bandit20  bandit27  bandit30      bandit5       drifter10  drifter4   formulaone1  krypton3
bandit6@bandit:/home$ pwd
/home
bandit6@bandit:/home$ cd ../
bandit6@bandit:/$ pwd
/
```

Okay great! Navigating back shows we have alot of places to search. Let's rerun our find command, but this time specifying some additional flags
```bash
bandit6@bandit:/$ find -group bandit6 -user bandit7 -size 33c
```
Yikes! Lot's of permission denied...\
We can utilize /dev/null to get rid of these errors.  Linux commands have 2 streams
1. Standard output
2. Standard error
We can access them using
`1>stdout.txt` which will write whatever would be output on standard out to the text file stdout.txt

Let's utilize this info to get rid of the permission errors
```bash
bandit6@bandit:/$ find -group bandit6 -user bandit7 -size 33c 2>/dev/null
./var/lib/dpkg/info/bandit7.password
```

Now we've found the password.  Let's keep going