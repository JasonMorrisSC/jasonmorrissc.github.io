---
toc: true
title: Demo of Blawx Integration with Docassemble over API for Rules as Code
description: >-
  Blawx is a tool for allowing people to easily encode written legal rules, and
  power other applications over the web with those encodings.
date: '2019-10-10T05:07:19.438Z'
categories: []
keywords: [blawx]
slug: >-
  /@roundtablelaw/demo-of-blawx-integration-with-docassemble-over-api-for-rules-as-code-1c631a3c24fc
---

[Blawx](https://www.blawx.com) is a tool for allowing people to easily encode written legal rules, and power other applications over the web with those encodings.

We have now built our first proof-of-concept web integration, with [docassemble](https://docassemble.org), demonstrating how rules you build in Blawx can be used to give legal reasoning capabilities to any other tool on the web.

### The Rules: Rock Paper Scissors

The first step is to encode the rules that you need. For this demo, we are using the rules of Rock, Paper, Scissors, aka Rochambeau, aka Boulder, Parchment, Shears.

The rules were encoded in Blawx, and then [published online](https://www.blawx.com/rps.blawx).

If you’d like to see how that encoding is done, and play with the published code, see [this tutorial post on Blawx.com](https://www.blawx.com/2019/07/tutorial-encoding-rock-paper-scissors-in-blawx/#page-content). A portion of the encoding, the rule for determining who wins a game, looks like this:

![](/0__PxY__l__mU95LFpiSx.png)

### The Integration: Docassemble

Integrating any tool with Blawx requires translating from what that tool knows to what the Rules are expecting.

That translation is done in Blawx itself. First, we imported the rules to Rock, Paper, Scissors, using Blawx’s [new include block](https://www.blawx.com/2019/09/blawx-updates-new-ui-and-include-for-code-re-use-video/#page-content). Then, we encode the translation using an experimental data connection block. The data connection block is not yet part of the public alpha.

The part of the encoding that takes a “game” in the docassemble data and converts it into the format that the encoded rules are expecting is shown here. This code is using some advanced, and if we’re being honest, kinda ugly-looking features of Blawx. In the future, we aim to make this process a lot simpler. But proofs of concept don’t get to be pretty.

![](/1__hoEDYCXWRjCPYkUuzhd4ug.png)

This file was saved as darps.blawx (for docassemble rock paper scissors).

### The Question: Who Won What Games?

Next, we added a question to the darps.blawx encoding. Specifically, the question was “who won what games?”

In Blawx, the encoded question looks like this:

![](/1__I55NEqK1aKU7g1InPCqwYg.png)

### The Interview: Tell Me About Your Rock, Paper, Scissors Games

We then created a docassemble interview that asks you about your games.

Here’s what one sequence of questions and answers might look like.

![](/1__NqlEMOnqtxMPt0dCZnb0Ag.png)
![](/1__8FUnuT0pXQAYqvNsrX__MRA.png)
![](/1__SI__Wj07eYaBGaNZtNjjLRg.png)
![](/1____aIPrzbT9pu5li7yCN27Yg.png)

The interview will allow you to give the details of as many different games as you want.

The interview then sends the data collected in the docassemble interview to the Blawx reasoner, along with the integration rules and the question. This is all done with one line of Python code inside the docassemble interview:

`answer = reasoner.ask('darps.blawx')`

The Blawx server uses the code, includes the rules from where they are published on the web, and uses the data from the interview to answer the question. The answer is sent back to the docassemble server, where it is used to display the results page:

![](/1__u8BQ9alXOPs9hpC6L__uEYA.png)

> Good ‘ol paper. Nothing beats paper.

> \- Every lawyer ever.

### That’s It?

Yeah, that’s it. But it’s not about the _what_, it’s about the _how_.

The same rules and the same interface can be used to answer multiple different questions. For example, the interview above is built for answering the question “who won what games.” But it would also be perfectly good for answering other questions, like “who won the most games”, or “did anyone win any games using Rock”.

What’s REALLY interesting about this proof-of-concept is not what it takes to build the first app. It’s _how little it takes_ to build the second one. And let’s imagine that we aren’t talking about Rock, Paper, Scissors, we’re talking about tax law. Here’s what you do:

1.  Import the encoding of the tax laws.

![](/1____x2OMGchZMAAdqqX58yVvQ.png)

2\. Import the docassemble interface.

![](/1__Q0ptf6ldbr7g2yDY5vjAHg.png)

3\. Encode the question.

![](/1__Jje9Q9o6EMVkDGslvXCM4w.png)

4\. Create a docassemble screen to display the answer.

That’s it.

### That Seems _Really_ Easy.

That’s what’s revolutionary. Blawx allows you to capitalize on your legal knowledge by codifying it. The codified rules can be used to cut development time for new automated legal service applications from days or weeks to _literally minutes_.

And imagine what happens when the wonderful people in the #RulesAsCode community get their way, and legislation and regulation and policy is published in or translated to Blawx as soon as it is released?

### Can I Try It?

Not yet, sorry! The proof of concept demo works in docassemble, but nowhere else, yet. We are going to fix that, and push the new data connection blocks to the public alpha version before we let people play with it. Thanks for your patience!