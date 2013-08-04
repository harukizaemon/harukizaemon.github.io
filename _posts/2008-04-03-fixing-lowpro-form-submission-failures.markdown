---
layout: post
title: "Fixing lowpro form submission failures"
alias: /2008/04/fixing-lowpro-form-submission-failures.html
categories:
---
I finally worked out why my forms weren't submitting when the user hits the ENTER key. [lowpro](http://lowpro.stikipad.com/home/) serializes the button that was clicked along with any other parmeters when submitting a form via AJAX. However, when the user hits enter under FireFox, there is no button and consequently the browser barfs. Safari on the other hand tries to be too helpful and triggers an onclick event for the first submit button (which is why I never noticed it).

So anyway, rather than try to be too clever myself, I simply changed the parameter serialization in `Remote.Form.onsubmit` to look like:

``` javascript
parameters : this.element.serialize({ submit: this._submitButton ? this._submitButton.name : null })
```

Problem solved.
