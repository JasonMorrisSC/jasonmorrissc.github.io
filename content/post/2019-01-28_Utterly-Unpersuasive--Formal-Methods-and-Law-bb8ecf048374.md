---
toc: true
title: 'Utterly Unpersuasive: Formal Methods and Law'
description: >-
  This is a story in which Hillel Wayne attempts to answer the question why
  don’t more programmers use formal methods?
date: '2019-01-28T19:43:08.088Z'
categories: []
keywords: []
slug: /@roundtablelaw/utterly-unpersuasive-formal-methods-and-law-bb8ecf048374
---

[This is a story](https://www.hillelwayne.com/post/why-dont-people-use-formal-methods/) in which Hillel Wayne attempts to answer the question why don’t more programmers use formal methods?

Formal methods are techniques that allow you to say with confidence that your software is right, instead of just trying it and waiting for a bug to show you where the software was wrong.

They can be applied to laws and contracts, too, but aren’t. But should be. That’s why I read the article, to see if there is something that I could learn about how to make formal methods more popular in law.

Then Hillel wrote this, talking about design specification (one of many formal methods):

> But there’s also a big mismatch in the value between fans and outsiders. To fans, the biggest benefit is that the act of writing a design forces you to understand what you’re writing. When you have to formally express what your system does, suddenly a lot of subtle errors become painfully obvious. This is **utterly unpersuasive to outsiders**.

This is so true it drives me nuts.

Let me try to tell you a story about how I went from being an outsider to being an insider.

I’m doing an LLM at the University of Alberta Faculty of Law in Computational Law. In one of my classes I had a project to encode the _Adult Interdependent Relationships Act_, SA 2002 c A-4.5 [AIRA], in a programming language called ErgoAI.

That process is almost perfectly analogous to the idea of doing a design specification for a piece of software. The law is the software. The encoding of the law is the design specification, done in a more formal language.

[_NB: This is not true of all methods of encoding laws. And most of the tools that are available for this are either too intimidating for lawyers to learn, or lack necessary features. That’s part of the reason I am working on_ [_www.Blawx.com_](https://www.blawx.com)_._]

The experience goes like this: You read a section of the law, and you try to figure out what the computer would need to know in order for you to be able to express that rule in the programming language.

For example, in order to encode the rule “no vehicles on the grass”, the computer needs to understand what a “vehicle” is, what “on” means in this context, and what “the grass” is. All of those things are understood by people automatically reading the natural language version (assuming it’s well-drafted), but the computer can’t understand any of them. You have to build up the terms the computer understands before you can utter the rules.

That process of taking what is implicit and making it explicit does not happen when you write the law itself, or when you read it in the abstract. When you look at the law in this way, you see it differently.

AIRA is Alberta’s codified version of common law marriage. A simplistic version of the rule is that you are in a common law relationship with someone if you live together and have children, or have lived together for at least a year. In all cases, you have to have lived together. Or so I thought.

The AIRA sets out a means by which people can enter into an agreement to be in a common law relationship, even one that is not conjugal.

As I was encoding the AIRA, one of the implicit ideas that I discovered was that there are multiple ways that a common law relationship can start, and multiple ways in which it can end. These two concepts, an initiating event, and a terminating event, are not explicitly stated in the law. But in order for a computer to be able to answer questions about whether two people are currently in a common law relationship, the computer needs those implicit ideas made explicit.

In the course of making all the initiating events explicit, I included two people entering into an agreement, prior to living together, but while they intend to live together. That is one of the options in the AIRA.

And then it happened. I realized that there was no terminating event for an agreement entered into while intending to live together, but the cohabitation _never starts_.

So because of this loophole it might be possible for two people who have never lived with one another to nevertheless be a common law couple.

Now if you are a lawyer in Alberta, familiar with the AIRA, and you _knew that already_, please tell me so. Because I have asked around. And none of my colleagues have had any idea that this loophole exists until I suggested it. I certainly had no inkling it was there until I tried to encode the Act.

Why? Are we all bad lawyers? Are we just failing to read carefully? No. It is because we are human beings, and laws are complex. It is what you would expect, and most lawyers consider this sort of loophole to be a sort of necessary evil (or good, depending on your perspective).

But is it really necessary? Had the people writing this law been using this technique, would this loophole exist? I expect not. Because it would have been made glaringly obvious, despite previously being totally invisible.

And this is not AI. I discovered this problem, not the computer. I discovered it because encoding the law forced me to think in a more disciplined way about what the law said. So even if the code was never actually compiled, or run, or used to answer any question whatsoever, the mere attempt required me to think better, and more clearly, about what the rules said.

There are lots of things that we can do with encoded laws. We can use formal methods that would allow us to generate random fact scenarios and look for weird legal results. Some people are looking at the ability to be able to prove certain things, mathematically, about high risk rules like smart contracts.

But look at what Hillel was saying:

> the act of writing a design forces you to understand what you’re writing

That is one of the most compelling arguments that these sorts of programming languages should be a part of legal education. Or at the very least, should be an aspirational goal of anyone who wants to write laws or contracts or rules that don’t suck.

Not because you want to automate legal reasoning. Though it will also allow you to do that. But because it forces you to understand better what you are reading and writing.

Learning to code with these sorts of tools forces you to learn how to think differently. It is a formal method of legal analysis. And it is demonstrably better than the “thinking like a lawyer” that we are taught now. I know, because I understand laws better when I use it. I see things that no one else sees.

And it is not taught, anywhere. Both because most lawyers and legal educators don’t know about it, and evidently because for “outsiders” the benefits may _still_ seem “utterly unpersuasive.”

That’s a shame.