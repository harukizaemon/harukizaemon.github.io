---
layout: post
title: "Getting Too Fancy with HTTP Response Codes"
alias: /2008/04/getting-too-fancy-with-http-response.html
categories:
---
As part of my adoption of REST and all its goodness, I've started using HTTP response `s more, responsibly ;-) So, for example, instead of always returning 200 (OK) for just about everything, I'm using 201 (Created) with a Location header set to the new URL after a POST. For PUT, I send back 204 (No Content), a 404 (Not Found) after a GET for a resource that no longer exists, and a good old 200 (OK) after a successful DELETE or GET.

Interestingly, in the system I'm developing at present, an update (PUT) might actually cause a resource to move due to the application of server-side business rules. In this case, the 204 response also sets the Location header so that the client knows where it can be found.

All this was working beautifully on my local machine using both Safari and Firefox so once I was happy with the result I deployed it into the remote test site and started playing in FireFox. So far so good. Everything checks out. Next let's try Safari...not so great.

Some bits of the application worked just fine but others seemed to have no effect. Then mysteriously things would start working again. Even stranger was the fact that hitting the browser's refresh button had no effect either.

At first I suspected that nesting Ajax calls might be to blame but as everything seemed to work perfectly in FireFox and a Google search turned up nothing, I decided to do some more investigation.

I logged in to the server box and tailed the logs for signs of life. Everything looked normal. All the expected requests and responses were there but still nothing client side. Using Safari's new Network Timeline I could see what the browser thought was going on. All the requests and responses were there but something was odd. In all but a few cases, the response code was 204 (No Content). I double checked the server logs but no, the server was definitely sending back the correct responses; a mixture of 204, 200 and 404 as appropriate.

On a hunch I went back and re-read the [HTTP Status Codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) document and in particular the definition for 204:

> The server has fulfilled the request but does not need to return an entity-body ... **the client SHOULD NOT change its document view** from that which caused the request to be sent ...

That might actually explain it. If Safari received a 204 and interpreted that to mean "Don't change anything" then hitting refresh would indeed have no effect even if my code subsequently went on to perform more asynchronous requests as a result.

So, I dutifully changed all the 204s to 200s and voila! Safari started to behave just as expected and FireFox continued to work as had previously.

I've also noticed a difference in the way both browsers handle 303 (Redirect) from within an XML HTTP Request: Safari performs the redirect and keeps all the headers as per the original, whereas FireFox seems to essentially construct an entirely new request. The upshot is that you can't actually detect (server-side) an XML HTTP Request from FireFox if it is the result of a redirect.

I'm really not sure why the two browsers have such differing opinions of what the appropriate behaviour should be in either case but I hope this helps some other poor sod keep from pulling their hair out.
