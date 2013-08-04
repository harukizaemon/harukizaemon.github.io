---
layout: post
title: "Using Lucene To Find A Date"
alias: /2004/11/using-lucene-to-find-date.html
categories:
---
For the next 3 weeks (and for the past few), I'm the _`DefectController`_. I get to watch the defects roll in, assess them, and hand them out to the approriate developer (which may be me). Last week I saw a rather odd defect pass by:

```
org.apache.lucene.queryParser.ParseException: Too many boolean clauses when performing date range search.
```

My first reaction was puzzlement replaced shortly thereafter with shock as I thought through the problem. It occured to me that the most obvious cause would be the unthinkable: the developer must have enumerated every possible date in the range and included them ALL in one gigantic `OR` condition.

A bit of [groking](http://dictionary.reference.com/search?q=grok) later and shock turned to horror. Fortunately, the developer had not done as I suspected. They had done the correct thing and generated the correct [Lucene](http://jakarta.apache.org/lucene) criteria in the form:

```
dateOfBirth:[19700801 TO 20030615]
```

Unfortunately, that left only one option: It must be Lucene!

Two minutes on Google and the [_`BuildController`_](http://www.mikemelia.com) turned up, among others, [this link](http://jira.atlassian.com/browse/JRA-3127). Yes indeed, it seems, Lucene does enumerate ALL possible dates. In fact depending on the granularity, it will end up enumerating all possible **seconds**! Apparently this is not a bug nor even a feature but a "known behaviour".

So now here's the thing that puzzles me. It would appear, from the [documentation](http://jakarta.apache.org/lucene/docs/queryparsersyntax.html), that string ranges are also supported allowing us to find say, people where:

```
name:[Albert TO Betty]
```

This being the case, does Lucene enumerate ALL possible names? I find that hard to fathom. If it does, then I give up now. If not, then couldn't we just encode the dates as umambiguous, comparable strings? something like:

```
dateOfBirth:[19700801 TO 20030615]
```

Look familiar? It should. I just copied and pasted the original example. But if this time around we consider the dates as strings of the form `yyyyMMdd` instead of attributing any special notion of _date_, wouldn't that solve the problem? Wouldn't that also easily allow us to perform partial range searches that include say only the year or year and month?

A Lucene expert I am not but all the links we found suggesting various other "work-arounds" (one of which suggested upping the limit on the number of clauses!) seemed little more than hacks. So, please, please, please tell me I've missed something obvious because the solution really **does** seem that simple to my feeble bwain.
