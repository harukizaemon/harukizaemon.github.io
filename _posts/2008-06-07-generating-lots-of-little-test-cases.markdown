---
layout: post
title: "Generating lots of little test cases"
alias: /2008/06/generating-lots-of-little-test-cases.html
categories:
---
When writing code that is largely algorithmic, I find I end up writing specs that sit in a loop repeating the same operations over a set of data.

This works well enough but it has the downside that the tests abort as soon as a single failing case is detected which can lead to vicious cycles of fixing one case only to find you've broken another.

The solution is of course to break out each expectation -- inputs and expected outputs -- so they are all run and reported individually. Doing so by hand however, is tedious to say the least so why not generate them on the fly instead:

{% highlight ruby %}
describe Fibonacci do
  [[0, 0], [1, 1], [2, 1], [3, 2], [4, 3], [5, 5], [6, 8]].each do |input, output|
    it "should generate #{output} for #{input}" do
      Fibonacci.calculate(input).should == output
    end
  end
end
{% endhighlight %}

This is such a elegant solution I'm not sure why it only just occurred to me.
