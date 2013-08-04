---
layout: post
title: "Conventional Validations"
alias: /2007/10/conventional-validations.html
categories:
---
As part of some other work I'm currently undertaking, I've just extracted a VERY new Rails plugin called, you guessed it, [Conventional Validations](https://github.com/harukizaemon/redhillonrails/tree/master/conventional_validations).

As the README says, Conventional Validations is a plugin that attempts to apply validation based on the naming of column. Specifically, the plugin searches for validation methods such that a columnnamed `foo` or ending in `_foo` would be validated using a method named `validates_foo`.

For example, by defining a class method as:

``` ruby
def self.validates_phone(*attr_names)
  validates_format_of attr_names, :with => /[0-9]+(\.[0-9]+)*/, :allow_nil => true
end
```

Any columns named `phone` or ending in `_phone` would be automatically validated.

I've only just added it (it's only available in trunk) and the matching rules are VERY simplistic but if you have company-wide or even project-wide validation and you're strict about your naming conventions (as I am), consider extracting them into a module and mixing them into `ActiveRecord::Base` and letting the plugin apply them for you.

There are already some existing plugins such as (off the top of my head) "validates_email" that would most likely just work out of the box.

Enjoy.
