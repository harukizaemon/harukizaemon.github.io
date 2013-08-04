---
layout: post
title: "Perforce Client Setup"
alias: /2006/10/perforce-client-setup.html
categories:
---
For anyone who is is unfortunate enough to work with Perforce -- and so I don't have to remember -- here's a quick-and-dirty kick-start guide for setting up a client workstation. (Note I'm on a Mac so your mileage may vary.)

First things first, install the perforce software available from [http://www.perforce.com/perforce/downloads](http://www.perforce.com/perforce/downloads).

Next, in order to avoid various command-line arguments, I have the following environment variables set:

```
export P4EDITOR="$EDITOR"export P4USER="username"export P4PORT="1666"export P4HOST="clientname.local"export P4CLIENT="clientname"
```

If you're using SSH, you'll need to create a tunnel to the server. Something like this should work:

```
ssh -L1666:server:1666 -p 22 -N -t -x username@server
```

This sets things up so all requests to `localhost:1666` are routed over ssh to the remote server. You can then setup the client:

```
p4 client
```

This will launch your default editor -- in my case that's TextMate but nano/pico/emacs/vi/etc will do -- and allow you to modify the following fields:

```
Client:	clientnameOwner:	usernameHost:	clientname.localRoot:	/path/to/projects/View:
```

See the documentation for an explanation on how to set-up the `View`. In my case, I'm running a rails application, so I have some rules to exclude various generated and client specific directories:

```
-//depot/projectname/config/database.yml //clientname/projectname/config/database.yml-//depot/projectname/db/schema.rb //clientname/projectname/log/schema.rb-//depot/projectname/log/... //clientname/projectname/log/...-//depot/projectname/tmp/... //clientname/projectname/tmp/...
```

Finally, to get a copy of the latest source code in `/path/to/projects/projectname`, run:

```
p4 sync
```

And because I just can't help myself, by way of comparison, here's the equivalent instructions for subversion:

```
svn co svn+ssh://username@repositoryurl/trunk/projectname
```

Gosh, wasn't that difficult.
