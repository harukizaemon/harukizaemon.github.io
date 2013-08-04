---
layout: post
title: "Finding the index of an item using a block"
alias: /2008/04/finding-index-of-item-using-block.html
categories:
---
**Update:** [Ruby 1.8.7](http://svn.ruby-lang.org/repos/ruby/tags/v1_8_7_preview1/NEWS) also has this.

Ruby 1.9 has it but if you're not that bleeding edge, you can have it now:

``` ruby
class Arraydef index_with_block(*args)
  return index_without_block(*args) unless block_given?
  each_with_index do |entry, index|
    return index if yield(entry)
  end
  nil
end
alias_method :index_without_block, :index
alias_method :index, :index_with_block

def rindex_with_block(*args)
  return rindex_without_block(*args) unless block_given?
  index = sizereverse_each do |entry|
    index -= 1
    return index if yield(entry)
  end
  nil
end
alias_method :rindex_without_block, :rindex
alias_method :rindex, :rindex_with_blockend
```

If you're using Rails you can substitute the two calls each to `alias_method` with a single call to `alias_method_chain`.
