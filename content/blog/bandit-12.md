+++
title = 'OverTheWire Bandit 12 Walkthrough'
date = 2024-06-07T22:15:13-05:00
+++

# Introduction

This post is part of a series walking through OverTheWire Bandit. You many want to start at the [beginning of the series]({{<ref "bandit-start">}}). The posts are my unedited, informal thought process while solving levels.

# Task

The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

# Theory

mktemp - create a temporary file or directory if you use -d\
cp - copies files\
mv - moves file OR RENAMES\
xxd - make a hexdump or do the reverse\
gzip - compress or expand files\
bzip2 - a block-sorting file compressor\
tar - GNU 'tar' saves many files together into a single tape or disk archive, and can restore individual files from the archive.\

# Solution

This one is a the first big challenge, but doable with a little trial and error. First they give you the hint that you need to create your own directory to do some work in. Following the commands is easy enough

```bash
mktemp -d
cp data.txt (whatever your output directory is)
cd (whatever your output directory is)
```

Ok, now we can start by looking at our file `cat data.txt`. So it is in fact a hex dump as they said. When I was looking through the suggested commands on the previous level, one of the ones I looked at was `xxd`, which can reverse a hex dump.

Starting with that we can run

```bash
xxd -r data.txt
```

We get a bunch of output nonsense, but that's because they mentioned it's compressed. Let's output it to a file.

```bash
xxd -r data.txt dump.txt
```

I noticed that one of the new commands they mentioned in this level is `file`. After a quick look at the man page I ran

```bash
file dump.txt
```

This told us it is a gzip compressed file. That's helpful! A quick look at the gzip page, and you can see it also has a `gunzip` command. Note if you try to run that on this `txt` file you will get an error, so we need to rename it first.

```bash
mv dump.txt dump.gz
gunzip dump.gz
cat dump
```

Okay, so now we have more nonsense, but with a quick look with the `file` command again we can see it's because it's compressed again, this time with a different compression algorithm. Once again, there is a simple unzip command for this found on the man page.

```bash
file dump
mv dump dump.bz2
bunzip2 dump.bz2
cat dump
```

Now when you run `file`, you can see it's a `tar` file. The man page shows we can run `tar -xf` to extract files.

```bash
file dump
mv dump dump.tar
tar -xf dump.tar
cat dump
```

Now we just repeat this process a few times until we get our password

```bash
ls
cat data5.bin
tar -xf data5.bin
ls
file data6.bin
mv data6.bin data6.bz2
bunzip data6.bz2
ls
file data6
tar -xf data6
ls
file data8.bin
mv data8.bin data8.gz
gunzip data8.gz
ls
file data8
cat data8
```

FINALLY!!
