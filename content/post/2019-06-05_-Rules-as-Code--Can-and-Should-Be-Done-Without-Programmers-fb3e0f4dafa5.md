---
toc: true
title: “Rules as Code” Can and Should Be Done Without Programmers
description: An Example from Law to Code to App
date: '2019-06-05T19:04:02.594Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/rules-as-code-can-and-should-be-done-without-programmers-fb3e0f4dafa5
---

I’ve been having conversations with people on twitter recently about my aspirational view that encoding laws will not always require programmers to be involved.

I am both a lawyer and a programmer, so it’s easy for me to be wrong about that sort of thing. So I’d like to share an example of why I think it’s possible, and you can decide for yourself.

The basic idea of Rules as Code is we take rules, we encode them, and that allows us and others to build more and better helpful applications.

#### The Tool: DataLex

[DataLex](http://austlii.community/foswiki/DataLex/WebHome) is a tool offered by [AustLii](https://www.austlii.edu.au/) that allows you to encode legislation and turn it into a chatbot.

#### The Law: Section 2 of the MHA

I do a lot of work in mental health in Alberta, so I took one section of the [Mental Health Act](https://www.canlii.org/en/ab/laws/stat/rsa-2000-c-m-13/latest/rsa-2000-c-m-13.html), RSA 2000 c M-13, as my example law:

> **Admission certificate**

> 2 When a physician examines a person and is of the opinion that the person is

> (a) suffering from mental disorder,

> (b) likely to cause harm to the person or others or to suffer substantial mental or physical deterioration or serious physical impairment, and

> (c) unsuitable for admission to a facility other than as a formal patient,

> the physician may, not later than 24 hours after the examination, issue an admission certificate in the prescribed form with respect to the person.

#### The Code

DataLex allows you to encode legislation in a way that reads very much like how the legislation is drafted. Here is an example of my test encoding of this section:
```
PERSON the physician  
PERSON the individual

LINK Section 2 of the Mental Health Act TO [https://www.canlii.org/en/ab/laws/stat/rsa-2000-c-m-13/latest/rsa-2000-c-m-13.html#sec2](https://www.canlii.org/en/ab/laws/stat/rsa-2000-c-m-13/latest/rsa-2000-c-m-13.html#sec2)

GOAL RULE Section 2 of the Mental Health Act PROVIDES

Section 2 of the Mental Health Act applies, and the physician may issue an admission certificate in prescribed form with respect to the individual ONLY IF

the physician has examined the individual in the last 24 hours AND

the physician is of the opinion that the individual is suffering from mental disorder AND

the physician is of the opinion that the individual is likely to cause harm to the individual or others or to suffer substantial mental or physical deterioration or serious physical impairment AND

the physician is of the opinion that the individual is unsuitable for admission to a facility other than as a formal patient
```
_I don’t think that requires a programmer_. Read the law, and the code, and tell me that only a programmer could get from A to B.

I think it’s realistic to expect non-programmers to be able to learn to express legislative concepts in that sort of a language, and to expect them to gain expertise at it.

#### The App

But if that’s the code, what is the application? Well, here’s what happens when you run the code:

![](/1__XpMCbZGqShDay3HcMRE8pQ.png)

First, the chat bot asks for the names and genders of the doctor and the individual. It then asks whether the requirements of the act are met, using the names of the people involved.

![](/1__MbjTxnQW1pepaHOZELwV2Q.png)

Then, the chat bot provides the answer to the question marked as a “goal” in the code, using the names of the individuals, providing reasons, and linking to the source legislation.

![](/1__Oyi8R6qcUUXu8C3YZHl9ag.png)

While the interview is going on, on the side of the screen the chatbot updates a list of the things it has been told, and the conclusions it has reached based on that information. For each fact, you can ask the chatbot to forget it. For each conclusion, you can ask for an explanation for how that conclusion was reached.

All of this is created with zero additional effort on top of encoding the law.

Seem unlikely? Try it yourself. Go to [the development page](http://www.datalex.org/dev/import/), copy and paste the code above into the “Edit DataLex knowledge-base” field, and click “Run Consultation”.

#### The Point

That’s what Rules as Code can mean, in practice.

Are there intricacies and expertise involved? Yes. Do you need the person using a tool like DataLex to be trained in how to use it properly? Yes. Does that person _need_ to be a software developer by profession?

No, they don’t.

But even if we _can_ do rules as code without programmers, _should we_?

If we care about access to justice, then yes, we should.

Automating legal reasoning has a lot of applications in access to justice that have not been exploited. The bottleneck in the use of this technology over the last nearly four decades is “knowledge acquisition”-getting what the subject matter expert (in this case, probably a lawyer) knows into the programmer’s head. People have been saying so since before Richard Susskind wrote his PhD on the topic in 1986.

But, if the “coding” tool is easy enough for the subject matter expert to use by themselves, that problem disappears. The technology will get used more, it will be more efficient to develop solutions, and those solutions will be of higher quality.

That’s why we should aspire to take the programmers out of it: Having to teach the programmers what we already know before we can turn it into an app is _slowing us down_.