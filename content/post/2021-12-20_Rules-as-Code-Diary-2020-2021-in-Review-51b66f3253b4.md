---
title: Rules as Code Diary 2020–2021 in Review
description: >-
  It’s traditional at the end of the year to look back and see where you
  started, and how far you have come. I’m going to extend the look…
date: '2021-12-20T21:56:29.804Z'
categories: []
keywords: [scasp,catala]
slug: /@roundtablelaw/computational-law-diary-2020-2021-in-review-51b66f3253b4
toc: true
---

It’s traditional at the end of the year to look back and see where you started, and how far you have come. I’m going to extend the look back to 2020, because I started Computational Law Diary half way through 2020.

![](/1__kFH3QTivoKDtihgGFXX8FA.png)

### A Little Personal Background

I started my full-time career in computational law about the same time that I started the Computational Law Diary in July of 2020. I spent one year with the [Singapore Management University Centre for Computational Law](https://cclaw.smu.edu.sg/), as the Principal Research Engineer in Symbolic Artificial Intelligence. I travelled to Singapore in January of 2021, and finished my one-year term with the Centre in June of 2021. I then returned home to Canada, and started a position in September as Director of Rules as Code inside the Benefits Delivery Modernization Program inside Service Canada, which is responsible for [alpha.servicecanada.gc.ca](https://alpha.servicecanada.gc.ca).

Looking back, while my work responsibilities have changed between the two positions, the things that get me excited enough to write about haven’t changed much. From my perspective, at least, it feels like I have been learning about and implementing things in much the same way. It’s just that my work at SMU was focused on building better tools, and my work at Service Canada is focused on writing working code with the best tools we have.

### Highlights from 2020 and 2021

Here are some of the big things that happened over the first year and a half of the Computational Law Diary:

#### 2020: [Blawx Paper in MIT Computational Law Report](/post/roundtablelaw/computational-law-diary-responding-to-mas-writing-in-sign-7f6972cd977c)

I had the honour of publishing a paper in the MIT Computational Law Report demonstrating the use of my software Blawx to encode Covid-19 rules, and build a user-facing app on the basis of that encoding.

#### 2020: [Presentation at Legislative Drafting Conference](/post/roundtablelaw/computational-law-diary-drooling-over-rules-as-code-52aa0470b792)

I got to speak at a conference of legislative drafters about the future that I saw for Rules as Code, and why it might be important to get legislative drafters involved and excited.

#### 2020: [ALTA Awards](/post/roundtablelaw/computational-law-diary-blawx-runner-up-at-atla-and-visual-interfaces-rock-904809dfbd26)

Blawx was awarded runner-up in the 2020 (inaugural) American Legal Technology Awards in the startup category.

#### 2020: [OECD Report](https://www.oecd-ilibrary.org/governance/cracking-the-code_3afe6ba5-en)

The OECD Observatory for Public Sector Innovation published “Cracking the Code”, a comprehensive introduction to the state of the Rules as Code movement at the time. The paper cited by LLM thesis, and listed Blawx as an example of rules as code-specific technologies that might be deployed in the future.

#### 2021: [Updates to Blawx](/post/roundtablelaw/blawx-dev-notes-ca2bdf03cf3b)

Early in 2021 I released a significant increase in the features of Blawx. Later in the year, that version was deployed live on the blawx.com website.

#### 2021: [s(CASP)](/post/roundtablelaw/s-casp-as-a-rules-as-code-tool-97ec3435975c)

I first [learned about s(CASP) in 2020](/post/roundtablelaw/computational-law-diary-explainable-legal-ai-in-s-casp-19da0a5d956), and thought it was an interesting way to generate explanations from logical code. But it was in 2021 that I started working with it in earnest, including in the L4-Docassemble project, the ICAIL paper.

At a workshop on s(CASP) held at the International Conference on Logic Programming I did a presentation on my work in the legal realm with s(CASP). Later in the year I learned that [it was being re-implemented inside SWI-Prolog](/post/roundtablelaw/s-casp-swi-prolog-ccf8e53c951a). s(CASP) inside SWI-Prolog is now at the root of the best rules as code tech stack I can imagine.

s(CASP) is not merely marginally better than the other tools I have used. Access to the techniques of answer set programming has completely changed my expectations as to what a reasoning tool for computational law could and should do.

#### 2021: [L4-Docassemble](/post/roundtablelaw/introducing-l4-docassemble-69ce4b1fb1e7)

L4-Docassemble was a project I built while with the SMU Centre for Computational law, that demonstrated the ability to automatically generate a prototypical web-based expert system on the basis of an s(CASP) encoding of the relevant legislation, and a LExSIS file that set out the relevant data structure and how to express questions in English.

As far as I’m aware, it remains the only open source solution for generating legal expert systems with natural language explanations, and the only legal expert system tool anywhere with the ability to encode exceptions the way they were written in the original law.

#### 2021: [Debugging Legislation in ICAIL](/post/roundtablelaw/rules-as-code-extended-abstract-to-appear-in-icail-21-proceedings-7fc5dedbf8b8)

I published an extended abstract in the 2021 ICAIL proceedings talking about an experiment at SMU Centre for Computational Law where we were able to use s(CASP) in order to find and correct an error in our model draft legislation. As far as I am aware, this is the first time that answer set programming has been used to model legislative rules in the published literature.

#### 2021: [talk.RulesAsCode.com](https://talk.rulesascode.com) released

My company Lexpedite Legal Technology teamed up with Jameson Dempsey of Legal Hackers and Stanford CodeX fame, to create an online community for discussing Rules as Code, at talk.RulesAsCode.com. It has made my life better as a person doing Rules as Code. If you are working in the space, please join!

#### 2021: [OpenFisca for Expert Systems](/post/roundtablelaw/explainable-openfisca-4c718d0fdc58)

As part of my work inside Service Canada, I needed to build a proof of concept showing that OpenFisca, which is a rules as code tool aimed at microsimulation, could be used to power a user-facing application built on the same encoding of legislation.

The resulting prototype demonstrates that OpenFisca can be used to generate explanations for its conclusions, can be used to determine what additional questions might be relevant, and can be used to determine when questions will remain only contingently answered.

### Take-Aways

It seems like a productive 18 months. What I’m most pleased with is that we now have open source, free, public demonstrations of a number of things that did not exist 18 months ago:

*   A user-friendly interface to declarative logic programming for Rules as Code, with code that can be run over Web API.
*   Communications targeted at legislative drafters explaining what Rules as Code is, and why they should care.
*   In-depth research available to public servants on the state of Rules as Code as of 2020.
*   Open Source, explainable declarative logic languages with strong ecosystems, capable of everything that the best commercial tools in this space can do, and more.
*   Demonstrations of how those tools can be used to simplify user-facing application development.
*   Peer-reviewed publication showing how those tools can be used to improve legislative drafting.
*   A place for the international Rules as Code community to have conversations that persist, and to share resources, that can be used to get new members of the community up to speed faster.
*   A demonstration of how the world’s most popular rules as code tool can be used to facilitate both microsimulation and prototyping user-facing applications.

### What I’m Looking Forward To In 2022

There are a lot of assumptions that we are still operating under in the Rules as Code space, and I’m looking forward to having the opportunity to throw some real world evidence at those assumptions and see what sticks. Is declarative code easier to write and read and update than procedural code? Can declarative code do causal reasoning, temporal logic, math, defeasibility, scoping, and explanations all at the same time? We should be able to find out.

I’m also looking forward to the first ever Programming Languages and the Law workshop in January, the Computational Legal Studies Conference in March, deploying Canada’s first public facing rules as code powered tool in the spring, and finally getting the opportunity to experiment with the Catala programming language. In a perfect scenario, I would also have some time to spend writing and releasing a massively revised version of Blawx.

Perhaps most of all, I’m looking forward to all of the things I’m going to learn from having the experience of encoding legislation for people who have specific and varied things they need to do with that code. There is just no way to theorize your way through that kind of learning, and it has been extremely valuable.

### Thanks

Thanks to the people at SMU Centre for Computational Law, and the people at Digital Experience and Client Data who have been so supportive of not only my work, but my desire to write, present, and make videos about it, for the last 18 months.

And thank you for your interest, for reading, for your feedback, and for sharing my musings, here. Here’s to more of the same in 2022, but with less epidemic.