---
toc: true
title: 'What does GPT-3 Mean for Rules as Code?'
description: >-
  GPT-3 is a newly announced AI model produced by OpenAI, that is showing some
  remarkable capabilities. I’m going to give you a quick…
date: '2020-07-17T21:49:15.577Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/computational-law-diary-what-does-gpt-3-mean-for-rules-as-code-d2f01caa6857
---

[GPT-3](https://arxiv.org/abs/2005.14165) is a newly announced AI model produced by [OpenAI](https://openai.com/), that is showing some remarkable capabilities. I’m going to give you a quick background into what it is, what it can do, and what that might mean for Rules as Code.

#### What is GPT-3?

GPT-3, simplified, is a piece of software that can predict what the next word will be in a string of text. It can do that over and over, to the point where it can effectively write text that is almost indistinguishable from human writing. You just need to give it the first bit, it will do the rest.

It does not seem to be a major change in technology. It seems to be an application of the same technology, but to way more data, on way stronger computers, and with a way bigger model.

What is truly remarkable about it is the degree to which it can “learn” a task to be completed by putting examples of that task into the text it needs to extend. For example, if you want to use GPT-3 to translate from English to French, you can give it a few lines of text being translated from one to the other, and then give it an English phrase, and it will continue the pattern by translating that phrase into English. It may also come up with a few phrases of its own, and translate them, too.

So GPT-3 is a general-purpose natural language generation tool that can be “trained” to do a task-even a task it has never seen before-with a few short examples.

#### What can it do?

One of the examples making the rounds is an example in which GPT-3 was used to translate from plain English into Legalese.

That’s a remarkable result, but I can’t endorse the idea it’s going to put lawyers out of work. It may ultimately change how lawyers spend their time.

But what I find more interesting from a legal technology perspective is actually this example in which the tool takes a plain langauge description of a website, and then codes that website in ReactJS.

Why is that more interesting from a legal perspective? Well, it is translating from a natural langauge to a formal programming language. And that could have a lot of potential use in the Rules as Code space.

#### How Could GPT-3 Help Rules as Code?

“Rules as Code” is the idea that legislation or regulation or contracts can and should be represented in software code at the time of drafting, because it makes them easier to automate, and it makes the natural language version better. The same technologies can be used to automate existing laws, but automating is harder, and the law doesn’t get better as a result. Rules as Code suggests that you should be using code from the beginning.

#### Controlled Natural Language: Pro and Con

I’m currently on contract with [Singapore Management University’s Centre for Computational Law](https://cclaw.smu.edu.sg/), which is doing research into domain specific programming languages for legal purposes including Rules as Code. One of the methods that we are looking at is called “controlled natural language.” There are a lot of examples of CNL out there, including [Attempto Controlled English](http://attempto.ifi.uzh.ch/site/), and the coding language used in [Oracle Intelligent Advisor](https://www.oracle.com/ca-en/applications/customer-experience/service/intelligent-advisor.html). You can also see it at work in [DataLex](http://austlii.community/wiki/DataLex).

The main upside of CNL is that it reads like English. It is therefore usually pretty easy for a human expert to read and understand, so that a non-programmer has a good chance of being able to read the encoding and determine whether or not it is consistent with their own understanding of the rules. That’s important for quality assurance in the development of legal apps.

The main downside of CNL, in my admittedly anecdotal experience, is that if it has a sufficiently sophisticated semantics it becomes difficult to learn to write. That’s a bit counter-intuitive, because it _looks_ like English. The reason for the difficulty in writing, I think, is precisely that the syntax and semantics is so similar to natural language, but also completely hidden. You are trying to write in the CNL, but you accidentally write in the natural language (NL) instead because they are so similar, NL is what you are used to, and you can’t see any visual cues to tell you you are doing it wrong.

Tools like [Blawx.com](https://blawx.com) solve that problem by going to the other extreme. In Blawx you have a limited number of expressions you can use. If you want to add to them you have to do it explicitly, and you can’t accidentally create a new concept by misspelling something. The syntax is visible in the screen, and if you try to put an element somewhere in the code it doesn’t belong, it won’t “fit”. That makes it harder to make mistakes while drafting, and the mistakes are easier to see, and learn from. But it may also make the code harder to read.

Another approach to this problem is using the text prediction capabilities of a tool like [Grammatical Framework](https://www.grammaticalframework.org/), in which you describe the grammar of the controlled natural language to the software, and the software can give you a list of the only words that will make sense next as you are drafting. That way you get the readability in the final result, but you have much better guidance while you are drafting. And GF will let you translate the result into any other language, natural or code, that can represent the abstract grammar.

So how does GPT-3 fit into this problem? Well, all the technologies right now sort of assume that the drafting of the code, whether in a CNL or otherwise, will be done by a subject matter expert. That is slow, and doesn’t scale well if you want to encode large libraries of legislative material, for example.

(That, by the way, is another one of the arguments for the idea that coders should be there when the law is drafted. It is a way of transitioning to encoded rules, without having to translate all the existing ones.)

#### Getting the AI to Help

There has been this open question of how helpful can we make AI in that process of taking a legal text and translating it into code of some sort. After all, translation is one of the things that AI has become astoundingly good at. Until now, there has been a lot of reasons to be pessimistic about the possibilities. Most tools trained on general natural language text tend to perform relatively poorly when given legal text. And there are not a lot of examples of translations from law to computer code for an AI to learn from, unlike for the task of translating between natural languages.

GPT-3 potentially helps solve both of those problems. First, it seems to be able to deal well with legalistic language, translating in and out of “plain English”. Second, it seems to be capable of learning a new translation task on the basis of a relatively small number of examples. So you wouldn’t need a huge database to get started.

Asking GPT-3 to generate, for example, [Flora-2](http://flora.sourceforge.net/) (aka ErgoLite) code on the basis of a statute would likely fail, because there is nothing sufficiently similar to Flora-2 in the data on which it was trained, or at least not enough of it. But it’s possible that a CNL would be sufficiently similar to natural language translation that GPT-3 would be better at generating code when the CNL was the target language.

Possible, but by no means certain. But it is worth finding out. Because that would put another plus in the CNL column as a potential solution for Rules as Code. In addition to something that it is relatively easy for humans to read, it may also be something that it is relatively easy for general-purpose AI to _translate into_. That could make drafting with the CNL easier (because the AI could do part of the work). It could also provide viable route to scaling the task of translating large libraries of existing rules into code, relatively quickly.

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._