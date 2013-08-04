---
layout: post
title: "It's OK for GET Requests to Update the Database"
alias: /2008/04/it-ok-for-get-requests-to-update.html
categories:
---
We've all been indoctrinated into associating the HTTP request methods POST, GET, PUT and DELETE with the standard database (aka CRUD) operations Create (INSERT), Read (SELECT), Update and Delete respectively. For the most part the analogue holds. When we make a GET request, our _intention_ is to read whatever the server hands back. When we POST some data, our _intention_ is to update something.

The general view however, seems to be that the HTTP methods relate _directly_ to database operations. In fact many developers seem to think that they are in fact one in the same thing: POST is for INSERTing data, GET for SELECTing, etc. The popularity of which seems to have strengthened with the growing interest in REST and the wide-spread adoption of Rails 2.[^1] In fact read of the HTTP/1.1 Method Definitions [specification](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) explicitly states that so-called "Safe Methods" such as GET:

> ... SHOULD NOT have the significance of taking an action other than retrieval.

But what happens when we have a site that tracks where you have visited, updates the "last read date" on each record retrieved, or remembers the last search criteria you used? Each of these features requires recording information into some kind of database -- be it relational or simply a log file.

What people seem to overlook is the paragraph that follows in the same document:

> Naturally, it is not possible to ensure that the server does not generate side-effects ... The important distinction here is that the user did not request the side-effects, so therefore cannot be held accountable for them.

The HTTP methods should be used to indicate the _user's_ intention without regard to the underlying implementation. The web application is an abstraction so we need to model the interaction on that abstraction. If the user's intention is to make a change to something then go ahead and use a PUT but if they're only reading some data use a GET even if you know it involves some database writes.

It may seem somewhat esoteric but spending a bit of time thinking about what the user's intention is exactly has helped me better flesh out an application's API.

[^1]: I don't mean to imply that Rails is the culprit here. Nothing in Rails explicitly makes these assertions. However the fact that the idiom is explicitly referred to as CRUD resources certainly doesn't help.
