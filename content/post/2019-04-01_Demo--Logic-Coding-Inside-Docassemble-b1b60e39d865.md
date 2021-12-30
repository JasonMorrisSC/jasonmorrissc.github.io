---
toc: true
title: 'Demo: Logic Coding Inside Docassemble'
description: >-
  Logic coding is awesome for automating legal services, and more lawyers should
  be doing it.-Me.
date: '2019-04-01T18:13:19.051Z'
categories: []
keywords: []
slug: /@roundtablelaw/demo-logic-coding-inside-docassemble-b1b60e39d865
---

> Logic coding is awesome for automating legal services, and more lawyers should be doing it.-Me.

I’m on a mission to prove that. I’ve got a list of obstacles in front of me that look distinctly like opportunities.

Take for example, the opportunity “programming is hard.” I am working on [www.Blawx.com,](http://www.Blawx.com,) which aims to make learning it way easier. Check out [this short demo video](https://www.blawx.com/2019/01/socrates-is-a-man-example/) for more details.

Another opportunity is that people don’t know how to get value out of logic programming. But it’s way more effective to show than to tell.

And today, I have something to show.

#### Stacking It Up

Hamish Fraser was recently talking on Twitter about the need for an open source “stack” for automating legal services.

That’s a bit jargony. Here’s the intuition:

> If there’s only one law, and lots of different people need to get computers to do things that depend on what that law says, then it seems silly to have each of those people write code for that law each time. Why don’t we just have one place where we can find the code for a law, and let everyone else use it?

The word “stack” is an analogy to the idea of building blocks. You take a bunch of different tools that are able to solve a part of a problem, and you stack them on top of each other to create something that’s able to solve a bigger problem.

Part of being able to stack technology is the interfaces between the tools. To extend the analogy a bit, if you want to add another block to the stack, the top of the stack needs to be the same “shape” as the bottom of the next block. Otherwise, they won’t “fit,” and your tower will fall over.

So part of the challenge of building an open-source stack for legal automation is to build the interfaces between the tools you already have, so they can work together. It’s worth noting, commercial “stacks” do exist for this sort of thing. But the licensing fees are a barrier to adoption that we need to get rid of.

#### The Building Blocks

Ergo Lite is a free, open-source declarative logic programming language, with features that make it really useful for encoding things like legislation and regulations. [Coherent Knowledge](https://www.coherentknowledge.com) sells a commercial version called ErgoAI.

The Ergo Lite language and reasoner is the logic coding block in my open-source stack for legal automation.

[Docassemble](https://docassemble.org) is a free, open-source web-based interview tool, designed by and for lawyers to automate legal interviews and automated document assembly.

Docassemble is the “build a web interview that gives the client something useful” block in my stack.

#### What I Added to the Pile

This week, I spent some time building the interface between Ergo Lite and Docassemble. Shout out to Markus Schatten at the Artificial Intelligence Laboratory at the University of Zagreb, who last week released an open-source tool called [pyxf](https://github.com/AILab-FOI/pyxf), which solved most of this problem for me.

![](/1__l4H39cxZHjMDSZVDGM__2rA.png)

The interface I built has three main parts:

1.  An easy interface for the Docassembe interview designer to access Ergo Lite capabilities.
2.  The interface letting Docassemble and Ergo Lite talk to each other in the background on the web server.
3.  A translator that takes the facts collected by the Docassemble interview and puts them into the Ergo Lite language, so the reasoner already knows what Docassemble knows.

#### Try It!

If you’d like to see what it looks like, [check out this live demo](https://testda.roundtablelaw.ca/interview?i=docassemble.playground3%3Adeclarative_demo.yml).

![](/1__nTeLcc4tEJnk84rUKFDx__w.png)

#### Where Does This Get Us?

Now, I have a way of demonstrating how you get from writing logic code to giving someone something they will pay you for.

I can then compare what I can build with logic programming, and how efficiently, to what everyone else can build, and how efficiently, without it. Show, not tell.

And, when trying to convince people to try something like [www.Blawx.com,](http://www.Blawx.com,) we can now point to this stack and say “here’s what you can build with it.”

Also, when I’m doing docassemble development work in my own law firm, of for my consulting clients, I can bring the power of declarative logic coding to bear where it will make things better, faster, and cheaper.

#### Need More Info?

I’m available on [Twitter at @RoundTableLaw](https://www.twitter.com/RoundTableLaw), and I’m always happy to chat. If you’re interested in building something and want some help, please contact me at [Lemma Legal Consulting](https://www.lemmalegal.com), we’d be happy to help you build something awesome.