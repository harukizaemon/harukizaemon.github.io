---
layout: post
title: "Abstract ActiveRecord Classes by Convention"
alias: /2006/10/abstract-activerecord-classes-by.html
categories:
---
Ruby on Rails provides a very simple mechanism for specifying that a model class is an _abstract base class_ and therefore has no corresponding database table:

```
class MyAbstractClass &lt ActiveRecord::Base**self.abstract_class = true**...end
```

Code can then interrogate a model class to see if it is _abstract_:

```
puts "it's abstract" if MyAbstractClass.abstract_class?
```

Not so hard, however I pretty much always prefix the name of my abstract classes with, you guessed it, `'Abstract'`. So, I added some code to the [RedHill on Rails Core Plugin](https://github.com/harukizaemon/redhillonrails) the other day to extend the definition of an abstract class to include the name:

```
def abstract_class?@@abstract_class || !(name =~ /^Abstract/).nil?end
```

With that simple change, I no longer need to explicitly set `self.abstract_class = true`; it just works by <strike>magic</strike>convention.

I suppose I could/should have created a plugin for it but I was feeling lazy :)
