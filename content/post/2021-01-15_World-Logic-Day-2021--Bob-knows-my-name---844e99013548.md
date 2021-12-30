---
toc: true
title: World Logic Day 2021 (Bob knows my name!)
description: >-
  January 14, 2021 was “World Logic Day”, and in celebration of it the
  Department of Computing Science at the University of Texas Dallas…
date: '2021-01-15T11:31:34.173Z'
categories: []
keywords: [blawx]
slug: /@roundtablelaw/world-logic-day-2021-bob-knows-my-name-844e99013548
---

January 14, 2021 was “[World Logic Day](https://wld.cipsh.international/)”, and in celebration of it the Department of Computing Science at the University of Texas Dallas [invited Robert Kowalski to give a speech on Logical English](https://www.meetup.com/utdcsor/events/275401890/).

![](/1__UCdUwRSOASsRoCzymsCVUQ.jpeg)

If you don’t know who [Dr. Robert Kowalski](https://en.wikipedia.org/wiki/Robert_Kowalski) is, he more or less invented logic programming in the 1970s. In the logic programming community, he is a “big deal.” He is also the first person I’m aware of to have used logic programming languages as a knowledge representation tool for legislation. So in that sense he can also be said to have invented at least one category of “Rules as Code.”

Using formal logic-based languages to encode legal knowledge is my day job, and considered quite cutting edge, so it’s remarkable that Bob was doing it 40 years ago. And he has evidently lost no enthusiasm for the idea and it’s potential for law.

The idea of encoding legal knowledge in natural-language like logical tools has seen a resurgence recently, he says. A point that he emphasizes in his presentation by pointing to other people who have recently been creating natural-language-like ways of encoding legal knowledge in a logic programming paradigm, including-much to my surprise-me and my work on Blawx!

That’s a delightful surprise, and thanks to Matthew Waddington for pointing it out, or I might not have noticed. :)

If you would like to see Dr. Kowalski’s talk, a recording was made, and is [available here](https://utdallas.box.com/shared/static/ngsyloscj5sk24uh3axexxz451o74z0u.mp4). It’s a technical introduction to Logical English, and how you could use it to encode a set of legal rules.

He acknowledges that there are debates to be had about whether natural-language methods of writing logical code are easier to write, but that they are undoubtedly easier to read. I would go further, and say that in my personal experience drafting legal knowledge in a controlled natural language, without more, is more difficult than in a typical programming language. It can be made easier, but it largely hasn’t been.

If you would like to see an interactive example of Logical English, there is an [implementation of Rock Paper Scissors in LPS (Logical Production System)](http://demo.logicalcontracts.com/example/RockPaperScissorsBaseEN.pl), which is on online SWI-Prolog-based tool that allows you to set out event calculus scenarios and visualize the outcomes by “running” them.

It is an interesting demo for at least two reasons: First because it sets out the rules entirely in logical english, and secondly because it gets translated into event calculus and then visualized like this:

![](/1__nJ84cusDo1ZPizil59ZL7w.png)

Oh, did I mention Dr. Kowalski invented event calculus, too? Yeah. Like I said, he’s a “big deal.”

In fact, the first time I learned about LPS, I was sort of unimpressed with it as a means of encoding legal knowledge, because not all kinds of legal knowledge fit into the pattern of events and actions and fluents. It seemed restricting to me. Now that I have actually come to understand what event calculus does, I realize that the problem was not LPS, but my mis-interpretation of the demonstrations. The demonstrations were demonstrating the event calculus features of LPS because that’s the cool part that creates pretty pictures. But all of the power of logic programming remains, just with event calculus built on top.

The value of that is much clearer to me, know.

It feels as though I’ve spent most of my career in computational law slowly realizing things Bob has known for 40 years. So when he talks, I listen. And when he points to my work as part of a movement toward natural langauge encoding of legal knowledge in logical programming, I’m delighted.

Wait until he sees what we are building at SMU. I have spent the last couple of days learning more about what our language L4 is capable of doing on the natural language generation front. And what is marvelous about the techniques we are using is that they go both ways. If you can generate natural language from code, you can generate code from natural language. The same software does both things.

So if you want a natural language interface to a logical programming language for encoding legal knowledge, that’s what we’re working on, too.