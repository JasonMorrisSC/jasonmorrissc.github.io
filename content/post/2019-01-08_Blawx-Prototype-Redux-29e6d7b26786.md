---
toc: true
title: Blawx Prototype Redux
description: >-
  I posted here earlier about a prototype tool that I’m working on to make it
  easier for lawyers to use declarative programming tools for…
date: '2019-01-08T00:59:18.436Z'
categories: []
keywords: [blawx]
slug: /@roundtablelaw/blawx-prototype-redux-29e6d7b26786
---

I posted here earlier about a prototype tool that I’m working on to make it easier for lawyers to use declarative programming tools for automating legal reasoning. It’s called “Blawx.”

I spent some more time working on it over the weekend, and made some progress, as you can see in the (now narrated) video linked below.

I have implemented code generation based on the block diagram, implemented the reasoner online with a CGI wrapper so that it can be accessed by the Blawx page, updated the interface to have an answer window at the bottom, and have changed the way that answers are received so that it is possible to get back the answers to questions that have more than one solution.

I need to code the system for automatically generating block definitions based on the categories, attributes, and objects that have already been defined. Once that is done, it should be possible to ask and get answers to arbitrarily-complicated questions, and the prototype would be useful for educational purposes, so I will probably stop feature development at that point and deploy it so people can start playing with it.

Where it goes from there will be up to everybody else.

What would you be more interested in seeing:

a) the ability to import libraries of code representing an area of law, and then state facts and ask questions about that area of law?

b) the ability to state a “goal” statement, and have the reasoner tell you what additional facts might help you prove that statement was true?

c) the ability to save a query, and then have it become a reasoning tool that can be used by other web-based tools over API?

d) the ability to automatically generate test data in order to do searches for the effects of changing rules?