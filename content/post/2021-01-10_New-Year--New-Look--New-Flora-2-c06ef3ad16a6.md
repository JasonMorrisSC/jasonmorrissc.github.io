---
toc: true
title: 'New Year, New Look, New Flora-2'
description: >-
  I thought that given it is a new year, it was time to refresh the way the
  Diary looks. So the Diary has become a publication on Medium…
date: '2021-01-10T13:45:57.974Z'
categories: []
keywords: [flora2,meta]
slug: /@roundtablelaw/new-year-new-look-new-flora-2-c06ef3ad16a6
---

![](/1__e1FLuKugeHTBwJJO__4L31w.jpeg)

I thought that given it is a new year, it was time to refresh the way the Diary looks. So [the Diary has become a publication on Medium](/post/roundtablelaw), which you can follow without needing to follow me personally, and to which I can add other members of the [SMU Centre for Computational Law](https://cclaw.smu.edu.sg/) team.

I would love it if you would share [the link to the diary](/post/roundtablelaw) with your friends networks interested in Rules as Code, Smart Contracts, Legal Tech, and Computational Law.

### Update: Singapore Arrival

I am now physically located in Singapore, waiting out the mandatory two-week isolation in a lovely hotel room. Travel and jet lag has been challenging. I’m shopping for accommodations, and doing the other things involved in setting up ones life as an ex-pat. If anyone has recommendations for Singaporean banks, and food delivery apps, let me know.

### New Version of Flora-2

Last week Coherent Knowledge released a new version of Flora-2, which I’m hoping will solve at least two problems.

#### Explanations in Flora-2

You may remember that I have been working on some code to add explanations to the answers that Flora-2 gives, using a meta-programming approach. Another way of approaching the same problem is to have the language’s reasoner record what it is doing as it is answering the questions, and then automatically parse the resulting log files. I started in on that approach a few months ago only to learn that there were bugs in how the log files were being generated. I’m hoping that the new version of Flora-2 has solved that problem, breathing new life into that project.

#### Date Subtraction in Flora-2

Flora-2 is maintained and developed by Coherent Knowledge, who also sell a commercial version of the language called ErgoAI. But SMU CCLaw is focused on open source technologies, and ErgoAI isn’t one. Flora-2 is like the “free tier” for ErgoAI, and as a result, it lacks a few features that ErgoAI has, like the ability to generate natural language explanations.

Another feature intentionally withheld from Flora-2 by the developers, as a means of promoting adoption of their commercial offering, is the ability to measure the amount of time between two dates. This has been a source of frustration.

Consider a smart contract that requires that something be delivered within 6 days of the date on which it is ordered. If you have a record of the delivery, and a record of the order, and you want to know whether or not that requirement has been complied with, you need to subtract the dates of those two records from each other and determine if the difference is 6 days or less.

It’s a VERY common problem.

This new version of Flora-2 doesn’t add that functionality, but it does add some features that make it easier for you to add it yourself. I’m making good progress on that front, and should have a working version that I can publish later this week. Stay tuned on that front.

### On the Agenda: SAFE in Flora-2

It seems like forever ago that I started trying to encode the SAFE in Flora-2. I quickly realized that I needed a more sophisticated way of dealing with time, and that I needed to be able to talk about events and their consequences. So after a detour into interval calculus, timepoint calculus, event calculus, and constraint logic programming, I’m back on track.

Once I have a representation of SAFE in Flora-2 that does the sorts of things I want it to be able to do, then I can use that code, and the answers it is able to generate, as a test-bed for generating explanations using Flora-2, and perhaps tools like Carneades.

### Also Coming: L4 and IDE Prototypes

The DSL team is working on a top-to-bottom implementation of a very simplified version of L4 that focusses on description logic for setting out ontologies. At first, it will not be able to do much, except perhaps tell you whether you have violated any of the rules in your own ontology, like “a person can have two biological parents.” Having that simple base to work out from is going to give the team a lot more certainty that the changes they make to the design as they go doesn’t break anything that is already working.

The IDE team is also ramping up work on building a language module for VS Code for L4, so I’m hoping to start doing demos sooner rather than later.