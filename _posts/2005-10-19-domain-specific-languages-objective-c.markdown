---
layout: post
title: "Domain Specific Languages: Objective-C, Ruby and Java (and Groovy)"
alias: /2005/10/domain-specific-languages-objective-c.html
categories:
---
I'm forever trying to "improve" my coding and design skills; I say "improve" because there is always the risk that I'm making changes for change sake. One thing I really try hard to do is remove any notion of getting/setting properties of objects and instead focusing on behaviour.

Now I've always tried hard to do this but it's a skill that definitely takes some time and constant practice. TDD certainly has made my life easier by giving me some tools for this but more recently I've really been making an effort to try and "design" my classes in a way that creates [Domain Specific Languages](http://www.martinfowler.com/bliki/DomainSpecificLanguage.html) (DSL). DSLs enable you to write code that is hopefully [more readable](/blog/2005/03/24/writing-readable-code) and understandable and therefore easier to write, debug and maintain. (At least that's the ide in principle anyway.)

I use a variety of languages in my day-to-day work and I've found that the effort required in creating a DSL to be vastly different depending on the language. Of the few languages I've actually used in anger ([Smalltalk](http://en.wikipedia.org/wiki/Smalltalk) unfortunately **not** being one of them) [Objective-C](http://en.wikipedia.org/wiki/Objective-C) really does provide a nice syntax for creating DSLs (once you get your head around the square brackets).

For example, take the age-old problem of transferring money from one account to another. In Objective-C, assuming we have an `Account` class with a `transfer()` method, we can write something like this:

```
id source = ...;id destination = ...;[source transfer:20.00 to:destination]
```

> **_* Using floats for currency is generally a bad idea but it'll do for illustrative purposes here :-)_**

Notice the use of named parameters to really help convey the intent. This is actually an optional feature -- you can still use positional arguments if you like -- but one I use exclusively.

The `transfer()` method might look like this:

```
-(void)transfer:(float)amount to:(id)destination {[self debit:amount];[destination credit:amount];}
```

Again, ignoring the unfamiliar method declaration (you'll just have to trust me when I tell you that you do get used to the language and even love it), it's all pretty nice and readable.

Essentially, when calling the method, the identifier that _preceeds_ a colon serves as the name of each argument -- ie as used by the caller-- with the method name itself serving as the name of the first argument and `to` for the second.

Once inside the method, the identifier _after_ the colon serves as the name of the argument to be used: `amount` and `destination`.

All up, Objective-C is very concise and allows for the creation of fairly nice DSLs without much effort at all. In fact from what I can tell, most Objective-C code turns out to be more-or-less DSL-like; it's typical to see methods calls that look like:

```
[report printTo:printer withPagination:YES]
```

The next example I whipped up uses [Ruby](http://www.ruby-lang.org/). Now, I have to admit that I have all of about 3 days of Ruby experience so if you can come up with a better way please, please, please let me know. So, caveats aside, here is the same example in my bestest Ruby, again starting with the usage:

```
source = ...destination = ...source.transfer :amount=>20.00, :to=>destination
```

This uses a ruby hash -- an associative array like a `HashMap` or `Hashtable` in Java. You usually need to wrap the hash definition in curly-braces but this can be omitted when the hash is used as the last -- or only -- parameter.

Now for the method itself:

```
def transfer paramsamount = params[:amount]self.debit amountparams[:to].credit amountend
```

Pretty good really but I still have a preference for the Objective-C use of named parameters. I did read in [Programming in Ruby](http://www.rubycentral.com/book/) that named parameters was to be a feature of Ruby at some point but hasn't yet made it. I also read somewhere recently that Ruby 1.9 will have a slightly simpler calling syntax so the invocation will look like this:

```
source.transfer amount:20.00, to:destination
```

The only Ruby-based DSL I know of is [Rake](http://rake.rubyforge.org/) but again my experience with Ruby is somewhat limited.

And finally Java, IMHO the ugliest of the bunch. To achieve something similar in Java seems to me at least (and again, somebody prove me wrong puhlease!) that named parameters with an even remotely useful syntax requires a combination of inner-classes and method chaining. So, to achieve a calling syntax like this:

```
Account source = ...;Account destination = ...;source.transfer(20.00).to(destination);
```

Requires something _at least_ as complicated as this:

```
public class Account {...public Transfer transfer(float amount) {return new Transfer(amount);}public class Transfer {private final float amount;Transfer(float amount) {this.amount = amount;}public void to(Account destination) {Account.this.debit(amount);destination.credit(amount);}}}
```

The best example of a Java-based DSL that I can think of would probably be [JMock](http://www.jmock.org/).

And finally, by popular demand, a [Groovy](http://groovy.`haus.org/) example. Groovy currently supports named parameters for calling methods that accept a `Map` so the calling syntax is a lot like Objective-C:

```
def source = ...def destination = ...source.transfer(amount:20.00, to:destination)```
```

The `transfer()` method itself then becomes something like:

```
void transfer(params) {def amount = params.amountthis.debit(amount)params.to.credit(amount)}
```

Which looks unsurprsingly like Ruby. Alas, I have no real-world example of a Groovy-based DSL to show you.
