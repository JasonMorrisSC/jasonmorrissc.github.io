---
toc: true
title: 'Live Demo: Integrating Docassemble and Ergo Lite for Logic Coding'
description: >-
  I posted just over a month ago about having figured out how to get Docassemble
  and Ergo Lite to play nice with one another, so that it was…
date: '2019-05-10T23:44:37.829Z'
categories: []
keywords: [flora2]
slug: >-
  /@roundtablelaw/live-demo-integrating-docassemble-and-ergo-lite-for-logic-coding-f35e9f25f311
---

I posted [just over a month ago](https://medium.com/@jason_90344/demo-logic-coding-inside-docassemble-b1b60e39d865) about having figured out how to get Docassemble and Ergo Lite to play nice with one another, so that it was possible to use logic coding inside a Docassemble interview.

Then, a couple of weeks ago I posted [a series about using Ergo Lite to encode LSAT puzzle questions](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

So today I’d like to show of a combination of the two.

#### Encoding LSAT Puzzles for Fun and Profit

The preamble of the LSAT puzzle questions that I encoded last week reads like this:

> Exactly three films-Greed, Harvest, and Limelight-are shown during a film club’s festival held on Thursday, Friday, and Saturday. Each film is shown at least once during the festival but never more than once on a given day. On each day at least one film is shown. Films are shown one at a time. The following conditions apply:

> * On Thursday Harvest is shown, and no film is shown after it on that day.

> * On Friday either Greed or Limelight, but not both, is shown, and no film is shown after it on that day.

> * On Saturday either Greed or Harvest, but not both, is shown, and no film is shown after it on that day.

Those rules have been encoded in Ergo Lite, and the code added to the docassemble server.

![](/1__dMzw7nNNFXnZ5mCi0UFejg.png)

I then created a simple docassemble interview that asks you to set out a three-day schedule for a festival, and tells you whether or not your schedule follows those rules.

![](/1__Bl66uYr2YAHqP__ixcPUAPg.png)

I needed to add some rules to translate facts between Docassemble and Ergo Lite. After that, adding the reasoner to the docassemble interview required only a couple of lines of code.

#### So What?

I’m making steady progress toward a stack of technology that duplicates the features of expert system tools like Oracle Policy Automation and Neota Logic, using exclusively open source tools. That’s going to make advanced legal expert system development a more realistic alternative for organizations serving people who fall into the access to justice gap.

#### What’s Next?

I want to demonstrate that once you have encoded rules in Ergo Lite, you can use them to do more than one thing. I also want to demonstrate that writing code in Ergo Lite doesn’t need to be as intimidating as it looks above.

That’s why I’m also currently working on expanding the features of [Blawx](https://www.blawx.com). Blawx is a graphical interface for declarative logic coding. I’ll be doing a brief demo of that work on May 13, 2019 at the Rules as Code Show and Tell.

The demo uses the same LSAT code from the series of posts last month, but shows how you can use Blawx to [encode all 5 answers for question 6](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-6-884c47d55b2b), and ask Blawx which answer is correct.

![](/0__jRh1myyugSZuWOyG.jpg)

#### Can I Try It?

Click [here to check out a live demo](https://testda.roundtablelaw.ca/interview?i=docassemble.playground3%3Alsat_demo.yml).

#### Need More Info?

I’m available on [Twitter at @RoundTableLaw](https://www.twitter.com/RoundTableLaw), and I’m always happy to chat. If you’re interested in building the next killer legal app, contact me at [Lemma Legal Consulting](https://www.lemmalegal.com), we’d be happy to help you build something awesome.