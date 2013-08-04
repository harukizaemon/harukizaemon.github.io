---
layout: post
title: "Tests as documentation"
alias: /2010/02/tests-as-documentation.html
categories:
---
Whilst I've been playing around with [immutable collection classes in Ruby](http://github.com/harukizaemon/hamster), I've also been working on ways to document behaviour without writing loads of RDOC that goes stale really quickly.

Tests have always been touted as a form of documentation but I've rarely -- if ever come to think of it -- seen that work in practice. [Cucumber](http://cukes.info/) comes very close but I wanted something a little closer to the metal, something that allowed me to write unit tests with something like [RSpec](http://rspec.info/).

For sometime now, I'd been structuring my specs with this in mind and I thought I was doing a reasonably good approximation. Then today I finally had cause to test my work. As part of a feature I was implementing in another project, I wanted to use a list method I thought provided just what I required but I couldn't remember exactly how it behaved or what the interface was so, naturally, I consulted the documentation. D'oh! But wait, I thought smugly, I've been writing these specs and as we all know, specs are documentation. Moreover, I've been putting in quite a bit of effort to make them read as such so why not go read the specs?

Suffice to say, they didn't live up to my expectations. After running `spec -f nested spec/hamster/list/span_spec.rb` the result wasn't bad but wasn't great either:

``` text
Hamster::List
  #span
    is lazy
    on []
      with a block
        preserves the original
        returns a tuple with two items
        correctly identifies the prefix
        correctly identifies the remainder
      without a block
        returns a tuple with two items
        returns self as the prefix
        leaves the remainder empty
    on [1]
      with a block
        preserves the original
        returns a tuple with two items
        correctly identifies the prefix
        correctly identifies the remainder
      without a block
        returns a tuple with two items
        returns self as the prefix
        leaves the remainder empty
    on [1, 2, 3, 4]
      with a block
        preserves the original
        returns a tuple with two items
        correctly identifies the prefix
        correctly identifies the remainder
      without a block
        returns a tuple with two items
        returns self as the prefix
        leaves the remainder empty
```

For a start, there was no narrative, nothing telling me what the desired outcome was; why do I want to use this method? Secondly, whilst the individual assertions seemed to make sense when reading the spec code, once they were in this purely textual form they were somewhat useless in helping me understand what to expect. And lastly, a purely aesthetic complaint, I didn't really like the indentation so much. Right when all that hard work should have paid off, it failed me. But not completely. I was still convinced there was some merit in what I wanted and perhaps a little more tweaking could get me closer to my ideal.

After a few iterations of modifying the code, running the specs, and reading the output, I finally hit upon something I think is pretty close to what I've been after:

``` text
Hamster.list#span
  is lazy
  given a predicate (in the form of a block), splits the list into two lists
  (returned as a tuple) such that elements in the first list (the prefix) are
  taken from the head of the list while the predicate is satisfied, and elements
  in the second list (the remainder) are the remaining elements from the list
  once the predicate is not satisfied. For example:
    given the list []
      and a predicate that returns true for values <= 2
        preserves the original
        returns the prefix as []
        returns the remainder as []
      without a predicate
        returns a tuple
        returns self as the prefix
        returns an empty list as the remainder
    given the list [1]
      and a predicate that returns true for values <= 2
        preserves the original
        returns the prefix as [1]
        returns the remainder as []
      without a predicate
        returns a tuple
        returns self as the prefix
        returns an empty list as the remainder
    given the list [1, 2, 3, 4]
      and a predicate that returns true for values <= 2
        preserves the original
        returns the prefix as [1, 2]
        returns the remainder as [3, 4]
      without a predicate
        returns a tuple
        returns self as the prefix
        returns an empty list as the remainder
```

This time there's a narrative describing what the method does, followed by a series of examples not only describing the behaviour but also providing concrete values. Now the output reads more like documentation only rather than duplicated as RDOC that rapidly becomes disconnected from reality, it's generated from the tests and automatically stays up-to-date.

The underlying spec is not perfect by any stretch -- there is certainly a modicum of duplication between the test code and the descriptive text -- but I think it strikes a reasonable balance between tests that are readable as code as well as plain text documentation. I'd certainly love to know what, if anything, others have done.

``` ruby
require File.expand_path('../../../spec_helper', __FILE__)

require 'hamster/list'

describe "Hamster.list#span" do

  it "is lazy" do
    lambda { Hamster.stream { |item| fail }.span { true } }.should_not raise_error
  end

  describe <<-DESC do
given a predicate (in the form of a block), splits the list into two lists
  (returned as a tuple) such that elements in the first list (the prefix) are
  taken from the head of the list while the predicate is satisfied, and elements
  in the second list (the remainder) are the remaining elements from the list
  once the predicate is not satisfied. For example:
DESC

    [
      [[], [], []],
      [[1], [1], []],
      [[1, 2], [1, 2], []],
      [[1, 2, 3], [1, 2], [3]],
      [[1, 2, 3, 4], [1, 2], [3, 4]],
      [[2, 3, 4], [2], [3, 4]],
      [[3, 4], [], [3, 4]],
      [[4], [], [4]],
    ].each do |values, expected_prefix, expected_remainder|

      describe "given the list #{values.inspect}" do

        before do
          @original = Hamster.list(*values)
        end

        describe "and a predicate that returns true for values <= 2" do

          before do
            @result = @original.span { |item| item <= 2 }
            @prefix = @result.first
            @remainder = @result.last
          end

          it "preserves the original" do
            @original.should == Hamster.list(*values)
          end

          it "returns the prefix as #{expected_prefix.inspect}" do
            @prefix.should == Hamster.list(*expected_prefix)
          end

          it "returns the remainder as #{expected_remainder.inspect}" do
            @remainder.should == Hamster.list(*expected_remainder)
          end

        end

        describe "without a predicate" do

          before do
            @result = @original.span
            @prefix = @result.first
            @remainder = @result.last
          end

          it "returns a tuple" do
            @result.is_a?(Hamster::Tuple).should == true
          end

          it "returns self as the prefix" do
            @prefix.should equal(@original)
          end

          it "returns an empty list as the remainder" do
            @remainder.should be_empty
          end

        end

      end

    end

  end

end
```
