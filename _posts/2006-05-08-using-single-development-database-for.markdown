---
layout: post
title: "Using a Single Development Database For all Rails Applications"
alias: /2006/05/using-single-development-database-for.html
categories:
---
I used to have a separate database and/or user for each application I was developing. This worked for a while but once I had three or so projects underway, I grew tired of remembering which database and which login to use when running psql, etc. In addition, if anyone else wanted to do development, they too needed to either configure another rails environment (eg. james_development) or they had to create another database/user just as I had.

Then it occurred to me that psql (at least under Mac OSX) defaults to connecting to a database named for the currently logged in user. So, I reasoned, if all the developers had a _default_ database, then if I could configure rails to connect to that I might be rid of my myriad databases. The problem was: how?

At first I tried not specifying a database in `config/database.yml` at all. D'oh! No luck. Then I wondered if I could add an [ERB](http://www.ruby-doc.org/stdlib/libdoc/erb/rdoc/)-ish macro for the current user (I may even have read that it was possible but I can't for the life of me recall where):

```
development:adapter:  postgresqldatabase: **&lt;%= ENV["USER"] %&gt;**username: **&lt;%= ENV["USER"] %&gt;**
```

I'm not sure at what level this is being handled (rails, yaml, ruby, somewhere in between) but it worked! Now when the application loads in development, the name of the currently logged in user is substituted in for the name of the database.

The only other thing is to always remember to specify a table-name prefix in `config/environments/development.rb` and all the applications will happily co-exist in the same, default, database:

```
  config.active_record.table_name_prefix = "cjp_"
```

I've been playing with having all my applications in the one development database for a while now and it seems to be working out ok. On the down-side, it does mean I need to remember the table-name prefix when using psql, in some ways moving the problem rather than removing it, however it doesn't seem to get in the way especially because psql has table completion for table-names, column-names, indexes, you name it. (That's right: `select * from cjp_[tab] where [tab] = '...';`). The other downside of course is if I ever want to blow away the entire database I can't but then again, that's what `rake migrate VERSION=0` is for.

I'm not sure if I'll continue in the long-term with this but it sure makes adding a new developer to the team dead-simple and makes my local machine configuration a lot simpler in the meantime. Besides, I needed an excuse to try using table-name prefixes so I could test my migration extensions. Guess what? They didn't handle it! They do now :)
