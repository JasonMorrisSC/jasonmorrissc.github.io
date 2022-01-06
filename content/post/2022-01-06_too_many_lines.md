---
title: "At Least Four Times Easier"
date: 2022-01-06T00:47:08-07:00
draft: true
keywords: [openfisca]
toc: true
---

## What I Think

I have a lot of thoughts in my head about Rules as Code, but I'm usually able to tell the difference
between things that I _think_, and things that I actually _know_.

One of those things I think, but don't know, is this:

> It is way easier to encode legislation in declarative logic programming languages with
> defeasibility than it is in imperative languages.

How much easier? My best current guess is at least four times easier.

## How We Could Know

Here's how I would like to move the above idea from "I think" to "I know":

I would take some real-world requirements, and
I would write code in both a defeasible declarative logic language, and in an imperative language.
I would try to make the code follow the same best practices. I would try to make the code do the same things. Then
I would measure things that can be measured like the ratio of words of legislation to lines of code, the
number of reformulations, the capabilities of the resulting code, etc.

## The Great News

The great news is that the experiment is essentially underway.

OpenFisca, oversimplified, is a Python library for writing
Rules as Code. I've been using it at work encoding the Old Age Security Act. In fact, I've been
using it to encode the Old Age Security Act in two different ways, the second of which would make
the code a good comparator for equivalent code in a declarative logic language. So pretty soon I'm
going to be able to write some declarative code that duplicates it, collect the data, and have some
pretty strong evidence one way or the other.

## My Current Best Guess

The bad news is there's a lot of work to do to get to the point where it's reasonable to start drawing
conclusions.
But we do have some data we can use to make less accurate measurements
in the meantime.

Here are some of the assumptions that we are making in order to come to a rough estimate:

* lines of code per word of legislation will remain relatively constant within a programming paradigm
* lines of code per word of legislation will remain relatively constant across different legislation types
* the legislation I have encoded is typical of larger categories of legislation
* the way I have encoded the legislation is typical of the programming paradigms
* the tools I am using are typical of their categories
* shorter code is easier to write
* the use cases my code is aimed at are typical
* etc.

### The Ratio for Imperative Languages: `~2+ loc/word`

[Section 3 of the Old Age Security Act](https://www.canlii.org/en/ca/laws/stat/rsc-1985-c-o-9/latest/rsc-1985-c-o-9.html#sec3subsec1) is roughly 500 words. So far - and I'm not finished yet - I have written 
[nearly 1000 lines of code](https://github.com/JasonMorrisSC/openfisca-canada/blob/isomorphic_version/openfisca_canada/variables/old_age_security_act/oas_s3.py) to implement that section in my
best-practices OpenFisca implementation, designed to mimic the capabilities of encodings I have done in declarative
languages. That's a ratio, because _I'm not done yet_, of at least 2 two lines
of code per word in the legislation.

### The Ratio for Decalarative Logic Languages is `~0.5 loc/word`

When I encoded Rule 34 for SMU CCLaw last year, the [legislation was just under 800 words](https://sso.agc.gov.sg/SL/LPA1966-S706-2015?ProvIds=P13-#pr34-). The encoding was done
in s(CASP), a defeasible declarative logic programming tool, and [the substantive code was less than 400 lines](https://github.com/smucclaw/r34_sCASP/blob/main/r34_amended.pl). 
That's a ratio of less than 0.5 lines of code per
word of legislation.

### _At Least Four Times Easier_: `~2+/~0.5 = ~4+` 

So if you want my best justifiable guess, as of today, of how much easier it is to use declarative logic programming
for Rules as Code over typical programming languages, my best guess is "at least four times easier."

If you have a better guess, let me know.