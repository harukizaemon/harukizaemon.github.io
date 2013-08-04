---
layout: post
title: "Alias a Static Method in Ruby"
alias: /2006/05/alias-a-static-method-in-ruby.html
categories:
---
As much as I love Ruby on Rails, one of the things that dissapoints me is the rather large number of static methods. Maybe it's my Java background but class methods just irk me: They're difficult to override, mock, stub-out, and they're inherintly thread-unfriendly. But that aside, the fact remains that they exist and, now and again, I need to mess with them. Case in point: My [Foreign Key Migrations](https://github.com/harukizaemon/redhillonrails) plugin.

The plugin was working fantastically -- thanks to the efforts of Ted Davis for giving it a work out -- until it came time to running functional tests. Unfortunately, but unsurprisingly, the schema dumper used to copy the database structure from development to test creates tables in alphabetical order. Now I can't actually think of a much better alternative but the problem is that this means the foreign key clauses were being generated against tables that possibly didn't exist at the time of execution. (For example, Order sorts before Product however the orders table has a foreign key to the products table.)

So anyway, the longer-term solution is to batch up the foreign-key declarations until the end of the script and then execute them but in the meantime, I just wanted to disable their generation altogether. In either case, I needed to interfere with the execution of `ActiveRecord::Schema.define` a static (boo, hiss) method. Now, for the fun bit.

In Ruby, the way to _safely_ mix-in methods (rather than use subclassing) is to use some method chaining. For this we use `alias_method`. So, for example, if we wanted to override `to_s` to always place quotes around the value (nopt very useful but it will suffice for now) we could write a module like this:

```
module QuoteToSdef self.included(base)base.class_eval do**alias_method :to_s_without_quotes, :to_s** unless method_defined?(:to_s_without_quotes)**alias_method :to_s, :to_s_with_quotes**endenddef to_s_with_quotes"'#{to_s_without_quotes}'"endend
```

This essentially says that when the module is included (ie mixed-in) to a class then: add a method named `to_s_with_quotes`; create an alias of the existing `to_s` named `to_s_without_quotes`; make an alias of the new `to_s_with_quotes` named `to_s`; and finally, whenever `to_s`. is executed, call the old `to_s` method (now named `to_s_without_quotes`) and surround the results with, you guess it, quotes.

To use this you would either manually include the module in a class or, more along the lines of aspects, force the inclusion with some code like this:

```
MyClass.send(:include, QuoteToS)
```

(As a side note, the use of `unless method_defined?(:to_s_without_quotes)` is to work-around a bug in Ruby 1.8.4 that causes an infinite recursion when using `alias_method`. I never detected it under Mac OS X but apparently it affects windows machines with monotonous regularity. D'oh!)

So that's all very well and good but what happens when you need to do the same thing with static methods? The answer is, use `class &lt;&lt; self` and `extend`. In my case, overriding the behaviour of `ActiveRecord::Schema.define` looks something like this:

```
module ForeignKeyMigrations::Schemadef self.included(base)**base.extend(ClassMethods)**base.class_eval do**class &lt;&lt; self**alias_method :define_without_fk, :define unless method_defined?(:define_without_fk)alias_method :define, :define_with_fk**end**endend**module ClassMethods**def define_with_fk(info={}, &amp;block)...define_without_fk(info, &amp;block)endend**end**
```

Here, the use of `base.extend` causes all the methods defined within the module `ClassMethods` -- an aribitrary name used by convention in most if not all the rails code I've ever seen -- to be added as static methods on the class. Then, surrounding the `alias_method` calls within a `class &lt;&lt; self` causes them to be executed in a static context.

Again, to have this code mixed-in to the existing `ActiveRecord::Schema` class looks like this:

```
ActiveRecord::Schema.send(:include, ForeignKeyMigrations::Schema)
```

Phew!

P.S. all the plugins are now [available via SVN](http://rubyforge.org/scm/?group_id=1699).

**Update:** See the comments on how to simplify the code thanks to Ryan Tomayko.
