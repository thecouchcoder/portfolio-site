+++
title = 'OverTheWire Bandit 5 Walkthrough'
date = 2024-05-14T21:59:53-05:00
draft = true
+++

# Task
Find the password in a file in the `inhere` directory with the following properties\
- human-readable
- 1033 bytes in size
- not executable

# Theory 
- `find` - searches files in a directory hierachy.  There are alot of different options

# Solution
Let's start by using what we learned in the last level
```bash
bandit5@bandit:~$ cd inhere/
bandit5@bandit:~/inhere$ file ./*
./maybehere00: directory
./maybehere01: directory
./maybehere02: directory
./maybehere03: directory
./maybehere04: directory
./maybehere05: directory
./maybehere06: directory
./maybehere07: directory
./maybehere08: directory
./maybehere09: directory
./maybehere10: directory
./maybehere11: directory
./maybehere12: directory
./maybehere13: directory
./maybehere14: directory
./maybehere15: directory
./maybehere16: directory
./maybehere17: directory
./maybehere18: directory
./maybehere19: directory
```

Okay that's problematic.  We have a ton of directories, and if we look further each one has a bunch of files.  We need a better way to search through a bunch of directories at once.\
Enter the find command.  It will search through an entire hierarchy of directories and has alot of different options that can be passed.  We can use the `-executable` flag to search for exectuables in conjunction with the `!` operator to negate it.
```bash
bandit5@bandit:~/inhere$ find ! -executable
```
Unfortunately, this returns alot of results.  Let's try also using the `-size` filter.  We just need to pass the number and then `c` to specify bytes.
```bash
bandit5@bandit:~/inhere$ find ! -executable -size 1033c
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
(redacted-password)
```

That did the trick!
