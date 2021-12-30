---
toc: true
title: Legal Expert Systems Just Got Smarter
description: >-
  For nearly 40 years, the automation of legal services has hit a brick wall
  when it came to dealing with subjective questions. Until…
date: '2018-10-19T09:28:19.685Z'
categories: []
keywords: []
slug: /@roundtablelaw/legal-expert-systems-just-got-smarter-e7e12b75e872
---

For nearly 40 years, the automation of legal services has hit a brick wall when it came to dealing with subjective questions. Until recently, it just wasn’t possible at all. Recently, machine learning has made it possible, but it can’t explain how it came to the conclusion.

We _need_ an automated system that can answer subjective questions, and explain its answers. It is a critical missing tool in the access to justice toolbox.

When a human lawyer comes across one of these questions, they make an argument based on precedent, using a technique called “reasoning by analogy to prior cases”. But as far as I am aware, there has never been a commercial or open-source tool to automate that process.

🚨**_UNTIL TODAY._**🤯

### Introducing docassemble-openlcbr

The [ABA Centre for Innovation](http://abacenterforinnovation.org/) named me an [Innovation Fellow for 2018/2019](http://abacenterforinnovation.org/fellowships/meet-our-fellows) for a project to create an open-source, free analogical reasoning tool for automating legal services. [Clio](https://www.clio.com/), the Canadian cloud-based law firm practice management software company, heard about what I was doing, and have made a donation to help make it happen.

![](/1____ROG8efF__R3phnARSzZrzg.png)

The reasoning algorithm, called IBP, comes from the [academic work of Kevin Ashley and Stefanie Brüninghaus](https://pdfs.semanticscholar.org/24a4/7ca6f5b2ebec9e809bf19d2b0f0da3dcab81.pdf). The open-source implementation of the IBP algorithm, called openlcbr, was written by Matthias Grabmair and is [available on GitHub](https://github.com/mgrabmair/openlcbr). [Docassemble](https://docassemble.org/) is a fantastic open-source legal expert system and document automation tool written by Jonathan Pyle.

In the IBP algorithm, an expert designs the analogical reasoning tool, and the computer does the grunt work to implement it.

My project is a piece of software that lets you access the capabilities of openlcbr when building a docassemble interview. It is called [docassemble-openlcbr, and it is available on GitHub](https://github.com/Gauntlet173/docassemble-openlcbr).

### Did I Mention It’s Completely Free?

It’s completely free.

### Listen… just try it.

[Click here to try the live demo.](https://testda.roundtablelaw.ca/interview?i=docassemble.openlcbr%3Adata%2Fquestions%2Fexplain_lcbr_test.yml)

It’s easy. There is a one-page web form that asks you about 26 yes or no questions in order to understand your fact scenario. Or if that sounds too much like work, there’s a default scenario ready to go. You click a button, and it predicts whether or not your trade secret has been misappropriated, and explains how it came to that conclusion.

![](/1__X6UQxNiNjrX__9__3HPVKO4Q.png)

### Gratitude is the Attitude

A huge thanks to Matthias Grabmair, Jonathan Pyle, and the docassemble community for their recent and anticipated future help with the coding.

If you are interested in contributing to the project, or think it’s something you might like to try and use once I make it easier (that’s coming), or are willing to give me feedback on new versions as they come out, or if you are a legal technology nerd and you like nerding out with nerds, please join the #analogyproject channel on the [docassemble slack](https://docassemble.slack.com), or [follow me on Twitter](http://www.twitter.com/RoundTableLaw) or both.

![](/1__5meN__6521iZexlNKcRHEbA.png)