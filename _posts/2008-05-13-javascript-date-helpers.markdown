---
layout: post
title: "JavaScript Date Helpers"
alias: /2008/05/javascript-date-helpers.html
categories:
---
It's all the rage these days to have timestamps displayed in words to indicate how long ago some event occurred. You know something like "less than a minute ago" or "about 2 months ago", etc. You'll see plenty of examples on news sites, blog entries, and bug tracking tickets to name but a few.

If you've ever had to build this kind of thing in Rails you'll be familiar with all the [Date Helper methods](http://api.rubyonrails.org/classes/ActionView/Helpers/DateHelper.html) that make the task pretty trivial. The problem is that the result is fixed to whatever the date was when the page was rendered and as a result these timestamps go stale very quickly.

Save refreshing the page every minute -- or hour or whatever -- just to update the times, I figured what was needed was a little but of client-side action. Rather than send the text in the HTML, I decided to instead send the raw timestamps and have the browser periodically generate the textual representation.

To this end, I blatantly copied two methods from the afore-mentioned Rails helper -- `distance_of_time_in_words(from, to)`, and `time_ago_in_words(from)` -- and, taking some liberties along the way, converted them to JavaScript:

``` javascript
function distanceOfTimeInWords(to) {
  var distance_in_milliseconds = to - this;
  var distance_in_minutes = Math.abs(distance_in_milliseconds / 60000).round();
  var words = "";

  if (distance_in_minutes == 0) {
    words = "less than a minute";
  } else if (distance_in_minutes == 1) {
    words = "1 minute";
  } else if (distance_in_minutes < 45) {
    words = distance_in_minutes + " minutes";
  } else if (distance_in_minutes < 90) {
    words = "about 1 hour";
  } else if (distance_in_minutes < 1440) {
    words = "about " + (distance_in_minutes / 60).round() + " hours";
  } else if (distance_in_minutes < 2160) {
    words = "about 1 day";
  } else if (distance_in_minutes < 43200) {
    words = (distance_in_minutes / 1440).round() + " days";
  } else if (distance_in_minutes < 86400) {
    words = "about 1 month";
  } else if (distance_in_minutes < 525600) {
    words = (distance_in_minutes / 43200).round() + " months";
  } else if (distance_in_minutes < 1051200) {
    words = "about 1 year";
  } else {
    words = "over " + (distance_in_minutes / 525600).round() + " years";
  }

  return words;
};

Date.prototype.timeAgoInWords = function() {
  return this.distanceOfTimeInWords(new Date());
};
```

Now all I do is periodically invoke a function that calls one or other of these methods and updates the text of whatever display element is appropriate. Even better, because the raw timestamps have timezone information in them, the display doesn't suffer from, in my case here in Australia, always being 10 hours out because the server is sitting in the US with a US date/time.
