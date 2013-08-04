---
layout: post
title: "Multiple Mac (Tiger) Logins for Testing"
alias: /2005/09/multiple-mac-tiger-logins-for-testing.html
categories:
---
I do all my development on my mac powerbook which, up until today, has had only one account: mine. But this morning, as we were trying to solve the last of the MT issues, I needed another login with which to quickly try something out.

It seem as though there was something peculiar about my login that was causing some of the problems so, I open Accounts and created a test user. So far nothing special about his -- I'm sure you create test accounts all the time-- however it's always annoyed me that you have to logout to switch users. I mean really, this is _supposed_ to be unix right?.

Tiger (OS X 10.4) has a nifty "Fast User Switching" option which allows you to switch between users while leaving all the applications running. I had heard about this feature some time ago and thought to myself "phe! what do I care?" but I'd never thought about it in the context of testing.

To enable fast user switching, open "Accounts" in "System Preferences" and and click "Login Options". Tick the box titled "Enable fast user switching" (it's down the bottom). You should then see a list of accounts in the top-right of the menu-bar.

The great thing about this for me is that I can quickly blow away the account and re-create it; or have multiple accounts all logged in to my apps at once, all from the one machine. Of course the fact that I need to get down to manual testing says more than anything but alas, in this case, I didn't have much choice. Besides, as [James](http://www.redhillconsulting.com.au/blogs/james) has berated me for in the past: Just because you have automated tests, doesn't mean you can avoid manual testing as well.

On a side note, I also love SSH as it allowed me to install a public-key for the MT support guy to login to my shell account without needing me to create new users -- which I can't do anyway because it's not my server :)

**Update (2 September 2005):** I noticed today that Redstone Software have a [new version of OSXvnc](http://www.redstonesoftware.com/multidesktop.html) that supports this, allowing you to view (and control) the other logins without even switching! It's a free download; as is [Chicken of the VNC](http://sourceforge.net/projects/cotvnc/) -- A VNC client -- which you'll also need.
