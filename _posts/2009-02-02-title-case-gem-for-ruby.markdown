---
layout: post
title: "A Title Case Gem for Ruby"
alias: /2009/02/title-case-gem-for-ruby.html
categories:
---
A project I'm working on called for some "smart" capitalisation of page titles. Essentially I wanted to take a URL [slug](http://en.wikipedia.org/wiki/Slug_(production)) and generate a page title.

Rails comes with a built-in `String#titleize` method that capitalises every word but that looked a little odd when the title was something like: _"My Hovercraft Is Full Of Eels"_. So I went on a hunt for something "smarter".

After a little search I stumbled upon Marshall Elfstrand's [JavaScript, Ruby, and Objective-C  ports](http://vengefulcow.com/titlecase/) of [John Gruber's](http://daringfireball.net/) "Title Case" algorithm and decided to turn it into a [Gem](http://github.com/harukizaemon/titleizer) that adds `String#titleize` and `String#titleize!` (aliased as `#titlecase`, and `#titlecase!` respectively). When used in a Rails environment, this effectively replaces the Rails versions.

Now my page titles look a little more human-like: "My Hovercraft is Full of Eels".
