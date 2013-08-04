---
layout: post
title: "A City Full of Code Thieves"
alias: /2006/10/a-city-full-of-code-thieves.html
categories:
---
After nearly falling over at the ease with which I could use TeamCity to [crush helpless machines](/blog/2006/10/20/a-city-of-trojan-ants), it suddenly occurred to me that I may have found yet another security hole.

As you probably already know, the TeamCity server doesn't do the builds itself; rather, it farms the work off to build-agents. You can connect as many build-agents as you like to a server: simply download (or otherwise obtain) a copy of the build-agent code; configure it to point at the server; and start it up. No server-side credentials are necessary.

When a build is required, the server checks out the source code and sends a delta to the agent. This has a number of benefits, one being that you can securely configure the source-code-repository credentials in one place -- the server -- and the agents will be sent the source code as needed. This also poses a potential security risk.

Let's imagine that disgruntled developer X from our [previous exploit](/blog/2006/10/20/a-city-of-trojan-ants) wants to obtain a copy of source code from a repository for which he has no access but for which he knows there is a TeamCity build. He simply configures his build-agent to connect to the server and waits. When the server decides it's his machine's turn to do a build, the server dutifully sends him a copy of the source-code...!
