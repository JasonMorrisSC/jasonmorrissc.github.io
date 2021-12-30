---
toc: true
title: 'Monads, mo’ problems.'
description: >-
  This week at work I got a wonderful lesson on Monads, which are a technique in
  functional programming. But what it taught me, more than…
date: '2020-07-30T05:37:02.246Z'
categories: []
keywords: []
slug: /@roundtablelaw/computational-law-diary-monads-mo-problems-7978f952b2fb
---

![](/1__XdmfXa2AogI719K0diQQ0A.jpeg)

This week at work I got a wonderful lesson on Monads, which are a technique in functional programming. But what it taught me, more than what Monads are and why I should care, is how bad most people are at explaining computing science concepts. And how they could take a lesson from infomercials.

#### Monad Nomad

I’ve watched multiple videos online, and read multiple chapters of tutorials trying to explain them, and come away baffled each time. And the reason is simple. No one starts with the problem(s) that Monads solve. They start with what they are, then how they are built, and then how to use them, and then say “good luck to you.”

Well, I’m sorry, I can memorize those things, but it doesn’t mean I understand anything. In order to understand Monads, I need to understand the problem that they solve, and the problem that they solve for functional programmers is not obvious-at all-for non-functional programmers. You can’t just abstractly describe the problem. You need to show me an example. Help me feel the pain.

#### Functional What Now?

Oh, right. Let me back up. The team at SMU Centre for Computational Law is building a domain specific programming language for law. Haskell is a strongly-typed functional programming language that is particularly well-suited to the task of building interpreters and compilers for programming languages. Several of the team members are experienced Haskell coders, and regularly speak to each other in Haskell.

I’m not one of the people tasked with writing compilers (thank goodness), but I have been intrigued by Haskell for several years, and have played with it occasionally. Understanding it better would be a nice-to-have for my work, so one day this week I asked one of my colleagues to explain Monads to me. And through a Socratic dialogue over Slack, I have been enlightened.

#### Right there in Black and White

Infomercial makers know this problem, and how to solve it. If they are trying to sell you a kitchen gadget for slicing dragon fruit, first they show you-in black and white-a otherwise mentally healthy-looking individual nearly impaling themselves with a knife trying to slice a dragon fruit. Then, they show you someone using the dragon fruit slicer, and enjoying insanely delicious dragon fruit slices with their grateful and highly attractive family members. Then they tell you how the device is made, and what its features are. But wait, there’s more.

#### The Following is Paid Advertisement for Monads

So what’s the problem that Monads solve?* Well, there are types that have other types inside them. Like a list of integers. And sometimes you want to do something to it, like double the values inside it.

So in type-safe pure functional programming, you can’t use the same function to double a list of integers as a single integer. You would need to write two different functions for that, because they act on two different types (integers, and lists of integers). And in a strongly-typed language, that doesn’t work.

The black and white video in the Monad commercial is of a person writing a tonne of “double” functions every time they want to double the number inside a new kind of container. A number that is maybe an error message? New function. A number that is being logged when it changes? New function. Etc. Then they sigh, throw their arms in the air, and dejectedly wipe the dragon fruit juice from their eye.

#### Introducing: Monads

What if instead of that, you had a category of types, we could call them monads, that knew how to apply a function to their insides. Then your one `double` function, if it worked on monads, would work on _any_ monad. You wouldn’t even need to know in advance what type of monad it was. It just needs to know how to take a function and apply it to its insides.

Boom. Your friend writing double functions is happy. Next you can go ahead and tell me about how monads allow you to maintain purity while also obtaining side-effect-like behaviour. And then you can tell me that they are a combination of a constructor, bind function, and return function. And then you can ask me to re-implement the Maybe monad myself as a learning exercise. In _that_ order.

*I’m still a beginner, and I probably got something important terribly wrong in the technical explanation above.

#### It’s the Problem, Stupid

The point of the post is not to teach you Haskell. The point is the _value of problems in communicating complicated information_. That’s an important lesson for my work, because communicating complicated things to an unfamiliar audience is a major part of my job description. And I’m suddenly very easily able to see all of the places I could have started that way, and didn’t, and should have.

Show me a person who has the problem, then show me that person using the solution and no longer having the problem. If I can understand and relate to the problem, I have a motivation and a context to which I can attach all the details. Like how its parts are washable, and it stays sharp in the freezer, and it will make me more attractive to whatever gender I want to be attractive to.

Just try to be more honest than the infomercials are.

#### Coming Up…

*   It sounds like a paper of mine for [MIT’s Computational Law Report](https://law.mit.edu) about Rules as Code and [Blawx](https://www.blawx.com) is dropping soon. I am _really_ excited for the opportunity to introduce a lot of people to Rules as Code as a concept, and to Blawx as a tool. Can’t wait to get some feedback and start some more conversations.
*   A piece I wrote for [Slaw.ca](https://www.slaw.ca) about the SMU Centre for Computational Law will be coming out in late August.
*   I will also be participating in a Rules as Code panel on the first day (September 10) of the 2020 Legislative Drafting conference held (virtually) by the Canadian Institute for the Administration of Justice. Check [their website](https://ciaj-icaj.ca/en/upcoming-programs/2020-legislative-drafting/) for registration details. There are sponsored tickets available for students, I understand.
*   My “Coding the Law” class at University of Alberta Faculty of Law starts up in September, and the Faculty’s first ever Digital Law cohort is getting underway at the same time.

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._