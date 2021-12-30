---
toc: true
title: 'The Value of Rules as Code Without Computers'
description: >-
  I’ve been preparing a presentation for the Canadian Institute for the
  Administration of Justice’s Legislative Drafting Conference on Rules…
date: '2020-08-14T16:36:05.386Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/computational-law-diary-the-value-of-rules-as-code-without-computers-2a59c0766310
---

![](/1__STEUmNEjEfyAlQPB71u48w.jpeg)

I’ve been preparing a presentation for the [Canadian Institute for the Administration of Justice’s Legislative Drafting Conference](https://ciaj-icaj.ca/en/upcoming-programs/2020-legislative-drafting/) on Rules as Code. In the process of trying to figure out how to introduce the topic of Rules as Code to legislative drafters, I’ve been thinking about the different meanings the term “Rules as Code” has been given.

Some people use “Rules as Code” to refer to the larger public service delivery methodology, which includes a method of legislative drafting, where you write the legislation in natural languages and computer languages at the same time. Some people, I think more intuitively, use “Rules as Code” to refer to the product-a piece of legislation written in a programming language.

They aren’t the same thing, and the naming has been a source of some minor controversy among proponents. But what interests me is not what you call them. What interests me is what happens when you combine them, and what happens when you don’t.

You can do both at the same time, clearly. You can use the Rules as Code methodology, and through that process produce Rules as Code products.

And you can also produce Rules as Code products without using the drafting and public service delivery methodology. That type of “Rules as Code” is more or less just how legal application software would be developed now, _if_ developers compartmentalize out the legal reasoning. (I think if you don’t have the legal rules separate from the rest of the code, you’re talking about “Rules in the Code, there, somewhere”, which is different.)

But surprisingly, there are also benefits to the legislative drafting methodology by itself, even if you never actually deploy any code.

> If you want to understand something, teach it.

Why is this true? Because the process of attempting to communicate something you know to a person who does not already know it forces you to engage with the difference in contexts. It’s a similar idea to the “curse of expertise”, where the expert forgets what it is like to be a person who does not already understand.

Overcoming the curse of expertise requires you to put yourself in the shoes of a person who knows less, and rebuild your own understanding from that new starting point.

Similarly, when you are trying to understand a piece of legislation, a computer takes the role of the person who knows less. And the syntax and semantics of the programming language give you the entire context of what the computer already knows.

So attempting to put your legislation in code-even if you do it with pen and paper— forces you to understand it from a position of less context, and you learn it better. You catch mistakes you didn’t know you were making.

It is, in that sense, just an advanced technique of legislative drafting.

For me personally, this is not a mere hypothesis. I have consistently experienced this effect. Every time I sit down to encode a ruleset, I end up with a deeper understanding of how those rules work long before I have asked the computer to do anything with the code.

Most recently I have been encoding a set of rules regarding Covid-19 in Alberta. I read them a few times and saw nothing wrong with them. Then, I tried to put them in code, and realized that there is a scenario in which a person who is known not to have the disease can nevertheless be forced to continue to isolate.

Professor Scott Brewer at Harvard Law has a similar argument, which relies not on programming languages but on formal logic. He asserts that lawyers should learn to analyze, for example, appellate court decisions by making formal logic representations of the arguments made by the court, and analysing them using the methods of formal logic. Doing so reveals things that a less disciplined analysis doesn’t.

The programming languages with which I am most enthusiastic about for use in Rules as Code are built on a foundation of formal logic. Perhaps at some point something along the lines of [Blawx](https://www.blawx.com) will be used in law schools to teach legal analysis.

Wouldn’t that be something?

Speaking of Blawx, [my paper on Rules as Code and Blawx](https://law.mit.edu/pub/blawxrulesascodedemonstration/release/1) _just_ went live as a part of [release 1.2 of MIT’s Computational Law Report](https://law.mit.edu). I’d love your feedback.

### Computational Law Job Opportunity

*   I just heard about [a wonderful opportunity](https://www.uu.nl/en/organisation/working-at-utrecht-university/jobs/phd-position-in-hybrid-intelligence-explaining-data-driven-decisions-with-legal-ethical-or-social) for a 4-year PhD with Dr. Prakken at Ultrecht University, as part of a project to generate explanations for the conclusions of “black box” artificial intelligence algorithms. Dr. Prakken is a leading expert on modelling legal arguments, and an excellent teacher.

### Coming Up…

*   A piece I wrote for [Slaw.ca](https://www.slaw.ca/) about the SMU Centre for Computational Law will be coming out in late August.
*   I will also be participating in a Rules as Code panel on the first day (September 10) of the 2020 Legislative Drafting conference held (virtually) by the Canadian Institute for the Administration of Justice. Check [their website](https://ciaj-icaj.ca/en/upcoming-programs/2020-legislative-drafting/) for registration details. There are sponsored tickets available for students, I understand.

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._