---
layout: post
title: "Managing Sensitive Data on Mac OS X"
alias: /2005/09/managing-sensitive-data-on-mac-os-x.html
categories:
---
One of the clients I'm working for at the moment mandates -- quite rightly so -- that any data taken offsite must be encrypted. Mac OS X provides two ways to achieve this: FileVault; and Encrypted Disk Images. _(If you want to know how to encrypt emails, see [this article]({% post_url 2005-08-24-pgp-for-mac-mail %}).)_

FileVault (**System Preferences**&gt;**Security**&gt;**Turn On FileVault...**) is kinda cool: It allows you to encrypt your entire home directory. This might be handy if you're super paranoid but I'm not; neither do I want the overhead of having everything I write to disk encrypted.

Encrypted Disk Images on the other hand are super cool for this kind of thing. If you're familiar with Mac OSX then you've no doubt used plain disk images before. They're the `.dmg` files that are used for distributing most of the software you install. The neat thing about them is that you can mount a disk image and use it just like any other disk on your machine. On top of this, you can encyrpt them so that unless you have the password, no one can peek at the data.

To create a blank disk image:

* Open **Applications**&gt;**Utilities**&gt;**Disk Utility**
* Select **File**&gt;**New**&gt;**Blank Disk Image...**

To create an encypted disk image:

* Open **Applications**&gt;**Utilities**&gt;**Disk Utility**
* Select **File**&gt;**New**&gt;**Disk Image From Folder...**
* Choose the folder you wish to use as the source of your disk image; and
* Press **Image**

In either case:

* Enter a filename and location and choose an initial size -- disk images dynamically resize so don't worry too much about this
* Select **AES-128 (recommended)** for **Encryption**; and</li> * Press **Create**

You'll then be prompted for a password with which to encrypt -- and decrypt -- the file. You're also given the opportunity to save the password in your keychain. This just means you won't have to remember the password when mounting it on your own machine -- If you move the file to another machine or send it to someone else, a password must be entered manually.

Now anytime someone tries to mount the disk image, they'll be prompted for a password; if they don't have the password the data is protected, even if they try to read it using another tool.

Once mounted, you can use it as you would any other drive: as a mounted image in Finder; or under `/Volumes/` using the filesystem.
