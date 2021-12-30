---
toc: true
title: 'A Computer Takes the LSAT: Introduction'
description: >-
  In this series of posts I’m going to show you what it looks like when you use
  a programming language called Ergo Lite to get a computer to…
date: '2019-04-25T21:51:33.452Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-introduction-3a65fd8b982
---

In this series of posts I’m going to show you what it looks like when you use a programming language called Ergo Lite to get a computer to answer puzzle questions from an LSAT exam.

The LSAT is the standardized admissions exam that most lawyers in North America will have suffered through, so I’m hoping it is a sort of shared experience that gives us lawyers a shared starting point.

Of course, the LSAT questions stand as an analogy for encoding other sorts of legal reasoning about rules. If we can get a computer to answer LSAT questions, we can get it to do other useful things with laws, and contracts, and regulations, and the like.

I’m going to explain what I’m writing in code, as I write it. But this is not a logic programming tutorial, and you should not expect to know how to write code in Ergo Lite when you are done reading it. It’s expected that most of the technical details will go over your head.

That’s OK. Keep reading.

The point is merely to take the mystery out of what encoding legal reasoning looks like, and make it more obvious to lawyers, in particular, what the potential applications are.

The series will be broken up into a number of posts:

*   Introduction: You are reading it now.
*   [Preamble](https://medium.com/@jason_90344/a-computer-takes-the-lsat-the-preamble-8b25c994ef7c): I will encode the rules that all 5 questions will use.
*   [Test Data](https://medium.com/@jason_90344/a-computer-takes-the-lsat-generating-fact-scenarios-3a52fd6fe908): I will generate all possible fact scenarios in code, so the computer can find answers by searching through all possibilities.
*   Question [6](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-6-884c47d55b2b), [7](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-7-aa98d14d3217), [8](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-8-bf39194443da), [9](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-9-f33d7200f48f), [10](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-10-2172137fdc57): One post each for each of the five questions.
*   [Conclusion](https://medium.com/@jason_90344/a-computer-takes-the-lsat-conclusion-86dc7467b14d): Wrapping it all up.
*   [Resources](https://medium.com/@jason_90344/a-computer-takes-the-lsat-resources-77e5d07d7e3a): Links to all of the source materials, software, and the raw code used in this series.

Ready? Let’s begin with [encoding the Preamble](https://medium.com/@jason_90344/a-computer-takes-the-lsat-the-preamble-8b25c994ef7c).

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.