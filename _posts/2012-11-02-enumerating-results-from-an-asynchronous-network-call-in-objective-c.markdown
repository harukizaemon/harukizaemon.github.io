---
layout: post
title: "Enumerating results from an asynchronous network call in Objective-C"
date: 2012-11-02 21:00
categories:
---
I have a facade over an asynchronous network call—that also performs pagination, ie multiple network calls in order to handle very large result sets—along the lines of this but I now want to use it in a context where knowing when it's finished iterating is important (e.g in an `NSOperation`):

``` objective-c
[myClient enumerateFoosAtURL:URL usingBlock:^(id foo, BOOL *stop) {
  ...
} failure:^(NSError *error) {
  ...
}];
```

The simplest thing might be to add an extra block but that is so sucky I nearly vomited in my mouth just typing out the example below:

``` objective-c
[myClient enumerateFoosAtURL:URL usingBlock:^(id foo, BOOL *stop) {
  ...
} success:^{
  // we're done
} failure:^(NSError *error) {
  ...
}];
```

A less sucky option might be to indicate if there are more to come:

``` objective-c
[myClient enumerateFoosAtURL:URL usingBlock:^(id foo, BOOL more, BOOL *stop) {
  ...
  if (!more) {
    // we're done
  }
} failure:^(NSError *error) {
  ...
}];
```

Yet another option that I think I like the most might be to simply transform the semantics of the original into this:

``` objective-c
[myClient enumerateFoosAtURL:URL usingBlock:^(id foo, BOOL *stop) {
  ...
} finished:^(NSError *error) {
  if (error) {
    // an error occurred
  } else {
    // we're done
    ...
  }
}];
```

**Update** [Tony Wallace](http://tonywallace.com) suggested that he prefered the

> "less sucky option" because it makes it more obvious that the method returns paginated results.

And I have to say I agree with him on that. The only fly in the oitment however, turns out to be some filtering performed within the API methods which meant that determining if there were more results would have required some fancy look-ahead code.

In the end I went with my last option. It seems to have worked out rather painlessly :)
