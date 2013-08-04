---
layout: post
title: "Detecting unspecified method arguments in Ruby"
alias: /2011/04/detecting-unspecified-method-arguments-in-ruby.html
categories:
---
Unlike some languages, Ruby has no real way (that I know of) to define overloaded methods. Instead, you can specify that certain arguments are optional. For example, if we were to re-implement the `Enumerable#reduce` method, we might do so like this:

``` ruby
def reduce(memo = nil)
  return slice(1..-1).reduce(first) if memo.nil?
  each { |element| memo = yield(memo, element) }
  memo
end
```

That is, if no memo was specified, use the first element as the memo with a call to `#reduce` on the remaining elements of the array.

<!--more-->
This approach generally works out fine unless either `nil` is actually a valid value, or interestingly, when it's not. In the first case, I don't want `nil` to trigger the special behaviour, I want it passed to my block. In the latter case, I've encountered times when a programming bug has meant that I've explicitly passed nil to a method in error which has triggered the special behaviour.

At first I attempted to solve this problem using a splat:

``` ruby
def reduce(*args)
  return slice(1..-1).reduce(first) if args.length
  raise ArgumentError, "wrong number of arguments(#{args.length} for 0..1)" if args.length > 1
  each { |element| memo = yield(memo, element) }
  memo
end
```

Which on the face of it isn't too bad I suppose but get's awfully complex when you have methods that accept multiple arguments.

Another option might be to use named arguments and a hash. For example:

``` ruby
def reduce(args = {})
  return slice(1..-1).reduce(first) unless args.has_key?(:memo)
  raise ArgumentError, "wrong number of arguments(#{args.length} for 0..1)" if args.length > 1
  each { |element| memo = yield(memo, element) }
  memo
end

some_array.reduce(memo: "some memo")
```

But from experience, this makes the calling code unnecessarily complicated and is no better with multiple arguments.

The solution I settled on, and believe me I'm open to suggestions, is to use a default value that I know won't be used by calling code:

``` ruby
UNSPECIFIED = Object.new

def reduce(memo = UNSPECIFIED)
  return slice(1..-1).reduce(first) if memo.equal?(UNSPECIFIED)
  each { |element| memo = yield(memo, element) }
  memo
end
```

This way the calling code remains the same, the method definition remains largely the same, and to my mind explicitly calls out the case where the arguments aren't specified by the caller -- much the same way you can call `#block_given?` to determine if a block has been passed to a method.
