---
layout: post
title: "MT 3.2 Atom Feed Template Problem"
alias: /2005/09/mt-32-atom-feed-template-problem.html
categories:
---
**Updated 17 October 2005:** Seems there are all sorts of problems with the template. I ran it through a [feed validator](http://feedvalidator.org/) service and I'm frankly staggered anyone can actually read my blog. It seems the content type can be left as "html" after all but I needed to change a whole-lotta other stuff to get it to pass. Interestingly, Safari now _sometimes_ manages to read the whole feed just fine. Go figure? Luckily I recently switched to using PulpFictionLite as my news reader. Safari just doesn't cut it I'm afraid. Anyway, it's just another example of what can happen when I try playing with things about which I understand very little. *Sigh*.

I noticed that since I upgraded to MT 3.2 my atom feed -- the main one since the switch -- wasn't being displayed correctly in Safari's RSS Reader. The culprit: a slight problem with the content-type in the template for `atom.xml`.

To correct the problem, find where it says `&lt;content type="html" ...` and change `"html"` to `"text/html"` and all should be fine again.

The `index.xml` template works just fine though I've yet to bring either the `comments.xml` or `index.rdf` up-to-date. The former is just laziness -- I need to remember how I converted the original `index.html` template; The latter is because MT seem to have dropped support for RSS formats &lt;2.0. Neither of these is really an issue as they worked before and continue to work now.

FWIW, I'm considering removing `index.rdf` sometime soon anyway as I'm pretty sure almost every RSS reader now supports at least RSS 2.0 and most-likely Atom as well.`
