---
layout: post
title: "Encrypting containers in Linux"
description: "Moving from Truecrypt containers to cryptosetup - why didn't I do this years ago?"
summary: "Truecrypt is dead but Linux has had the ability to make encrypted containers for years.  Time to explore!"
category: "Blog"
tags: [security, encryption, linux]
---
{% include JB/setup %}

#Rationale

You may have heard that Truecrypti is dead.  There is lots of speculation as to why and I won't go into that, suffice to say it is no longer considered safe to use truecrypt to encrypt your hard drive or any device attached to your machine.  What I always loved about truecrypt was its ability to encrypt containers that could then be mounted on multiple systems.  Say you have a USB drive that you want encrypted - easy.  Say you just want part of it encrypted - also easy.  It turns out, you can do the same thing with encrypted containers in Linux - have been able to do so for years!  Let's get started ...

#Necessary packages

You may already have this (do `which cryptsetup` to find out).  If you don't have it, cryptsetup is available in most repos.  On Debian based systems simple apt-get install:

    sudo apt-get install cryptosetup

This will allow you to create and manipulate encrypted containers easily.  The website for cryptsetup is [here](https://code.google.com/p/cryptsetup).

#Make a container

Linux comes with a wonderful little utility called `dd` - it is definitely the kind of thing about which Uncle Ben would advice Peter however - we are talking great power here.  This utility is sometimes referred to as the "disk destroyer" so be careful and know the switches and options you are using!

For creating a container we will make a file and write all zeros to it.  `dd` helps us with this by taking an input file parameter (`if=`), the zeros come from the /dev/zero file.  You next need to tell `dd` where to put the file and what to call it, this is the output file parameter (`of=`), you can tell it wherever you want it to go.  Keep in mind that if you run `dd` as a normal user not only will you be unlikely to totally hose your system but the resulting container will have your user permissions - a handy feature!

    dd if=/dev/zero of=<path to container>/one.bin count=2000k

The above will create a 1 gigabyte file with all zeros and call it `one.bin`.

#Connecting to loopback device

After you make your container you will need to connect it to a loopback device before you can encrypt it.  You can find the loopback devices in your system with the following:

    ls -liha /dev/loop*

This will list a bunch of files that can be used to connect to your container.  The `losetup` command connects loopback files to containers (you may have used it to mount iso files to test distros in the past?  What you aren't a distro hopper?).

    sudo losetup /dev/loop1 <path to container>/one.bin

We attached to loop1 but could have used any of them (my system has 7 possibilities).

#Partitioning the container

Before we can format the container we need to put a partition table on it.  For that there are a number of tools (gparted if you like GUI tools, fdisk if you are a terminal hound).  I will leave figuring out how to put the partition on the loopback device to you but suffice to say if you use fdisk (like the command below) accepting the defaults works fine.

    sudo fdisk /dev/loop1

#Encrypting the partition

Now we finally get to use cryptsetup to encrypt the container (even before there is a filesystem on it!).

    sudo cryptsetup -v -y luksFormat /dev/loop1

The `-v` parameter means "give me verbose output" and can be replaced with `--verbose`.  The `-y` parameter means "verify my passphrase, get it twice" and can be replaced with `--verify-passphrase`.  After the parameters is `luksFormat` - basically you are initializing the LUKS partition.  Be warned - if you run this command on a container that already has a LUKS partition the old one will be completely replaced.  Apparently there is a way to retrieve it (something about header backups) but you are on your own there!

##Opening the device

Now we have to open the device - this is done by opening the LUKS partition and telling the file system to map it to a particular spot.  We need it in a spot in the filesystem so we can later mount it for access.

    sudo cryptsetup luksOpen /dev/loop1 cryptStuff

This will set up a file in `/dev/mapper` that is called `cryptStuff`.  As you likely know, things in the /dev tree are easily mounted into the filesystem (as long as they have a filesystem on them!).  So let's put a filesystem on this bad boy.

#Formatting the encrypted container

You can't write to a non-formated container so let's fix that.  Interestingly, you are supposed to be able to use mkfs as a regular user but I can't on my system.  If I make the filesystem as the root user (using `sudo`) then it will be owned by root and be in the root group.  This may be inconvenient at times so you can specify who the root owner should be by specifying a UID and GID when you make the filesystem.  That is in the extended (`-E`) options for mkfs.

    sudo mkfs.ext4 -j -E root_owner=<UID>:<GID> /dev/mapper/cryptStuff

If you don't know your user UID and GID you can find it in the passwd file like this:

    less /etc/passwd | grep <username>

The output will have something like this (assuming your username is `dude` and why would it not be?):

    dude:x:####:####:dude.........

The `####`'s in the above are the UID and GID respectively.

#Mount the device

Mounting is a simple matter.  You should have a place to mount it:

    mkdir /home/dude/cryptStuff

Then just mount it:

    sudo mount /dev/mapper/cryptStuff /home/dude/cryptStuff

Now you are ready to use.

#What about shutting it down?

This is no big deal if all you need to do is shut it down when you shut down the computer, everything will be handled automatically.  If you want to shut the encrypted container without shutting off the computer just do things in reverse:

    sudo umount /home/dude/cryptStuff
    sudo cryptsetup luksClose /dev/mapper/cryptStuff
    sudo losetup -d /dev/loop1

Nothing to it!

#What about starting it up the next time?

Obviously now the device is made, it has a partition on it and the partition is formated.  All you have to do is connect it to a loop device, map it and mount it:

    sudo losetup /dev/loop1 /home/dude/one.bin
	 sudo cryptsetup luksOpen /dev/loop1 cryptStuff
    sudo mount /dev/mapper/cryptStuff /home/dude/cryptStuff

And voila!

I leaned heavily on the post [here](http://www.linux.org/threads/encrypted-containers-without-truecrypt.4478/) with a few changes and comments to make it clearer to me for future use.  If this isn't clear to you you should check out that post - it is quite good. Alternatively you could take a chance that I could help and leave a comment, I would love to hear from you if this tutorial was helpful or confusing.
