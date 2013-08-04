---
layout: post
title: "Adding a comment feed"
alias: /2004/02/adding-a-comment-feed.html
categories:
---
I found an article detailing [how to add a comment feed](http://tweezersedge.com/archives/2003/10/000157.html) to a [Movable Type](http://www.movabletype.org) blog. I made a few changes (as one does) and now you can subscribe to the comments on this blog as well as the main feed. So for anyone who's interested, here's the template:

``` xml
<?xml version="1.0" encoding="<$MTPublishCharset$>"?>
<rss version="2.0" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:sy="http://purl.org/rss/1.0/modules/syndication/" xmlns:admin="http://webns.net/mvcb/" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
  <channel>
    <title><$MTBlogName remove_html="1" encode_xml="1"$> - Comments</title>
    <link><$MTBlogURL$></link>
    <description>Latest comments on <$MTBlogDescription remove_html="1" encode_xml="1"$></description>
    <dc:language>en-us</dc:language>
    <dc:lastBuildDate><$MTDate format="%Y-%m-%dT%H:%M:%S"$><$MTBlogTimezone$></dc:lastBuildDate>
    <admin:generatorAgent rdf:resource="http://www.movabletype.org/?v=<$MTVersion$>" />
    <sy:updatePeriod>hourly</sy:updatePeriod>
    <sy:updateFrequency>1</sy:updateFrequency>
    <sy:updateBase>2000-01-01T12:00+00:00</sy:updateBase>
    <MTComments lastn="20">
      <item>
        <title>Comment on "<MTCommentEntry><$MTEntryTitle remove_html="1" encode_xml="1"$></MTCommentEntry>"</title>
        <link><MTCommentEntry><$MTEntryLink$></MTCommentEntry>#comments</link>
        <description><$MTCommentBody remove_html="1" encode_xml="1"$><p>- <$MTCommentAuthor remove_html="1" encode_xml="1"$></p></description>
        <guid isPermaLink="false">comment<$MTCommentID pad="1"$>@<$MTBlogURL$></guid>
        <dc:pubDate><$MTCommentDate format="%Y-%m-%dT%H:%M:%S"$> <$MTBlogTimezone no_colon="1"$></dc:pubDate>
      </item>
    </MTComments>
  </channel>
</rss>
```
