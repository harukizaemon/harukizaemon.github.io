---
layout: post
title: "Integrating with MacPorts"
alias: /2007/02/integrating-with-macports.html
categories:
---
When I used to run [FreeBSD](http://www.freebsd.org/), I loved the [ports](http://www.freebsd.org/ports/index.html) system that comes as the standard mechanism for installing _most_ freely available (and some commercial) software. When switched to the Mac, I immediately starting using <strike>DarwinPorts</strike>[MacPorts](http://trac.macports.org/projects/macports/wiki) as my main method for obtaining and installing all my "geekware": [PostgreSQL](http://www.postgresql.org); [Subversion](http://subversion.tigris.org/); [Ruby](http://www.ruby-lang.org/); and even [Ant](http://ant.apache.org/) to name but a few.

Installing a new bit of software using MacPorts is usually pretty trivial. Something like:

```
sudo port install subversion
```

is all that's needed to install Subversion and all its dependencies.

Sure, there are Mac-specific GUI-based installers for some software that I use but I generally find it easier to use MacPorts for managing dependencies, versioning, updates, etc.

There are however, other command-line based software such as [Pulse](http://www.zutubi.com/products/pulse/) that, although relatively simple to install, still take some time and thinking not only to install but to install in such a way as to not turn my Mac into a glorified Windoze machine with all those lovely security holes. If only there was a way to create a MacPort installation for these too.

Well, there is: Create one yourself. So this morning I decided to see how difficult it would be to do just that.

Reading the [quick-start guide](http://darwinports.opendarwin.org/docs/pt02.html), I soon learned that writing a _Portfile_ -- Literally a [TCL](http://www.tcl.tk/) script named `Portfile` -- is often all you need to do. Unfortunately, the "quick" in "quick-start" is pretty much just that, leaving quite a lot to the imagination.

So, after reading a few existing Portfiles and following a bit of experimentation, I managed to get a relatively clean (and importantly, working) Portfile for installing Pulse which I figured I'd not only share with you but I would use as a guide to creating your own.

Now, before we continue, if you haven't already done so, I highly recommend reading the [quick-start guide](http://darwinports.opendarwin.org/docs/pt02.html). It's ok, I'll wait...

The start of a Portfile looks something like this:

```
PortSystem 1.0name              pulseversion           1.2.14categories        javamaintainers       haruki_zaemon@mac.comdescription       Pulse automated build serverlong_description  Pulse is an automated build or continuous integration server. \Pulse regularly checks out your project's source code from your \SCM, builds the project and reports on the results. A project \build typically involves compiling the source code and running \tests to ensure the quality of the code. By automating this \process, pulse allows you to constantly monitor the health of \your project.homepage          http://www.zutubi.com/products/pulse/master_sites      http://www.zutubi.com/download/
```

All pretty self explanatory really: The name and version of the software; what exactly it is and why I'd want to use it; who to blame when something goes wrong; and where to download the installation (in this case .tar.gz) file from.

Following on, we have:

```
checksums         md5 dcebf03b7a7099a476371b8142ba7624depends_lib       bin:java:kaffe
```

These specify the MD5 checksum for the installation file to ensue we download the correct one, and any dependencies; in this case we need a Java runtime which comes pre-installed on most Macs anyway but I figured it doesn't hurt to make sure.

Next, we set up some variables for use later on in the script, again all pretty self explanatory:

```
set pulseuser     pulseset pulsegroup    pulseset home          ${prefix}/share/java/${name}set bin           ${home}/binset executable    ${bin}/pulseset dbdir         ${prefix}/var/db/${name}
```

(`${prefix}` is pre-defined by the port system as the path to the base of the installation -- usually `/opt/local`.)

Now for something kinda cool: `launchctl` integration. If you don't know, `launchctl` is the preferred method for starting, stopping and otherwise controlling server processes under OS X. It can be a little tricky to get right with various XML configuration files, etc. however MacPorts comes to the rescue. With a few simple TCL commands, the installer will magically do all the heavy-lifting for us:

```
startupitem.create  yesstartupitem.init    "PULSE_HOME=${home}"startupitem.start   "su ${pulseuser} -c \"${executable} start\""startupitem.stop    "su ${pulseuser} -c \"${executable} shutdown\""
```

Neat huh? Now when we install the port, we'll automagically have scripts generated in all the right places so the pulse server can be started and stopped by the system.

We're almost up to the installation part but before we get to that, we need to stub out a few things.

You see, the whole installation process caters not only for unpacking and deploying of files but in the case of C/C++, etc. also configuring, patching and compiling, etc. In our case though, not only are these latter steps unnecessary, they're not even possible -- we're dealing with a binary distribution -- so we need to override them to do nothing:

```
configure {}build {}
```

At last we get to the actual deployment of the files, creation of users, setting of permissions, etc.:

```
destroot {# Create the Pulse useraddgroup ${pulsegroup}set gid [existsgroup ${pulsegroup}]adduser ${pulseuser} shell=/bin/sh gid=${gid} home=${dbdir} realname=Pulse\ Server# Ensure we have the needed directoriesxinstall -m 755 -d ${destroot}${home}# Copy the filessystem "cp -R ${worksrcpath}/ ${destroot}${home}"# Keep empty directoriesdestroot.keepdirs-append ${destroot}${home}/logs ${destroot}${home}/versions# Fix ownership of some directories pulse really needs to write tosystem "chown -R ${osuser}:${osgroup} ${destroot}${home}/logs"system "chown -R ${osuser}:${osgroup} ${destroot}${home}/versions"# Add a symlink from bin directory to the pulse scriptsystem "ln -fs ${executable} ${destroot}${prefix}/bin/pulse"}
```

Once again, pretty straight forward except for that odd looking `${destroot}`. Why is it being used in some cases and not in others you may ask.

This actually got me for a while to until I realised that what the `destroot` step actually does is create an "image" of the installed software in a working directory -- `${destroot}` -- after which the predefined install step copies the staged files into their final destination. Not a bad idea really when you think about it. Certainly saves screwing up the production environment when something goes horribly wrong -- like when you're playing around with things to see what happens ;-).

Last but not least, a little bit of post-installation goodness to instruct the user with any manual (or perhaps optional) processes, or even just a nice message to say "why thanks for using our software":

```
post-install {ui_msg "#"ui_msg "# The script ${executable} has been installed to facilitate starting and"ui_msg "# stopping ${name} as a true daemon process. It must be run as ${osuser}."ui_msg "# For example:"ui_msg "#"ui_msg "#   sudo su pulse -c "${executable} start"ui_msg "#"ui_msg "# This script assumes it is run from ${home}. To run from outside this"ui_msg "# directory, you must set the value of PULSE_HOME to the absolute path"ui_msg "# of this directory. For example:"ui_msg "#"ui_msg "#   PULSE_HOME=${home} sudo su pulse -c "${executable} start"ui_msg "#"ui_msg "# You will also need To create the directory ${dbdir} if it does not"ui_msg "# already exist:"ui_msg "#"ui_msg "#   sudo mkdir -p ${dbdir}"ui_msg "#   sudo chown ${osuser}:${osgroup} ${dbdir}"ui_msg "#"}
```

And there you have it. Not too bad really and although we didn't cover quite a number of the other features the port system provides, I think you will have seen enough to get you started and on your way to integrating your favourite software into the ports system.

It sure beats <strike>remembering</strike>forgetting a lot of manual steps each time you need to install/re-install software.
