---
layout: post
title: "Rails, PostgreSQL and Case-Sensitivity"
alias: /2007/02/rails-postgresql-and-case-sensitivity.html
categories:
---
Possibly the only thing I like about MySQL is when performing a search, the values `'SIMON'` and `'sImOn'` are considered equal -- case-_insensitive_ searching. PostgreSQL on the other hand considers them to be different -- case-_sensitive_ searching. Now I don't know about you but for %99.999~ of the applications I've ever written, I'd rather `'Australia'` and `'AuStRaLiA'` weren't considered different countries.

The "standard" approach to solving this problem is to change a query from this:

```
SELECT * FROM countries WHERE name = ?;
```

To something like this:

```
SELECT * FROM countries WHERE **LOWER(**name**)** = **LOWER(**?**)**;
```

Thereby forcing the database to perform a pseudo case-insensitive search. The only problem is that all those nice indexes you've created to ensure fast, efficient searching are totally ignored. (Who can spell full-table scan?) Performance issues aside (I mean after all we know that premature optimisation is the root of all evil right?) what's just as annoying is that I can't actually guarantee uniqueness, which is pretty much the whole point! No, even with a unique index on `countries.name`, the database will still quite happily allow me to:

```
INSERT INTO countries (name) VALUES ('Australia');INSERT INTO countries (name) VALUES ('AUSTRALIA');INSERT INTO countries (name) VALUES ('aUsTrAlIa');
```

So when I perform a case-insensitive search as previously discussed, I'll end up with three (count 'em 3) records. Thankfully, there is a solution (of sorts): expression indexes.

PostgreSQL allows you to create indexes based on expressions, say for example `LOWER(name)`, allowing us to create a unique, case-insensitive index as simply as:

```
CREATE UNIQUE INDEX index_countries_on_name ON countries (**LOWER(**name**)**);
```

Ok, so perhaps you knew this already and you're wondering what all this has to do with Rails? Well I'm glad you asked.

Rails (as of 1.2) has a new option for `validates_uniqueness_of` named, oddly enough, `case_sensitive`. This is assumed to be `true` by default (meaning all searches are case-sensitive). Set it to `false` however and you'll magically get validation queries that look like:

```
SELECT * FROM countries WHERE (**LOWER(**countries.name**)** = 'australia') LIMIT 1;
```

To compliment this feature, I've recently enhanced the [RedHill on Rails Plugins](https://github.com/harukizaemon/redhillonrails) in two interesting (and hopefully useful) ways.

The first is in the [core](https://github.com/harukizaemon/redhillonrails/tree/master/redhillonrails_core) and supports the creation of case-insensitive indexes during schema migration:

```
add_index :countries, [:name], :unique => true, **:case_sensitive => false**
```

The second is in [schema validations](https://github.com/harukizaemon/redhillonrails/tree/master/schema_validations) and causes `case_sensitive => false` to be passed as an option to `validates_uniqueness_of` whenever a case-insensitive index is detected.

(I also looked at the possibility of automagically surrounding query parameters, etc. with `LOWER()` inside `find` methods but given the myriad forms queries can take, it seems altogether too difficult for my feeble mind at this point.)

The upshot of all this is that at the very least, it should now be possible to add case-insensitivity to your queries and be assured that (bugs not withstanding) the performance of your application won't suddenly plummet as a consequence.
