---
title: "Blawx_v1"
date: 2022-01-27T00:20:43-07:00
draft: true
toc: true
keywords: ['blawx']
---

## Announcing Blawx v1.0.0-alpha

I'm very happy to announce the release of [Blawx v1.0.0-alpha](https://github.com/Blawx/blawx).

## What is Blawx?

Blawx is an open source, web-based, user-friendly tool for Rules as Code. It allows you to
take what you know about laws, regulations, or contracts, and describe them to the computer
in a simple but powerful block-based visual language.

It is a project of my company, Lexpedite Legal Technologies, Ltd. In September of 2020 it was awarded
second place in the startup category of the American Legal Technology Awards.

## What's New?

There is very little about the application that hasn't been rebuilt from scratch.

### New Reasoning Engine

Blawx v0 was based on the Flora-2 language and reasoner. In Blawx v1, the back-end language is
s(CASP), and the reasoner is SWI-Prolog running an s(CASP) library. This has the effect of
expanding the sorts of questions that Blawx is capable of answering, and enables many of the
other new features.

### New and Improved Block Language

Using a new reasoner and language required revising the blocks available to the user in Blawx.

Blawx is now capable of expressing numerical constraints, logical constraints, and more. Existing
blocks have also been revised to make them easier to understand.

### Natural Language Explanations

The importance of being able to justify automated legal conclusions cannot be overstated. The
success of the entire Rules as Code enterprise may turn on its trustworthiness, which in turn
depends on independently-verifiable, legally-justified explanations for conclusions.

Blawx is now capable of not only answering your questions, but also of **explaining those answers in plain English**. 
It is also not only capable of answering why an answer was found, but unlike many similar reasoning tools
it can also explain why a conclusion was not reached.

### Multiple Justifications

Many tools will find a correct answer. Some tools will find all the correct answers. Blawx will
find all the correct answers, *and all the reasons each of those answers is correct*. This is
useful in legal applications to be aware of alternative methods of reaching the same legal
conclusion. It is also extremely helpful as a debugging tool, whether what you are trying to
debug is your encoding of the law, **or the law itself**.

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
`.blawx` file. The server is a Django app, and can be added to any existing Django
application.

### Live Code Generation

The Blawx interface will now show you the s(CASP) code that it is generating from your
workspace in real-time. This feature is designed to help transition intermediate and advanced
users from the visual interface of Blawx to the textual interface of s(CASP), when and if
that will make them more efficient.

But honestly, puzzles are more fun than typing, so I'm not sure why you would switch. :)

### Zero-Click Deploy to API

When you save your code in Blawx v1, you have also deployed it as a web API. No additional
work required. To use that web API in your applications, you only need to package
the relevant facts in a JSON payload, and send it to the API endpoint for your encoding.
No longer is there any need to export your `.blawx` file and include it in your application.
The API endpoint also provides a user-friendly interface for testing it directly.

### Error Messages

Error messages may not sound like any fun, but if something isn't working, error messages are
your clue as to why. Previously, Blawx did not have any error messages. Now, when something
goes wrong you get some help figuring out what, and why.

### Speed

Blawx now responds to most small queries almost instantly, speeding up your development
process and your applications.

## What is still to come?

It will take some time for the documentation to catch up to the changes in the code. That
work will continue. But we also have a long list of features that are going to be added
soon.

In the near future, the backlog includes:
* dates
* events and consequences
* default rules and exceptions
* associating code with elements of legal documents to enhance justifications
* simulating JSON input
* multiple deployed queries per encoding
* importing other workspaces on the same server
* automatically-generated web applications for testing deployed code
* ... and more

## Where Can I Try It?

