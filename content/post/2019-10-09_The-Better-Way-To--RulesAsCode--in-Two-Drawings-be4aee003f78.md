---
toc: true
title: 'The Better Way To #RulesAsCode, in Two Drawings'
description: 'Here’s how we generally build tools that automate legal reasoning now:'
date: '2019-10-09T16:24:36.192Z'
categories: []
keywords: []
slug: /@roundtablelaw/the-better-way-to-rulesascode-in-two-drawings-be4aee003f78
---

Here’s how we generally build tools that automate legal reasoning now:

![](/1__tJ__he4VFfqd1iHSu12zHWw.jpeg)

So we have some rules, and we have a question we would like to answer. A programmer comes up with a procedure for answering that question given the rules, and encodes that procedure in an algorithm. The algorithm and the data (collected from the user somehow) are given to the computer, and the computer follows the steps in the algorithm and spits out an answer.

Note that a “question” here is analogous to an application, or a feature of an application. The more apps you have, or the more complicated those apps, the more questions are being asked and answered.

Note also that a single set of rules can help you answer any number of different questions. For example, the rules of the game of checkers could be used to answer questions like “who is winning?”, “whose turn is it?”, or “is it possible for black to capture more than one red piece?”

So what’s sub-optimal about the way we are doing this now?

1.  Every question needs its own algorithm. So if you want to answer a new question, you need a programmer who understands the rules to write a new algorithm.
2.  If the rules change, all of the algorithms might need to be updated. Which requires a programmer who understands the old algorithms, and the old rules, and the new rules.

That’s a lot of work for people who know the rules well enough to encode them. Those people are expensive.

Here’s what the picture looks like if you use declarative logic programming tools:

![](/1__0FcIAp9y53CSzuOm5Z90nw.jpeg)

So here, a programmer who knows the rules creates an encoded version of the rules (the #RulesAsCode). Another programmer encodes the question. This second programmer doesn’t need to know what the rules are, or how to encode them. Encoding a question is much easier. A piece of software called a “reasoner” uses the encoded rules and the encoded question to automatically generate a procedure for answering the question. Then, the computer uses the procedure and the user-supplied data to generate an answer.

Why is this better?

1.  The programmer who knows the rules only needs to encode the rules once, and any number of questions can be answered using that one encoding.
2.  The programmer who encodes the question doesn’t need to know what the rules are, just how to ask questions about them.
3.  If the rules change, only one thing needs to be updated (the encoded rules), and all of the algorithms will be updated accordingly.

So building new or maintaining existing apps or features is way easier, and way less expensive.

When people talk about Rules as Code (check out the #RulesAsCode hashtag on twitter), they are often talking about the first way. I’m focused on trying to make the second way easier.