---
layout: post
title: "HELP: Developing with WebSphere"
alias: /2008/01/help-developing-with-websphere.html
categories:
---
I have to know. Is it just me or is the compile, deploy, debug cycle with WAS just so ridiculously long and slow that it's practically impossible to do anything productive with it? Does anyone ACTUALLY use WAS for development? If so, how do you manage? If not, what do you use instead and how do you then make sure that it still runs in WAS when it can take 10 minutes just to re-deploy an application? Is there some secret to determining why WAS silently rolls back transactions, for no apparent reason (WLTC0033W and WLTC0032W), before control has even returned to the container and with no exceptions; doesn't seem to be able set transaction isolation levels on SQL Server but instead creates extra connections with the correct isolation level (READ_COMMITTED) leaving the original one handed back from the DataSource as the default (REPEATABLE_READ); and mysteriously times-out waiting for Oracle connections from a pool of 10 even it only ever seems to need at most 3 when the maximum pool size is set to 100!
