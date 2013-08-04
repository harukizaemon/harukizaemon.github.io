---
layout: post
title: "Automatically Validate Uniqueness of Columns with Scope"
alias: /2006/09/automatically-validate-uniqueness-of.html
categories:
---
The first cut at [Schema Validations](https://github.com/harukizaemon/redhillonrails/tree/master/schema_validations) only applied `validates_uniqueness_of` for single-column unique indexes. This removed 80% of the cases in my code base but there were still cases where a scope was specified that lingered. Not any more.

The plugin now automatically generates `validates_uniqueness_of` with `scope` for multi-column unique indexes as well.

As always, there are some assumed conventions -- which I believe will handle close to 99% of cases -- around how to decide which column to validate versus which columns to consider part of the scope. The column to validate is chosen to be either:

<ol>* The last column in the index definition not ending in ‘_id’; or simply
* The last column in the index definition.</li></ol>

With all remaining columns considered part of the scope, following, what I believe to be, a typical typical composite unique index column ordering.

So, for example, given either of the following two statements in your schema migration:

```
add_index :states, [:country_id, :name], :unique => trueadd_index :states, [:name, :country_id], :unique => true
```

The plugin will generate:

```
validates_uniqueness_of :name, :scope => [:country_id]
```

My next stop is to have a look at simple column constraints such as `IN('male', 'female')` and turn them into `validates_inclusion_of :gender, :in => ['male', 'female']`.

Perhaps tomorrow :)
