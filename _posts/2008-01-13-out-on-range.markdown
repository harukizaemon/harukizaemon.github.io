---
layout: post
title: "Out on the Range"
alias: /2008/01/out-on-range.html
categories:
---
I'm pretty sure I've blogged before about my dislike of prefix/suffix pairs such as `from`/`to` and `start`/`end`. They smell of a missing abstraction. There are however languages that provide a nice solution.

We have an application that stores a date against a record in the database. At various times, we want to see if that date falls between a specific date range. The initial implementation of the code looked something like this:

```
def within?(from_date, to_date)@date >= from && @date <= toend
```

Which you would then be used along the lines of:

```
puts "Not available" if record.within?(start_date, end_date)
```

This immediately activated my olfactory senses: Not only do we have some common suffixes, we also have comparison code that looks like pretty much like every other kind of range comparison code you're ever likely to write.

As it happens, Ruby has a [Range](http://ruby-doc.org/core/classes/Range.html) class built-in that I don't see being used nearly as much as I think it deserves. Using a range we could re-write the example to look something like:

```
def within?(date_range)date_range.includes?(@date)end
```

And then in the worst case, call it like this:

```
puts "Not available" if record.within?(start_date..end_date)
```

I say worst case because here the client code still uses two separate dates which are then converted to a Range for the purposes of the call. (Interestingly the use of use of `..` to construct a Range out of two individual values uses the same number of keystrokes as passing the parameters individually!) However, in the best case, we'd have been using a Range to store the dates in the first place.
