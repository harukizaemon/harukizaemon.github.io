---
layout: post
title: "Acts As Teapot"
alias: /2009/01/acts-as-teapot.html
categories:
---
No, it's not April Fools yet but I thought I'd get in early this year. [Acts As Teapot](http://github.com/harukizaemon/acts_as_teapot) is a Ruby on Rails plugin that ensures your Ruby on Rails applications conform to [RFC2324](http://www.ietf.org/rfc/rfc2324.txt). My assumption here is that your application is not a coffee pot and therefore does not understand the Hyper Text Coffee Pot Control Protocol (HTCPCP/1.0). Thus, if ever a BREW request or any other request with the Content-Type set to "application/coffee-pot-command" is received, the server will respond with 418 I’m a teapot.
