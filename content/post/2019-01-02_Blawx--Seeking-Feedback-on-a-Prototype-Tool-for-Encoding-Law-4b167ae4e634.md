---
toc: true
title: 'Blawx: Seeking Feedback on a Prototype Tool for Encoding Law'
description: >-
  I study declarative programming tools for automating legal reasoning in my
  Computational Law LLM at the University of Alberta. What I have…
date: '2019-01-02T09:19:29.656Z'
categories: []
keywords: [blawx]
slug: >-
  /@roundtablelaw/blawx-seeking-feedback-on-a-prototype-tool-for-encoding-law-4b167ae4e634
---

I study declarative programming tools for automating legal reasoning in my Computational Law LLM at the University of Alberta. What I have learned is that they are both very powerful, and highly inaccessible for normal legal service providers.

I have had an idea in my head for how to solve that problem for a long time now. I’ve told people about it, but it’s the sort of thing that needs to be shown to be understood.

So take a look. And I’d really appreciate it if you’d take the time to share your initial reactions with [me on twitter](https://www.twitter.com/RoundTableLaw), even before you read the rest of the article. The point of the prototype is to learn something. And what I want to learn is “how do other people react when they see what I’m talking about.”

#### What Just Happened?

In the video, I’m using Blawx to do a number of things.

First, I describe how data about the real world is structured. I tell the software that there is a category of thing called a “Person”, and that a “Person” has an age, and a true or false attribute called “is a minor.”

Second, I tell the software something about the real world. There is a Person named Bob, who is 40.

Third, I encode a legal rule, the “age of majority”. I tell the software that any object (called “A”) is a minor if it is a person, and its age is less than 18.

Lastly, I ask the software to tell me whether it is true that Bob is a minor.

In a later version of the prototype, there will be a “Run” command. If you clicked “Run” at this point, the software would tell you “No.”

One of the things I’d love to know is how much of that you already knew after watching the video.

#### So what is Blawx, exactly?

Blawx will be a combination of different tools that work together.

First, Blawx is the interface you just saw (or a much better one), which is to allow you to encode data structures, facts, rules, and queries in a user friendly and interactive way, allowing you to generate test data as you go, and helping you to navigate from what you already know to what you would like to know using those rules.

Second, it is a reasoning tool. When you ask the interface a question, it is sent to the reasoner to generate an answer. The results of the reasoning are sent back to you in the design interface, so you can see the answer or answers to your queries. But the reasoning tool can interact with software other than the design interface. Once you have encoded something, that legal knowledge can be uploaded to a server, and then accessed by any other piece of software that might need it, inside or outside your organization.

#### What could you do with it?

So, there are a few tools out there that are already trying to take advantage of the power of declarative programming for legal reasoning. For starters, Blawx could be used to do something similar to what those tools do.

For example, [Neota Logic](https://www.neotalogic.com/) uses a similar technology to power interactive interviews and document automation. By connecting an interview tool like Docassemble to a Blawx server, you would be able to have the rules you encoded in Blawx guide Docassemble in deciding what questions to ask and in what order, and use advanced deductive logic in order to get the answers to complicated legal questions.

Similarly, there is a project in Australia called [Regulation as a Platform](https://digital-legislation.net/) or “Digital Legislation” that uses a similar technology to make it possible to have legal reasoning as a service online. Blawx can do very similar things.

But the point of Blawx is to provide more direct access to the power of declarative programming, so online legal reasoning for interviews and other applications is just the tip of the iceberg.

Blawx could be used for quality assurance on contracts. You could do analysis of the terms of a contract to determine whether, given a million random fact scenarios, there are any in which the contract results in undesired outcomes.

Blawx could be used for knowledge management. Firms could encode what they know about an area of law already, and merely by loading the existing code any other member of the firm would be able to see the most important questions to ask in order to get a preliminary answer on a legal issue.

Blawx could be used to generate litigation strategy, by asking it what additional facts or arguments would disprove a legal conclusion that seems likely, providing potential avenues for argument or investigation.

Blawx could also be used to power other forms of AI. Learning software needs to have the rules of the game encoded before they can master them. Blawx could be used to encode a set of legal rules, and then machine learning could be used to come up with a super-human strategy to achieve certain outcomes under those rules.

#### What’s Next?

There are a lot of steps that could be taken next. An obvious one is to get the reasoner connected to the interface prototype above so that you can see the reasoner’s answers to your queries in real time, and set it up as a live demo online that people can play with.

But really, where to go next depends on what I learn from this prototype.

What do you think? Is this a tool you could see yourself using (a future version of)? What about the video made perfect sense? Where did you get lost? If you had this tool, what would you want to try to do with it? Does that seem like it would be fun, or painful, or something in between?

Let me know how the video strikes you [on twitter](https://www.twitter.com/RoundTableLaw) or [by email](mailto:jason@roundtablelaw.ca), and I’ll decide where to go from there.

Thanks in advance.