---
layout: post
title: "The Days I Turned Pluralisations: Off"
alias: /2006/04/days-i-turned-pluralisations-off.html
categories:
---
Oh what a day it was.

You see, the first real Rails project we did, I thought, "well, I'll give it a try. This guy seems to know what he's talkign about and even though pluralisation goes against everything I've ever learned or studied with repsect to database design, why not?"

One of the major factors in making rails so productive (for me at least) is that it reduces the cognitive dissonance -- between my ideas (creativity) and the implementation (ruby/rails). I find when I'm coding in rails that it's enabling me to quite literally code what I'm thinking as I'm creating, in way that Java never has. (That's not necessarily the fault of Java per-se, rather my feeble brain.) The pluralisation stuff seems to fly directly in the face of this.

Well suffice to say that on my second real project, I decided to turn it off, and my brain thanked me for it. I simply added the following to `config/environment.rb`:

``` ruby
Rails::Initializer.run do |config|
  ...
  config.active_record.pluralize_table_names = false
end
```

No longer did I need to keep remembering that even though the class is called `Entity` the table is called `Entities`. No more pissing around with pluralisation rules because, what-do-you-know, the default ones can't handle the fact that the plural of `UnitOfMeasure` is `unit**S**_of_measure`. No more switching contexts everytime I go from SQL to Ruby code. (Yes, I manually dig around in the database using SQL to make sure I haven't screwed something up. And yes, I also use the rails console.)

And you know what? In neither project has the customer once asked me to show them the database schema -- one of the arguments supposedly for using plural table names. No, in fact if I ever wanted to show the customer anything (other than the application itself), I usually scribbled labelled boxes on a whiteboard and even then, I've not once had anyone give me a puzzled look or ask "why is that singular?" Strange perhaps but true!

So whilst I admire the audacity and coding athleticisim of DHH for creating the pluralisation code, I'm far more impressed with the work he and the team have done on Rails 1.1. Everything seems so much "better": ActiveRecord for one; and especially RJS templates as another example. At least they had the foresight (even pre 1.0) to allow pluralisation to be turned off!

Halleloujah!
