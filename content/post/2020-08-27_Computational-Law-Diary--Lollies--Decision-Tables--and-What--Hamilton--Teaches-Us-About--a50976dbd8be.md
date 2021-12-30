---
toc: true
title: >-
  Lollies, Decision Tables, and What “Hamilton” Teaches
  Us About…
description: Search Space Episode 3
date: '2020-08-27T22:25:24.304Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/computational-law-diary-lollies-decision-tables-and-what-hamilton-teaches-us-about-a50976dbd8be
---

### Search Space Episode 3

This past weekend I was listening to [an episode of the Search Space](https://thesearch.space/episodes/3-chris-martens-on-narrative-generation), a wonderful podcast for just my particular breed of nerd, and Professor Chris Martens was talking about their language [Ceptre](https://github.com/chrisamaphone/interactive-lp), which is used for programming interactive worlds.

I’m now excited to learn more about linear logic and the “lolly” connective `A -o B` which in Ceptre can be used to say “if A is true, then there can be a transition in state where A is no longer true and B becomes true.” Professor Martens uses Ceptre in [their PhD thesis](https://www.cs.cmu.edu/~cmartens/thesis/) to demonstrate how you can have a computer generate narratives interactively with a user.

That, to me, seems to have application in the legal expert system space, where what you want to do may be to give a user a plan that they can follow in order to obtain a desired outcome. Explaining how that plan works requires an accurate representation of how the context will change over the course of executing the plan. And may require adjustment if the context changes midway.

Dealing with legal processes-sequences of events over which some legal fact is changed-is a big part of what lawyers do. Being able to model the transitions between being a person who is married, to a person who is married and has filed for divorce, to a divorced person, is important.

And understanding where the right balance is between representing stable rules and representing states and transitions between them is an important part of our language design. This episode has helped me understand that challenge better.

All of the episodes of Search Space have been great, so far. If you are interested in declarative logic programming you should check it out and [support the host and producer Felix Holmgren’s efforts](https://thesearch.space/).

And Professor Martens talks about encoding the rules to the Pandemic board game, which basically makes them a kindred spirit, in my books.

### DMN Decision Tables

I spent some time playing with DMN decision tables last week and have come to the conclusions that while they are useful, I don’t think they are a first choice in formalisms for legal rules. They are a very efficient way of letting human beings write, read and use (which is unusual) non-looping procedures. Non-looping procedures are not all you are usually going to need, so a lot of the time they only solve part of the problem. But more importantly, because they represent procedures, instead of rules, you lose isomporphic representation of rules in natural language, which is a big loss.

### Hamilton’s “10 Commandments”

My eldest shared something with me yesterday that hits precisely on this distinction between rules and procedures. In the musical Hamilton, there is a piece that is called “The 10 Commandments of Duels.” My eldest said “That doesn’t actually make sense. The 10 Commandments were rules. The 10 things in the song are not rules, they are steps.”

For computational law, that’s a critical distinction. Writing procedures-based on rules-for figuring out legal conclusions is a very different task than writing rules and letting the computer build the processes for answering questions about them.

DMN decision tables, like the Hamilton song, are a very cool way of expressing a set of steps to follow. But they are not a way of expressing rules, or commandments. There are lots of cool programming languages out there that are designed to allow you to set out a procedure that follows rules. But the ones that are really useful for computational law are the ones that allow you to set out rules, and figure out the procedures themselves.

I got to watch some of our software developers working on building our language L4 for that purpose, and I have to say that I’m excited at what we’re going to be able to show in the next few months.

### Quick Notes

#### New Slaw.ca Article on SMU

An [article I wrote for Slaw.ca about my work with SMU Centre for Computational Law](http://www.slaw.ca/2020/08/25/digitizing-law/) is now live. Check it out.

#### Blawx Demo at Rules as Code Session of Legislative Drafting Conference

Preparations for the 2020 Legislative Drafting conference held (virtually) by the Canadian Institute for the Administration of Justice (see [their website](https://ciaj-icaj.ca/en/upcoming-programs/2020-legislative-drafting/) for registration details) are now in high gear. I will be presenting on Rules as Code and demonstrating [Blawx](https://www.blawx.com). Join us if you can.

#### Blawx Demo at MIT Computational Law Report

Please check out my [interactive demonstration of Rules as Code and Blawx](https://law.mit.edu/) in the MIT Computational Law Report. Your feedback would be greatly appreciated.

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._