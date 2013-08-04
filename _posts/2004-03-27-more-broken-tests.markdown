---
layout: post
title: "More Broken Tests"
alias: /2004/03/more-broken-tests.html
categories:
---
Following on from my entry yesterday on [broken tests](/blog/2004/03/26/broken-tests) I came across another interesting problem I see from time to time in peoples test code.

Here's a very simple example to demonstrate. As far as I'm concerned it's a big fat broken test:

```
public class FileBuilderTest extends TestCase {public void testHeaderIsWritten() throws Exception {StringWriter writer = new StringWriter();FileBuilder builder = new FileBuilder(writer);BufferedReader reader = new BufferedReader(new StringReader(writer.toString()));String line = reader.readLine();assertNotNull("too few lines", line);assertEquals(FileBuilder.HEADER, line);...}}&nbsp;public class FileBuilder {public static final String HEADER = "# Created by " + FileBuilder.class.getName();private final Writer _writer;public FileBuilder(Writer writer) throws IOException {if (writer == null) {throw new IllegalArgumentException("writer can't be null");}_writer = writer;writer.write(HEADER);}}&nbsp;
```

Seems pretty sensible right? We check to make sure that the header gets written to the stream. Putting aside the _are tests for testing or design_ debate for a moment, one thing remains clear - a test should break if I change the class in anyway that breaks the intended behaviour.

Right?

If so, then this test is truly crippled. Don't believe me? Well just change the value of header to some absolute garbage and re-run the test.

Did it break?

No. Why? Because the test never checks that what gets written is what is supposed to be written. Instead, it is simply checking that whatever header the `FileBuilder` has defined is what gets written. So anyone can inadvertently (or deliberately as we have done)  change that header and the test never breaks.

I'd rather see this:

```
public class FileBuilderTest extends TestCase {public void testHeaderIsWritten() throws Exception {StringWriter writer = new StringWriter();FileBuilder builder = new FileBuilder(writer);BufferedReader reader = new BufferedReader(new StringReader(writer.toString()));String line = reader.readLine();assertNotNull("too few lines", line);assertEquals("# Created by " + FileBuilder.class.getName(), line);...}}&nbsp;public class FileBuilder {private static final String HEADER = "# Created by " + FileBuilder.class.getName();private final Writer _writer;public FileBuilder(Writer writer) throws IOException {if (writer == null) {throw new IllegalArgumentException("writer can't be null");}_writer = writer;writer.write(HEADER);}}&nbsp;
```

I recall one of the things I found disturbing when reading (one of) Mr. Becks TDD book being his suggestion that our first example is in fact good practice because we are removing duplication. Personally, I don't care about duplication in tests nearly (if at all) as much as I do in production code.

An interesting variation I heard about involved a developer essentially coding the same algorithm in both the production class and the test so they could verify the results.

At any rate, hopefully you can see the similarity in both cases - the tests weren't explicit about what the expected results should be instead using a "calculated" value. I'd much rather see the test explicitly state exactly what it expected to be written. That way if someone accidentally does say a find+replace within our `FileBuilder` code, we will catch it.
