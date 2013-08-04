---
layout: post
title: "Perhaps Pluralize Means Something Other Than I Thought"
alias: /2006/06/perhaps-pluralize-means-something-other.html
categories:
---
Besides the unfortunate use of a _z_ where there really ought to be an _s_ (yes, yes, I'm Australian), what does this word really mean?

> [**pluralize**](http://dictionary.reference.com/browse/pluralize) v. tr.<ol>* To make plural.
* Grammar. To express in the plural.</li></ol>

OK, that's pretty much what I thought it meant. So then riddle me this Batman:

```
simon$ ./script/consoleLoading development environment.&gt;&gt; "country".pluralize=&gt; "countries"&gt;&gt;
```

Good good. Just as expected. So, now let's try something else:

```
&gt;&gt; "countries".pluralize=&gt; **"country"**&gt;&gt;
```

Huh??? Since when did _country_ become the plural of _countries_?
