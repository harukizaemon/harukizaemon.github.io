---
layout: post
title: "How To Write eql?() in Ruby"
alias: /2005/12/how-to-write-eql-in-ruby.html
categories:
---
So, as I've been delving into Ruby more and more of late and I needed to write an `eql?()` method for one of my classes. (`eql?()` is the Ruby equivalent of `equals()` in Java.)

Understanding how to write an `equals()` method in Java is one of the most fundamental yet poorly understood practices (if you're not worried about `equals()` or `hashCode()` then I suggest you never use a `HashMap`) and I wondered if the same was true of Ruby.

The most common mistake looks like this:

``` java
public boolean equals(Object object) {
  if (this == object) {
    return true;
  } else if (**!(object instanceof MyClass)**) {
    return false;
  }

  MyClass other = (MyClass) object;
  return this.x == other.x && this.y == other.y;
}
```

Spot the mistake? It's subtle yet very VERY wrong. Thankfully -- now that we've largely managed to get over our obsession with deep inheritence hierarchies -- it's effects aren't felt _that_ often. The problem is that `equals()` needs to be symmetric: `a.equals(b)` should return the same result as `b.equals(a)`.

Now, imagine a class `Person` with attributes of `name` and `address` and another class `Employee extends Person` with an extra attribute of `company`. Given the previous implementation of `equals`, what do you think the chances that the following two statements will return the same result?

``` java
person.equals(employee);
employee.equals(person);
```

So anyway, unlikely or not, it's still no excuse not to be precise so, in Java the canonical form of an `equals()` method should look roughly -- give or take some formatting and style -- like:

``` java
public boolean equals(Object object) {
  if (this == object) {
    return true;
  } else if (object == null || **getClass() != object.getClass()**) {
    return false;
  }

  MyClass other = (MyClass) object;
  return this.x == other.x && this.y == other.y;
}
```

Now we're comparing based on the _actual_ class and as such the implementation is symmetric; no more problems.

Assuming I still know next to nothing about Ruby, I decided I wouldn't let my Java habits get in the way of learning something new so I searched the 'net for some examples of how to implement `eql>()`.

After failing dismally to find anything non-trivial, I realised that Rails probably had something in `ActiveRecord::Base` and I was rewarded with:

``` ruby
def eql?(object)
  self == (object)
end
```

``` ruby
def ==(object)
  object.equal?(self) ||
  ( object.instance_of?(self.class) &&
    object.id == id &&
    !object.new_record? )
end
```

Nothing too outrageous: `eql?()` simply delegates to `==` (nice syntactic sugar); and `==` does pretty much the same thing as we would have done in Java only a little more terse. (`object.instance_of?(self.class)` is Ruby's way of saying `getClass() == object.getClass()`; the Ruby equivalent of Java's `instanceof` is `kind_of?()`.) We could easily re-write this -- although I wouldn't in practice -- as:

``` ruby
def ==(object)
  if object.equal?(self)
   return true
  elsif !object.instance_of?(self.class)
   return false
  end

  return object.id == id && !object.new_record?
end
```

So it seems -- I trust the Rails guys to get this stuff right-- that the same rules apply in Ruby as in Java. (I had no _real_ reason to suspect otherwise but hey, it s nice to have it confirmed.)

Now if only someone could explain to me why `Hash#reject()` and `Hash#select()` aren't symmetric: one returns a `Hash`; the other an `Array`.

**Update 2009-11-02:** Almost 4 years later I stumbled on this post and to my horror noticed that my naivety with Ruby at the time allowed me to believe the Rails code when really I shouldn't have. Here's the way the eql? method should have been written in the first place:

``` ruby
def eql?(object)
  if object.equal?(self)
   return true
  elsif !self.class.equal?(object.class)
   return false
  end

  return object.id == id && !object.new_record?
end
```

Or, if you don't care about the optimisation:

``` ruby
def eql?(object)
  self.class.equal?(object.class) &&
    object.id == id &&
    !object.new_record?
end
```
