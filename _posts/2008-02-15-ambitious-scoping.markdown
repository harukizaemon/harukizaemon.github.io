---
layout: post
title: "Ambitious Scoping"
alias: /2008/02/ambitious-scoping.html
categories:
---
If you haven't checked out [Ambition](http://ambition.rubyforge.org/) for generating ActiveRecord queries I highly recommend you do so. In a nutshell, it allows you to generate queries using the standard Ruby Enumeration idioms. Take for example the following snippet:

```
class Message < ActiveRecord::Basedef self.unreadselect { |m| m.read_at == nil }.entriesendendMessage.unread=> SELECT * FROM messages WHERE messages.read_at IS NULL
```

Notice anything missing? SQL perhaps?

Now clearly there's a whole lotta magic going on here. That said, ambition does use the standard ActiveRecord finders to query the database. One of the side-effects of this is that when navigating associations, you automatically get all the appropriate scoping.

So, for example, assuming a `has_many` relationship between `User` and `Message` we can do something like this:

```
user = User.detect { |c| c.name == 'Simon Harris' }=> "SELECT * FROM users WHERE (users.name = 'Simon Harris') LIMIT 1"user.messages.unread=> "SELECT * FROM messages WHERE messages.read_at IS NULL AND messages.user_id = 3"
```

Here we navigated from `user` to `messages` and selected only those that have no `read_at` date.

As you can see, the restriction by `user_id` was automatically inserted for us by ActiveRecord as we navigated the association. We didn't have to do a thing. It just worked the way you would expect it to. Very cool!

However (there's always a but) this only worked because we included an execution trigger in `Messages.unread`. These "kickers" as they're known, include the standard `Enueration` methods `first`, `entries`, `detect` and `size`.

Without a kicker, the result of `select` is actually a query object that you can assign to a variable, call more selects on, or even store for later use! This last feature is kinda neat and leads to some nice stuff with partial caching but it also leads to some hidden complications.

If we remove the kicker from `Message.unread` so that it instead returns a query and call the kicker explicitly, this happens:

```
class Message < ActiveRecord::Basedef self.unreadselect { |m| m.read_at == nil }endenduser = User.detect { |c| c.name == 'Simon Harris' }=> "SELECT * FROM users WHERE (users.name = 'Simon Harris') LIMIT 1"user.messages.unread.entries=> "SELECT * FROM messages WHERE messages.read_at IS NULL"
```

Where did all the scoping go?!

In the earlier example, the query was executed inside a method on an model class which, because it was called whilst navigating an association, means it was also executed using an appropriate [`with_scope`](http://api.rubyonrails.com/classes/ActiveRecord/Base.html#M001416).

In the second example however, because the query wasn't actually executed until sometime later -- ie outside any model classes and associations -- all the scoping information has been forgotten, as if it were never there in the first place. Bbbbbut, you want to have all that Ambition goodness and you'd like your scoping as well right? So what's a poor boy (or girl) to do?

I'm glad you asked. Here's a [patch](http://err.lighthouseapp.com/projects/466/tickets/188-remember-association-scoping)[^1] to ambitious-activerecord that remembers the scoping that was in play at the time the query was constructed. It seems to work for all my current uses.

[^1]: DISCLAIMER: very quick-and-dirty and thoroughly untested
