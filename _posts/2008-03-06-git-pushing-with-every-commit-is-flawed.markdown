---
layout: post
title: "Git: Pushing with Every Commit is Flawed"
alias: /2008/03/git-pushing-with-every-commit-is-flawed.html
categories:
---
Yesterday, both James and Marty indicated that one thing they didn't like with Git is that not every commit is pushed to the server. Actually what they were complaining about wasn't that, it was that they couldn't see what I was working on _until_ I pushed to the server. I complained that kinda defeated the purpose of Git -- to push on every commit -- but couldn't articulate why. I now can.

Last night I made a bunch of changes, a spike, to set me up for today. I dutifully pushed those changes to the server (as requested) and went to bed. Unknown to me at the time, though completely predictable, those changes (a spike remember) broke the app. Amusingly, only a day earlier, James had quipped that "we're doing agile. shouldn't the repository always reflect working software? what's with all the broken functionality?"

Git allows me to make micro changes, to spike stuff all day long, and eventually when I'm confident there's something for the world to see I can push those changes back to the origin. It gives me far greater control and flexibility over the nature and granularity of the changes I make. It means I can have greater trust that changes I make WONT affect other developers or "customers". These benefits are immediately negated when I push on every commit.

Why don't we have this problem with something like subversion? Good question. I think the answer is simple: because subversion forces me to work in a way that ensures that everything I commit is in full working order. It also means I tend to commit larger chunks and work a lot more on branches. I would argue that if that's what we want to do then using Git doesn't buy us much except "offline" mode which, by definition, doesn't satisfy the "transparency" issue anyway.

I'm not using Git as a subversion replacement. I'm using it precisely because it allows me to do "more" than subversion. With this comes cultural change. I respect the need for greater transparency but I don't believe that pushing (at least to the master branch) on every commit is the solution.

So perhaps flawed is too strong a word. Perhaps Ill-considered would be more appropriate.
