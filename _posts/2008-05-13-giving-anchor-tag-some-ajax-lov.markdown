---
layout: post
title: "Giving the Anchor tag some Ajax Lov'n"
alias: /2008/05/giving-anchor-tag-some-ajax-lov.html
categories:
---
It seems I'm forever needing to submit links using an XMLHttpRequest rather than the default full-page refresh. One approach commonly used in the Rails community is to render each link with the JavaScript already in place. My preferred approach however is to keep the HTML as free from JavaScript as possible and unobtrusively add behaviour using [LowPro](http://www.danwebb.net/2006/9/3/low-pro-unobtrusive-scripting-for-prototype).

LowPro already comes with a built-in behaviour for links but sometimes I need something little more complex than simply submitting the request and so I usually end up doing the following:

``` javascript
anchor = ...;
new Ajax.Request(anchor.href, { method: "get", parameters: ... });
```

Granted that's not a lot of effort but it still felt as though I were repeating myself and that the overall intention of my code was largely obscured by the infrastructure. It then struck me that submitting a form using the [Prototype JavaScript framework](http://www.prototypejs.org) is almost trivial:

``` javascript
form = ...;
form.request({ parameters: ... });
```

So I cooked up a version for anchors as well:

``` javascript
Element.addMethods("A", {
  request: function(anchor, options) {
    new Ajax.Request(anchor.href, Object.extend({ method : "get" }, options || {}));
  }
});
```

Now I can submit links in pretty much the same was as I do forms:

``` javascript
anchor = ...;anchor.request({ parameters: ... });
```

I'm wondering what other possibilities might occur were I to add a `serialize()` method to extract the request parameters.
