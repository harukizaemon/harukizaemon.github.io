---
layout: post
title: "Mac OS X House Keeping"
alias: /2004/11/mac-os-x-house-keeping.html
categories:
---
Having been a linux weenie for a few years now I had become accustomed to running various house keeping jobs on a regular basis and I wanted to do the same thing on my new PowerBook.

In particular, I use `locate` for quickly finding files, which to be of any use, requires the the indexer (`updatedb`) to be run periodically. A quick `grep` through the `man` pages and I discovered the OS X version was `/usr/libexec/locate.updatedb` so the next step was to get it to run as a batch job.

Whilst searching for the appropriate place to put my daily system cron jobs (`/etc/daily.local`), I ran across this little gem in `/etc/daily`: `# Clean up NFS turds.  May be useful on NFS servers.`
