---
layout: post
title: "Plugging a Team City Security Hole with a Little Obfuscation"
alias: /2006/10/plugging-team-city-security-hole-with.html
categories:
---
If you're not sure what I'm talking about, have a quick read of [my earlier post]({% post_url 2006-10-26-a-city-full-of-code-thieves %}).

The _trick_ -- as far as I can tell -- to plugging the hole is to: disable guest logins to the server; and ensure each build configuration requires each agent to have a secret environment variable set to a, secret, value -- something like a [generated WEP key](http://www.andrewscompanies.com/tools/wep.asp) for example.

This seems to be a reasonable solution until JetBrains figures out a better mechanism. That said, in many respects, it's not that different to the way WEP authentication works anyway.

Oh, an why the need to disable guest login? TeamCity shows all logged in users -- yes, even guests -- which agents aren't compatible and, in particular, why.

Of course there may well be a way to interrogate programatically what the requirements are in which case, you're hosed anyway :(
