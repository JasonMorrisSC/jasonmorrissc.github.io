---
toc: true
title: "s(CASP) + SWI-Prolog = \U0001F525"
description: >-
  It was back in December of 2020 that I first learned about a tool called
  s(CASP). Since then I have written about using s(CASP) to encode…
date: '2021-11-01T23:45:59.589Z'
categories: []
keywords: [scasp]
slug: /@roundtablelaw/s-casp-swi-prolog-ccf8e53c951a
---

It was back in December of 2020 that [I first learned about a tool called s(CASP)](/post/roundtablelaw/computational-law-diary-explainable-legal-ai-in-s-casp-19da0a5d956). Since then I have written about [using s(CASP) to encode Covid-19 rules using basic event calculus](/post/roundtablelaw/computational-law-diary-encoding-covid-19-rules-using-basic-event-calculus-in-s-casp-32a55b789eeb), I wrote about how it is [my new favourite tool for Rules as Code](/post/roundtablelaw/s-casp-as-a-rules-as-code-tool-97ec3435975c), I’ve written about [what it is](/post/roundtablelaw/what-is-justified-stable-model-constraint-query-driven-answer-set-programming-c895955bf68c), and [why that matters](/post/roundtablelaw/and-why-it-matters-for-rules-as-code-8610e49511c8) for Rules as Code, about how [you can use it to make laws better](/post/roundtablelaw/how-rules-as-code-makes-laws-better-115ab62ab6c4), and I introduced [a new tool for user-facing legal expert systems powered by s(CASP) encodings of rules](/post/roundtablelaw/introducing-l4-docassemble-69ce4b1fb1e7), and published an [extended abstract in the ICAIL ’21 proceedings](/post/roundtablelaw/rules-as-code-extended-abstract-to-appear-in-icail-21-proceedings-7fc5dedbf8b8) based on an encoding done in s(CASP).

I’m excited about it, to say the least. I was also concerned, because the only implementation of s(CASP) that I was aware of was created by a relatively small number of developers, people who are primarily university professors, in a language that is relatively unfamiliar to most people in the logic programming world, called Ciao. The software works, but it did not have a lot of features that made it easy to integrate with other tools. Making s(CASP) play nice with others was challenging. It also didn’t have a lot of supporting tools, like development environments.

That has now changed, and in a very big way.

SWI-Prolog is the world’s most popular version of Prolog. It has been developed since the late 80’s by Jan Wielemaker. Wielemaker saw what s(CASP) can do (which includes things that until then SWI-Prolog could not do), and decided to re-implement it as a SWI-Prolog extension.

That means is s(CASP) now lives within the SWI-Prolog ecosystem, and gets the benefits of that swanky address.

Do you want an online web-based coding environment where you can write s(CASP) in the browser and see the results of your queries, including tree-structured explanations? No problem. [Swish](https://swish.swi-prolog.org/), the online SWI-Prolog IDE, already exists. And now it has an s(CASP) mode, that will work in either a program or a Jupyter-style notebook!

![](/1__SmgCHuBAcUaTQ3i8JRe__5Q.png)

What’s that, you say? You’d like to use a web API that accepts s(CASP) encodings and queries and facts, and returns answers and explanations over JSON, to power your user-facing Rules as Code applications? And you’d like a one-page form that mimics the output of the web API in HTML or JSON? [No problem](https://dev.swi-prolog.org/scasp/). SWI-Prolog is used to delivering things online.

![](/1__pg0KFbaYnn3C9SDt9__1f3w.png)

The implementation is still a work in progress, but I’m taking it as given that it will soon be feature complete. Which means that my job, as a person who is trying to advocate for adopting the _best_ tools for Rules as Code in a risk averse public service environment, has gotten a _lot easier_.

Check this out. I can take a version of the Rock Paper Scissors code I wrote in s(CASP), and publish it on Swish as a publicly available example that you can log into, and press “Run”, and it will show you the s(CASP) model and the s(CASP) explanation.

![](/1__w5p43Sr4tLejCBMlXDBlLA.png)

Creating an interactive demo like that was impossible 6 months ago. Now it takes less than 5 minutes. [Try it yourself](https://swish.swi-prolog.org/p/rps_scasp.pl). And for the purposes of Rules as Code, having a place where you can look at the legal encoding, and play with it, and examine it, and test it, all in a browser, and without having to install a single piece of software, all on the basis of open source code, is exactly the sort of transparency and accountability we need.

I’m still not 100% sure that s(CASP) can do everything I need to to, as of right now. In particular, I’m concerned about date math. But finding out just got a lot easier.