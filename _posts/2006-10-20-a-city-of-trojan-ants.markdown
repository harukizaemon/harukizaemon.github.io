---
layout: post
title: "A City of Trojan Ants"
alias: ["/2006/10/a-city-of-trojan-ants.html", "/blog/2006/10/20/ca-ity-of-trojan-ants"]
categories:
---
As much as I've bitched about IntelliJ performance, and as much as I wish I didn't have to do Java development on a regular basis, the fact of the matter is I do and IntelliJ just rocks my world in terms of features -- I guess I'll just have to wait for the new Core 2 Duo MacBook Pro to address the performance issue-- so today I purchased a copy of the latest version.

IntelliJ 6.0 comes with TeamCity, a continuous-integration/build-server tool. It would appear that TeamCity isn't just restricted to building applications, but could really do anything assuming you do in  ant script.

TeamCity has the concept of build agents so all the real work is farmed off to other machines, so I setup my machine as a build-agent which required opening up a port -- 9090 is the default -- through the firewall on my machine. This immediately rang warning bells in my head: I had just run the agent using `sudo` and any application run using `sudo` that needs ports opened up scares me. In the end I realised I could easily run the agent as a non root user and all was happy. Excellent!

That did get me thinking however, as to what would happen on say, M$ Windoze machines where developers typically run with administrator priveleges or even poor saps like me who had stupidly run using `sudo` or even just as my own login.

Imagine a team of 20 developers, all of whom have obligingly run a build-agent so that their spare CPU cycles don't go to waste. One day developer X becomes disgruntled with his employer and decides to run wild. Before he goes home at night, he checks in a change to `build.xml`. Not a large change, nothing special, just a one line change that recursively deletes everything starting at the root directory or even just the user's home directory.

This change quickly makes it to the TeamCity server and then out to a build machine which dutifully executes the script, destroying everything taking down the machine with it!

Thankfully, the TeamCity server detects that the agent has gone down and, doing it's best to utilise the resources at its disposal, re-schedules the build on the next available agent...

To be fair, TeamCity isn't really to blame, if a developer runs `build.xml` without first ensuring that it does nothing macilicious, it's really no different to running an un-verified shell script. TeamCity just makes it really easy to setup agents and ensure that the script is executed.

Me thinks it's time make a seperate, locked-down account for running the build-agent!
