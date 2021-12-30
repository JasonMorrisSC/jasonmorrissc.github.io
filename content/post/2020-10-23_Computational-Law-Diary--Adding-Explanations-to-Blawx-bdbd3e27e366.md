---
toc: true
title: 'Adding Explanations to Blawx'
description: >-
  Blawx is my web-based tool for Rules as Code. It is a graphical development
  environment that allows you to easily learn to encode legal…
date: '2020-10-23T21:22:04.609Z'
categories: []
keywords: [blawx,flora2]
slug: >-
  /@roundtablelaw/computational-law-diary-adding-explanations-to-blawx-bdbd3e27e366
---

![](/1__9__ywTDz6VshjxSKiK6xVQQ.png)

[Blawx](https://www.blawx.com) is my web-based tool for Rules as Code. It is a graphical development environment that allows you to easily learn to encode legal knowledge, describe fact scenarios, and ask questions and get answers.

One of the features that is missing from Blawx is explanations for those answers. Explanations are a very important part of Rules as Code for two reasons. First, they provide the user with context and reasons for the results that your tool provides, which goes to the accountability and transparency of your tool, and increases its trustworthiness. Second, explanations are an effective tool for knowledge engineers to troubleshoot the encoding as they are writing it.

So explanations have been high on the “to do” list for Blawx features since day 1.

### Reinventing the “Why” Wheel

One of the most frustrating parts of dealing with Rules as Code in 2020 is how much of the technology we need was readily available 40 years ago, and has now disappeared.

> Expert systems that were capable of explaining their conclusions were commonplace in the 1980s. Now, there are no open source tools available for download that include explanations as a feature. All of the previous art is hidden in academic writings.

As it happens, some time ago I came across [a paper from June of 1986](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-8640.1986.tb00079.x), by Sterling and Lalee, entitled “An Explanation Shell for Expert Systems.” This paper thankfully set out an example of how you can write a generic explanation system for any expert system written in Prolog.

Blawx uses a language called [Ergo Lite](http://flora.sourceforge.net/), which is in the Prolog family of languages. The implementation in that paper gave me what I needed to start reinventing the wheel so that people can ask “why” about conclusions they get from Blawx.

I started work on it ages ago, and haven’t touched it since last June. This week I reacquainted myself with what I had accomplished so far, and I was pleasantly surprised.

What is wonderful about the meta-programming approach that Sterling and Lalee set out is that you can make any yes/no query in Blawx explainable without needing to change the rules at all. It allows you to just add an “Explanation” block to Blawx’s toolbox to give users access to the new command. The Explanation block will accept any “yes/no” query, return the answer, and (eventually) return a tree-structured explanation for how that answer was obtained.

### Meta-Data for Meta-Programming

You don’t _need_ to change the Rules, but you _can_. ErgoLite has features for providing meta-data about Rules. If Blawx’s interface can be updated to allow the user to do that, that meta-data can be used to define how the explanations should read. Here’s an example of a set of silly rules I have been using for testing, with meta-data provided in the Ergo Lite syntax.
```prolog
@!{What_It_Takes_To_Be_Bigbird[source->'My friend', description->'to be a bigbird, you have to be tall, yellow, and eligible.']}  
bigbird(?X) :-  
    tall(?X),  
    yellow(?X),  
    eligible(?X).

@!{What_Makes_You_Eligible[source->'My friend',description->'to be eligible, you have to be old or military.']}  
eligible(?X) :-  
    old(?X);  
    military(?X).

old(BB).  
tall(BB).  
yellow(BB).
```
The first rule explains what you have to be in order to qualify as a “Big Bird.” The second rule explains what you have to be in order to be “eligible.” And the facts are laid out that BB is old, tall, and yellow.

With the exception of the `source` and `description` meta-data provided for the two rules, this is functionally equivalent to the sort of ErgoLite code that Blawx already generates.

When you run the query `?- solve(bigbird(BB)).` , which for our purposes means “explain why BB is a bigbird,” you get the following results:
```
According to My friend, to be a bigbird, you have to be tall, yellow, and eligible..  
bigbird(BB) can be proven if (tall(BB), (yellow(BB), eligible(BB))) can be proven.  
tall(BB) is a base fact.  
yellow(BB) is a base fact.  
According to My friend, to be eligible, you have to be old or military..  
eligible(BB) can be proven if (old(BB); military(BB)) can be proven.  
old(BB) is a base fact.  
Disjunction of old(BB) or [military(BB)] is satisfied.  
Conjunction of yellow(BB) and [eligible(BB)] is satisfied.  
Conjunction of tall(BB) and [(yellow(BB), eligible(BB))] is satisfied.
```
The result is by no means perfect. There is a lot of work to be done to fit it into a structure that will make it possible to let the user start at the main conclusion and drill down into the reasons for sub-sections of it. But what I’ve got suggests that the technical obstacles to an explanation feature for Blawx have fallen.

Now it’s just a matter of writing the code.

### Quick Notes

*   Oct 29: Stanford CodeX lunch webinar on [Blawx](https://www.blawx.com/) (free to join, if you’re interested)
*   Nov 4: Cyberjustice Laboratory (University of Montreal) webinar on [Blawx](https://www.blawx.com/)
*   Nov 17: European Commission Rules as Code [Blawx](https://www.blawx.com/) demo in conjunction with the OECD’s “[Government After Shock](https://gov-after-shock.oecd-opsi.org/)” event.
*   Nov 17&18: Guest lecturing for Megan Ma’s course at Sciences Po.
*   Nov 24: Rules as Code session with Canada School of Public Service

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._