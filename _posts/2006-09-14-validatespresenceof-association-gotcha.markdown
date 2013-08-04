---
layout: post
title: "validates_presence_of association Gotcha"
alias: /2006/09/validatespresenceof-association-gotcha.html
categories:
---
The more I use Rails (and the more plugins I create) the more quirks I find.

Imagine I have a one:many relationship between `Country` and `State`:

``` ruby
State.belongs_to :country
Country.has_many :states
```

We then issue the following sequence of statements (I've interleaved the output of tailing the development log):

``` ruby
c = Country.find_by_name('Australia')
**  Country Load (0.006506)   SELECT * FROM countries WHERE (countries."name" = 'Australia' ) LIMIT 1**
s = c.states.build(:name =&gt; 'Victoria', :abbreviation =&gt; 'VIC')s.country
**  Country Load (0.009738)   SELECT * FROM countries WHERE (countries.id = 1)**
```

Notice the `SELECT` to find the country? Now why would that be necessary? I just used `.states.build` on the country. I would have thought that would set the association but that doesn't appear to be the case.

Looking at the code, my suspicions were confirmed: only the parent's `id` is set. That seems decidedly odd given that we know for a fact the parent exists -- we just used it to create the child.

So anyway, I'm pretty sure this is considered a "feature" but to be honest, I can't see why it is desired behaviour over and above the fact that doing otherwise would be more work and why would you need this if you already have the parent yada, yada, yada.

Well, for a start, I'd like this behaviour because I'd like to use `validates_presence_of` on foreign-keys and have it work for newly constructed graphs. Usually this barfs no matter what but I concocted a work-around last night and committed it to my [Foreign Key Associations](https://github.com/harukizaemon/redhillonrails/tree/master/foreign_key_associations) plugin which, if done manually, would look something like this:

``` ruby
class State &lt; ActiveRecord::Base
  validates_presence_of :country_id, :if =&gt; lambda { |record| record.country.nil? }
  ...
end
```

Essentially this says to validate the presence of `country_id` but only if there isn't an associated country. This means that for cases where the parent record is also new, the validation checks for the presence of the associated object rather than the foreign-key column. If you had simply used `validates_presence_of :country_id` then `save` would fail because `country_id` was still `nil`.

OK that's all very well and good but it still doesn't help because, as shown above, the association isn't set anyway. So, I'm now back to manually setting the association; at least the validation works hehe

I'm sure someone far smarter than I will point out why the behaviour as it stands is obviously the most appropriate and that no one in their right mind would want to do anything else, of course ;-)
