---
toc: true
title: Playing Along With Rules as Code Part 2
description: >-
  Scott McNaughton’s latest blog post about the Government of Canada’s Rules as
  Code project indicates that they have decided to change the…
date: '2020-02-24T21:21:02.322Z'
categories: []
keywords: [pawrac]
slug: /@roundtablelaw/playing-along-with-rules-as-code-part-2-4acc82c53f95
---

[Scott McNaughton’s latest blog post](https://medium.com/@mcnaughton.sa/week-50-reflections-on-rules-as-code-5878ff42d43c) about the Government of Canada’s Rules as Code project indicates that they have decided to change the scope of the project a little bit.

The first thing is that they have decided they are worried primarily about being able to answer questions about how much vacation pay is due, and not questions about entitlement to vacation periods.

The second thing he mentions is that they want to be able to take into account what is called “protected leave.” When calculating how much vacation pay a person is owed, how long the have been working with you matters. When they have been working for you for a period of time, but have also had leave during that period of time, some of that leave may count as how long they have been working with you, and some of it may not.

For example, a certain amount of medical leave counts as employment but after that it doesn’t anymore.

### A critique

I have a critique to offer. I’m a little concerned that there is a risk of getting away from doing a Rules as Code experiment, and getting into a programming experiment.

What’s the difference? Rules as Code has an extra layer compared to a basic programming task.

With rules as code, you start with a law, you create a digital representation of that law, and then computers can use that digital representation for many of the things that people could use the law for. The digital representation of the law is the middle layer. The advantage of using it as a middle layer, instead of writing code to answer a specific question, is that you can re-use it to answer multiple questions, and you can change it when the law changes and apply those changes to all the applications that use the representation.

If you take out that middle layer, and just write software that answers a specific question, then what you’re doing isn’t Rules as Code, anymore, it’s just… programming.

So I’m a little worried about defining a Rules as Code experiment in terms of the single question it is supposed to answer, or the sections of law that relate to answering that question. Because it is possible to succeed at writing software to answer a specific question, and never really have represented the meaning of the rules in a way that could be reused.

The “big idea” in Rules as Code is co-drafting. Co-drafting is the idea that legislation should be written in a natural language and in code at the same time so as to both make it easier to implement, and to end up with better laws. And it’s rewarding to see that this benefit is starting to make itself clear to the people in the Government of Canada involved in the project. Scott reports that one of the drafters involved told him as much specifically.

But imagine you are drafting a law, and you are trying to do co-drafting. You are trying to draft the law in a natural language and in code at the same time. If the point is to create a digital representation of the law that can be used for as much as possible, you don’t have any questions to start from. You can’t know in advance which questions are going to be the important questions. And the representation is supposed to be able to address as many of them as possible.

So from my perspective, hard-core Rules as Code should be agnostic as to the questions, and should start from the meaning of specific sections. Adding or removing sections on the basis of the question you’re trying to answer, and defining success as being able to answer only that question risk missing the point.

Now, in the real world, what is important in the short term is demonstrating the practical benefits of Rules as Code, not demonstrating the greatest possible practical benefits of Rules as Code. We need to take baby steps. So this is not to say that the Government project is doing anything wrong. But it might be a good idea to look for opportunities to use the representation you generate to answer more than one question, as secondary objective.

### Moving Forward — Changing Tools

The point here is to play along. So I’m going to follow along with the idea that we’re not worried about vacation time, we’re only worried about vacation pay. That’s a simple task of reducing the scope.

But protected leave is more complicated. The best way of encoding the answers to questions like “at this point in time for how long had this fact been true?” is using something called temporal variables.

A normal variable has a value. A normal (non-temporal) boolean variable, for example, can only have a value of True or False. A temporal variable, however, has values that change over time. So a temporal boolean variable is True or False as of a given time, and the value can change back and forth.

Temporal variables are very useful when automating legal rules that deal with the passage of time because you can logically combine temporal values to create new ones by saying very clearly understandable things like:
```
the person is employed for the purpose of calculating vacation pay if  
   the person is employed  
   and  
   the person is not on unprotected leave
```
In that statement “person” is an object, it is has two temporal boolean variables, “on protected leave”, and “employed”. The “on protected leave” can be reversed to create “not on protected leave”, and then that and “employed” are used to create “employed for the purpose of calculating vacation pay”.

Once you have this new temporal boolean variable, it becomes very easy to ask question like “how long has the employed for the purpose of calculating vacation pay” variable been set to “True” prior to and including today?

The problem is that Flora-2, the programming language I started playing along with, does not support temporal values (as far as I know). Because it is open source, in theory I could write a library to make it support temporal values, but that’s a relatively large project that I don’t have time for.

So, I can either leave out protected leave, and continue using Flora-2, or I can include protected leave, use Flora-2 and implement it in a much more complicated way, or include it and use a tool for temporal values.

The only tool I’m aware of that supports temporal values out-of-the-box is Oracle Intelligent Advisor (until last year known as Oracle Policy Automation).

There are a couple of reasons using OIA is less than ideal for a Rules as Code project. First, it’s proprietary software. It is free to use for learning purposes, and being proprietary doesn’t prevent it from being used inside government. But a proprietary format does limit the degree to which the encodings could be shared with the public, which is one of the long-term objectives of Rules as Code.

Second, OIA doesn’t include defeasibility, so you end up with all the problems of encoding the rules in a tool that doesn’t allow you to specify exceptions.

But there are upsides to using OIA, too. First, it allows you to go quickly from an encoding of the rules, and the posing of a question, to generating a web interface that will allow you to generate answers to those questions. Second, it has interesting visual representations of temporal values as value graphs in the design tools. So you could create a complicated fact scenario, and then show how the value of the person’s entitlement to vacation pay had changed over time. Third, OIA has explainability built-in, so the web tool you generate will give you an answer, and also give you a detailed explanation for why that answer is correct.

All things told, I think we will have more fun if we implement in OIA as opposed to Flora-2, so despite my strong preference for open source tools, for this project I’m going to hold my nose and switch.

I’m also going to continue to advocate for people, including the Government of Canada, to put real resources toward the development of open source Rules as Code technologies that have all the things we need: defeasibility, explainability, temporal reasoning, links to source materials, interactive testing between drafters and clients, publishing rulesets as question-answering tools over API, ease of use for non-programmers, ease of integration for developers, and more.

So stay tuned. Next post, I’m starting over in a new piece of software.