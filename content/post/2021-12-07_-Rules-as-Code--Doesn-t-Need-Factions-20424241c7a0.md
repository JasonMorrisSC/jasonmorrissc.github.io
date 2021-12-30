---
toc: true
title: “Rules as Code” Doesn’t Need Factions
description: Responding to Catala Paper’s Criticisms of Logic Programming
date: '2021-12-07T20:41:46.393Z'
categories: []
keywords: [catala]
slug: /@roundtablelaw/rules-as-code-doesnt-need-factions-20424241c7a0
---

Last December, Denis Merigoux and Liane Huttner presented a paper at the Algorithmic Law Symposium hosted at HEC Paris, on “[Catala: Moving Towards the Future of Legal Expert Systems](https://hal.archives-ouvertes.fr/hal-02936606/)”. I’ve been meaning to take a look for some time, and finally managed it today.

[Edit: Denis Merigoux notes in the replies that the version of the paper I viewed was a pre-print before the authors had received the reviewer’s comments, which were only recently received, and some of which are echoed here. Revisions are anticipated shortly. This post should be read in the context that it critiques a work in progress.]

![](/1__Ce1g4hVewq0l____IkOaFXXg.png)

In the main the article is good. It sets out some real problems with how taxes and benefits are automated in government systems elsewhere, and suggests how [Catala](http://catala-lang.org), combined with literate- and pair-programming might serve to ameliorate those problems.

But the paper seems to take the position that Catala needs to be framed as “better than declarative logic programming.” And then the efforts to do that are all frustratingly flawed.

#### So-called “Rules Engines”

The paper distinguishes Catala from “so-called ‘rules engine’”, citing Flora-2 and Prolog as examples. The paper asserts that these rules engines are based on logic programming, interpreted, slow, and therefore ineffectual for calculating large numbers of taxes or benefits in a real-world application.

Catala, on the other hand, can be compiled to any other language, allowing Catala code to be implemented inside other legacy systems, and for it to be implemented in ways that are computationally efficient.

So the first problem here is that there is a difference between a “rules engine”, and a declarative logic programming language. Flora-2 and other members of the Prolog family of languages are examples of the latter. “Rules engine” is any piece of software that allows you to specify rules, and process data in accordance with them, and that typically exists outside of the rest of your application. So that includes applications like Oracle Intelligent Advisor, and production rule systems like Drools, which are categorically different things. You could use Flora-2 as a rules engine, and that would [not be a bad idea](https://blawx.com). But the features of “rules engines” are not the features of declarative logic programming languages. You could write your entire application in declarative logic programming.

The second problem here is that it is not true that logic programming languages are interpreted. Prolog can be interpreted or compiled, and can be very efficient. And modern software approaches to speed generally involve crossing boundaries between languages anyway. People use numpy in Python to do large mathematical calculations, but in the background numpy is implemented in code that was written in a more efficient language like C. You can do the same thing in declarative logic programming, and people do.

The third problem is that Catala has no exclusive claim to being capable of compiling to multiple other languages. If you wanted to take a knowledge representation written in a declarative logic programming language, and use that encoding to generate FORTRAN or COBOL, you could.

The fourth problem is that the paper claims that Catala provides explainability because of the one-to-one relationship between code and law. Based on my understanding of default logic, which Catala implements, and the other forms of defeasibility I have dealt with in declarative logic programming languages, like “logic programming with defaults and argumentation theories”, I think declarative logic approaches have a stronger claim to 1-to-1 correspondence than Catala. I suspect that is not a strength compared to logic programming, but a weakness.

And explainability is easier to get from declarative logic approaches, because while functions are unidirectional (from inputs to outputs), implications are bidirectional. So in declarative logic programming the same code can explain why something is true, and why it isn’t. Catala, as a functional language, can only explain the answers it found, not the answers it didn’t. And because it’s explanations are a trace, it can only provide an explanation that reflects the method that it used to reach that conclusion, as opposed to all the other ways the same conclusion might have been reached under the rules.

#### We Are All on the Same Side

Catala brings a combination of two things that haven’t existed together before. First, it is a domain-specific language for encoding quantitative statutes. That’s important, because it makes it easier for the programmer to encode those sorts of laws, than if they tried to do it in OCaml, for example. Easier is better.

Second, the programming language has features to prevent you from making certain kinds of mistakes (typing errors), and has the capacity to allow you to detect other kinds of consistency errors. So if what you want to do is write software for calculating taxes and benefits, and know that the calculations are not wrong in certain ways, Catala brings strengths to the table that aren’t available elsewhere.

That’s a good thing.

But all of the available approaches have strengths and weaknesses. Raw Python has the advantage of a huge pool of expertise to draw from. OpenFisca has the advantage of being designed specifically to process large number of queries simultaneously, while allowing you to write code in Python. Flora-2 has higher order logic programming, advanced defeasibility, meta-programming, all of which let you do interesting things. SWI-Prolog and s(CASP) give you access to constraints over infinite domains like time, goal-directed answer sets, natural language explanations, and the ability to answer questions that every other approach would loop infinitely on.

If we pit them against each other for no reason-and there is no reason-we risk leaving the Rules as Code community impoverished as a result.

I’m hoping that the [ProLaLa conference in January](https://popl22.sigplan.org/home/prolala-2022) can help us build a community of advocates for and practitioners of and researchers in computational law that sees no need for factionalism.