---
title: "Announcing Blawx v1.0.0-alpha"
date: 2022-02-02
draft: false
toc: true
keywords: ['blawx']
---

## Announcing Blawx v1.0.0-alpha

I'm very happy to announce the release of [Blawx v1.0.0-alpha](https://github.com/Lexpedite/blawx).

![Blawx Screenshot](/blawxv1screenshot.png)

## What is Blawx?

Blawx is an open source, web-based, user-friendly tool for Rules as Code. It allows you to
take what you know about laws, regulations, or contracts, and describe them to the computer
in a simple but powerful block-based visual language.

In September of 2020 it was awarded second place in the startup category of the American Legal Technology Awards.

## What's New in v1.0.0-alpha?

There is very little about the application that hasn't been rebuilt from scratch, which is
why we moved from version 0 to version 1. It's still alpha software, but it is completely
different alpha software.

### New Reasoning Engine

Blawx v0.x was based on the Flora-2 language and reasoner. In Blawx v1.0.0-alpha, the back-end language is
s(CASP), and the reasoner is SWI-Prolog running an s(CASP) library. This has the effect of
expanding the sorts of questions that Blawx is capable of answering, and enables many important new features.

### New and Improved Block Language

Using a new reasoner and language required revising the blocks available to the user in Blawx.

![Blawx Blocks](/blawxv1_blocks.png)

Blawx is now capable of expressing numerical constraints, logical constraints, and more. Existing
blocks have also been revised to make them easier to understand.

### Natural Language Explanations

The importance of being able to justify automated legal conclusions cannot be overstated. The
success of the entire Rules as Code enterprise may turn on its trustworthiness, which in turn
depends on independently-verifiable, legally-justified explanations for conclusions.

Blawx is now capable of not only answering your questions, but also of **explaining those answers in plain English**. 
It is also not only capable of answering why an answer was found, but unlike many similar reasoning tools
it can also explain why a conclusion was not reached.

![Blawx Explanation](/blawxv1_explanation.png)

### Multiple Justifications

Many tools will find a correct answer. Some tools will find all the correct answers. Blawx will
find all the correct answers, *and all the reasons each of those answers is correct*. This is
useful in legal applications to be aware of alternative methods of reaching the same legal
conclusion. It is also extremely helpful as a debugging tool, whether what you are trying to
debug is your encoding of the law, **or the law itself**.

![Blawx Multiple Explanations](/blawxv1_multiple_explanations.png)

### Hypothetical Reasoning

Blawx is now capable of reasoning about your rules **without describing any facts**. Simply use the
"assume" block to specify which statements the reasoner should consider possible, and it will
find hypothetical fact scenarios to answer your questions, and tell you which of the assumed
inputs were relevant to those scenarios.

This feature can be used to power contingent legal advice, procedural planning tasks, 
finding legislative loopholes, prompting users for relevant inputs in legal expert system
applications, and much, much more.

### Numerical Constraints

Blawx is now capable of dealing with numerical constraints, like "the tax rate
must not be higher than 0.10." It is able to reason across multiple constraints at the same
time, and combine their effects. This facilitates not only numerical reasoning, but reasoning
about dates and times, and reasoning about events and their consequences over time.

Dates and events will be added to Blawx soon.

### Server-Side Storage

Blawx now provides a web server with an administrative interface that allows you to manage
a database of encoded rules. When you edit rules in Blawx, they are saved to the server,
and will still be visible to you on a new device accessing that server, without the 
`.blawx` file.

![Blawx Server Side](/blawxv1_server_side.png)

### Live Code Generation

The Blawx interface will now show you the s(CASP) code that it is generating from your
workspace in real-time. This feature is designed to help transition intermediate and advanced
users from the visual interface of Blawx to the textual interface of s(CASP), when and if
that will make them more efficient.

![Blawx Live Code Generation](/blawxv1_code_gen.png)

But honestly, puzzles are more fun than typing, so I'm not sure why you would switch. :)

### Zero-Click Deploy to API

When you save your code in Blawx v1, you have also deployed it as a web API. No additional
work required. To use that web API in your applications, you only need to package
the relevant facts in a JSON payload, and send it to the API endpoint for your encoding.
No longer is there any need to export your `.blawx` file and include it in your application.
The API endpoint also provides a user-friendly interface for testing it directly.

![Blawx API](/blawxv1_api.png)

### Error Messages

Error messages may not sound like any fun, but if something isn't working, error messages are
your clue as to why. Previously, Blawx did not have any error messages. Now, when something
goes wrong you get some help figuring out what, and why.

### Speed

Blawx now responds to most small queries almost instantly, and has massively improved performance for larger queries, in the range of 100X for some queries.

## What is still to come?

Some of the things on the short-term to do list include:
* Documentation
* Examples
* Dates, and Durations
* Event Calculus
* Legal Source Text
* ... and much more ...

There are obviously also going to be a lot bugs to squash.

Ultimately, we are building up toward automatically generating explainable legal
expert systems on the basis of a friendly encoding of a law,
similar to those generated by [AustLII's DataLex](http://datalex.org/) tool.

## Where Can I Try It?

I'm going to be deploying the new version on www.Blawx.com shortly. Later today, with
any luck.

In the meantime, or if you prefer to be in an environment where strangers on the internet
can't delete your encodings, you can go to the repository and
follow the installation instructions to try it yourself locally.

## Thanks

This version of Blawx is only possible because of the genius and support 
of the people behind s(CASP), like Joaquín Arias and Gopal Gupta, 
the effort and willingness to help of the people behind the SWI-Prolog
implementation of s(CASP), in particular Jan Wielemaker.

That it is possible is one thing. That I have been given the opportunity to prove it
is thanks to my former employer, the Singapore Management University Centre for Computational Law, and my colleagues in the Benefits Delivery Modernization programme of Service Canada, and the Canada School of Public Service.

Their vision and commitment to open source has made it possible to get this far, and every step
we take in open source is a lesson learned for the entire world.