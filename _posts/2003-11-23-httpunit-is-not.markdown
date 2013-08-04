---
layout: post
title: "HTTPUnit is NOT!"
alias: /2003/11/httpunit-is-not.html
categories:
---
I hope that I'm not the only one that sees HTTPUnit as a misnomer. Yes it may well easily plug into the JUnit framework but by definition, any tests involving HTTP and Servlets/JSPs are not unit tests. They are more likely functional tests, at best integration tests.

As my good friend James Ross points out, HTTPUnit is really a rich http client library. Infact I did some work for a client of mine some months ago to screen scrape web pages and after much searching, I decided that JWebUnit which is built on HTTPUnit was the best choice. The only draw back is it has some very trivial dependencies on junit classes.

I would have been happier if HTTPUnit had been called HTTPTest. For that matter HTTPClient as it doesn't have any real dependencies on JUnit save a few convenience classes for in-servlet testing

But I digress. Why does this all bug me so much? Because HTTPUnit is named so, it's easy for developers to convince their managers that they are writing unit tests when they're not.
