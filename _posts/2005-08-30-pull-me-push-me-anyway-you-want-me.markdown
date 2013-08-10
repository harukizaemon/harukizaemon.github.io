---
layout: post
title: "Pull Me Push Me Anyway You Want Me"
alias: /2005/08/pull-me-push-me-anyway-you-want-me.html
categories:
---
I recently needed some code to parse text streams. Conceptually, the logic was pretty simple: break a stream into words and combine consecutive words into "phrases". So for example, the stream `"have a nice day"` might be broken into the phrases: `"have"`, `"have a"`, `"a"`, `"have a nice"`, `"a nice"`, `"nice"`, `"a nice day"`, `"nice day"` and `"day"`.

The problem is that I'm not going to own this code in the end, someone else is (permission was given to publish the code) and after numerous consulting gigs over the years, I've become very careful to avoid leaving behind any [Alien Artifacts](http://www.eaves.org/blog-archive/000071.html). So I produced two versions, each producing identical output but based on very different designs, in the hope that at least one of them would fly.

I'll start with my first approach which, IMHO, is pretty easy to understand -- well I wrote it so I guess it is for me anyway -- and an interface that accept tokens as they are parsed (a [Visitor](http://en.wikipedia.org/wiki/Visitor_pattern) of sorts):

```
public interface TokenVisitor {public void onToken(String token);}
```

Classes that implement `TokenVisitor` will be notified anytime a token becomes available for processing.

Next, we want to create phrases from tokens and print them to the console so, we also need a class that takes the tokens it receives, creates phrases of between 1 and `maximumPhraseSize` tokens and sends them to another visitor (a [Decorator](http://en.wikipedia.org/wiki/Decorator_pattern)):

```
import java.util.LinkedList;import java.util.Queue;public class PhraseBuilder implements TokenVisitor {private final Queue&lt;StringBuilder&gt; builders = new LinkedList&lt;StringBuilder&gt;();private final TokenVisitor output;private final int maximumPhraseSize;public PhraseBuilder(TokenVisitor output, int maximumPhraseSize) {assert output != null : "output can't be null";assert maximumPhraseSize &gt; 0 : "maximumPhraseSize can't be &lt; 1";this.output = output;this.maximumPhraseSize = maximumPhraseSize;}public void onToken(String token) {assert token != null : "token can't be null";for (StringBuilder builder : this.builders) {builder.append(' ').append(token);this.output.onToken(builder.toString());}this.output.onToken(token);this.builders.add(new StringBuilder(token));if (this.builders.size() == this.maximumPhraseSize) {this.builders.remove();}}}
```

And finally, some sample usage:

```
    Lexer lexer = new Lexer(_stream_, new PhraseBuilder(new TokenVisitor() {public void onToken(String token) {System.out.println(token);}}, 3));lexer.run();
```

Here, an instance of the `Lexer` class (not shown for the sake of brevity) simply breaks `_stream_` into words (tokens) and calls `TokenVisitor.onToken()` passing each one in turn. The phrase builder acts as the first level visitor and passes its results on to the next level visitor, an anonymous inner class that simply prints each token to the console.

For many people it seems, this _push_ style of processing is not only unfamiliar, but downright peculiar. When I show this style of code to developers -- especially junior developers and non-technical people -- they find it hard to grasp. Not so much the logic in `PhraseBuilder` but what stumps many people is the "complexity" of the overall "pattern" and in particular the usage. For many it seems, this approach is all a bit "backwards".

So for comparison, here's an example of a more conventional _pull_ mechanism, starting with the interface:

```
public interface TokenStream {public String nextToken();}
```

This time we have an interface which we can call to get (pull) the next token rather than be notified (push) as in the previous example. The method `nextToken()` returns `null` to signify the end of the stream -- no more tokens.

Next up, the phrase builder. Again, we'll implement the interface -- to allow chaining -- but this time we're relying on a pull rather than push mechanism to get tokens:

```
import java.util.LinkedList;import java.util.Queue;public class PhraseBuilder implements TokenStream {private final Queue&lt;StringBuilder&gt; builders = new LinkedList&lt;StringBuilder&gt;();private final Queue&lt;String&gt; phrases = new LinkedList&lt;String&gt;();private final TokenStream input;private final int maximumPhraseSize;public PhraseBuilder(TokenStream input, int maximumPhraseSize) {assert input != null : "input can't be null";assert maximumPhraseSize &gt; 0 : "maximumPhraseSize can't be &lt; 1";this.input = input;this.maximumPhraseSize = maximumPhraseSize;}public String nextToken() {return this.hasNextToken() ? this.phrases.remove() : null;}private boolean hasNextToken() {if (this.phrases.isEmpty()) {makePhrasesWithToken(this.input.nextToken());}return !this.phrases.isEmpty();}private void makePhrasesWithToken(String token) {if (token != null) {this.builders.add(new StringBuilder());for (StringBuilder builder : this.builders) {if (builder.length() &gt; 0) {builder.append(' ');}builder.append(token);this.phrases.add(builder.toString());}if (this.builders.size() == this.maximumPhraseSize) {this.builders.remove();}}}}
```

Holy schmokes! That's a whole lotta code with multiple queues and extra private methods. Surely it can't be that complicated?

Ok, so how about some sample usage:

```
    PhraseBuilder builder = new PhraseBuilder(new Lexer(_stream_));String token;while ((token = builder.nextToken()) != null) {System.out.println(token);}
```

Sheesh! That's pretty simple. Far simpler than the code in the first example and pretty obvious what it's doing really and when I show this kind of code to people, they tend to respond with "Oh, I see. That makes sense."

Considering the relative complexity of the second phrase builder to the first, I find this all somewhat odd: the original example took me about ten minutes to code up and test; the second probably around twenty -- at first I tried to do it from sratch but I gave up in the end and resorted to a brute-force conversion approach not dissimilar to that required to convert a recursive algorithm to an iterative one.

In fact, push versus pull is very similar to recursive versus iterative: people tend to have the same comprehension difficulties with recursion as they do with a push-style calling mechanism. It's a strange thing really because although conceptually simpler, an iterative approach can often be much harder to implement than a recursive one; similarly, a pull-mechanism can be harder to implement than a push-mechanism.

It should be obvious by now that I have a preference for recursion and push-style processing. For one thing it removes lots of `getXxx()` methods which is no doubt why I like closures in languages such as Smalltalk, Ruby, Groovy, JavaScript, etc. I also find it forces me to create [lots of little classes]({% post_url 2004-02-04-lots-of-little-classes %}) that [do one thing and do it well](http://www.oreilly.com/catalog/bfljava/chapter/ch03.pdf).

That said, I can also see why (and under what circumstances) pull is more attractive: it's usually easier to manage flow-control than with push. Often the difference between the two comes down to where state is being maintained: In the case of pull, state is maintained inside the stream; for push, state is maintained inside the parser.  No doubt why many people have switched from using [push-parsing](http://www.saxproject.org/) to [pull-parsing](http://www.xmlpull.org/) for XML.

**Updated (1 September 2005)**

Thanks to Kris for pointing out the typos in my code. I had copied the examples from and made some on-the-fly modifications to reduce their size but it seems I missed a few things -- oops.
