---
layout: post
title: "Don't blame the brace"
alias: ["/2003/12/dont-blame-the-brace.html", "/2003/12/dont-blame-brace.html"]
categories:
---
Everyone remember this:

``` c
int main(int argc, char *argv[]) {
    return 0;
}
```

The C programming language is the the ancestor of Java that no one likes to talk about anymore. One of the overriding imperitives when developing the syntax of the language was ease of compilation. Curly braces make it easy for a parser to work out where blocks of code start and end. So naturally, we humans realised we could use them for the same purpose. Only we also worked out, smart little things that we are, that code is much easier to read if nested blocks are indented. This way the blocks stand out even more.

(As an aside, languages like [Python](http://www.python.org) use this indenting to delineate blocks of code which is kinda cool in my book but certainly not to everyones taste).

Anyway, next thing you know, the C language takes off. Developers used curly braces on new lines and indenting to make their code more readable. You could easily match up start and end blocks, you could see the nesting. Ooh it was a happy time for all. Sure made a change from all that assembler code right? Great mountains of code are produced. Functions from tens to even hundreds of lines long, containing levels so deep that, once again, it became hard to tell where blocks of code started and end.

Enter the end-of-block-comment! "Of course!" exclaimed the developers, "lets mark the end of each block with a comment to indicate what to look for when searching for the start. If it's a `while` loop we'll add `/* while */` at the end. If it was a `for`, we'd finish with a `/* for */` , etc. What a sensible idea" they chuffed.

Years later, we C programmers ventured forth into the brave new world of C++ and sometime thereafter, into Java, bringing with us all our accumulated knowledge and wisdom.

When I wrote [Simian](/simian) one of the first things I noticed was that removing curly braces from the input stream improved the overall effectiveness of the matching. Why? Because by removing the curly braces, I reduce the signal/noise ratio. Yup, curly braces are noise. They started off as a neat way of simplifying parsing and still add little value over and above that.

"But, but but..." I hear you complain loudly. "What about all the examples you've just given. Surely that counts for something?"

Sure. If you have methods that are hundreds of lines long. If you have blocks of code nested 3, 4 even 5 or more levels deep. If you have boolean expressions containing dozens of conditions, then you know what? I might just have to agree with you.

But I don't! I have methods with an average length of no more than probably 15 lines with an upper bound (enforced by [checkstyle](http://checkstyle.sf.net)) of 30. I'm talking executable statements here not simply `EOL` characters.

Make your code more readable by simplifying the code! Break out those conditionals. Break out those nested `for` loops. Stop nesting `try-catch-finally` blocks. Break up those ugly `switch` statements and turn them all into private methods. Make them all static final if you're worried about performance (not that it buys you much these days). The fact is that compilers can do wonders with inlining of private methods these days. Compilers are much smarter now than when Mr. Kernighan and Mr. Ritchie were playing around with their lovely new C language compiler.

Importantly, I believe the usual arguments over whether one way is esthetically more or less pleasing than another are a waste of time. [It's not a democracy](/blog/2003/12/03/its-not-a-democracy) but it is a benevolant dictatoriship so if you like it that way let's all agree and get over it. Just please stop using the curly brace as an excuse to write crappy code.
