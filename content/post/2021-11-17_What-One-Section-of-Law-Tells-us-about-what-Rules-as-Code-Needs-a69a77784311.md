---
toc: true
title: What One Section of Law Tells us about what Rules as Code Needs
description: >-
  I’m currently working within Service Canada’s Benefits Delivery Modernization
  Program, and I’m working with a team that is looking to…
date: '2021-11-17T20:38:33.090Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/what-one-section-of-law-tells-us-about-what-rules-as-code-needs-a69a77784311
---

I’m currently working within Service Canada’s Benefits Delivery Modernization Program, and I’m working with a team that is looking to automate calculations under the Old Age Security Act.

I have been looking at various approaches for encoding the relevant sections of the Act, and over the last couple of days I’ve been struck by what a good example section 3(1) of the OAS Act is of the kinds of features that we need in Rules as Code technologies.

### Section 3(1), OAS Act

[Section 3(1) of the OAS Act](https://www.canlii.org/en/ca/laws/stat/rsc-1985-c-o-9/latest/rsc-1985-c-o-9.html#sec3subsec1) reads as follows:

> **3** **(1)** Subject to this Act and the regulations, a full monthly pension may be paid to

> **(a)** every person who was a pensioner on July 1, 1977;

> **(b)** every person who

> **(i)** on July 1, 1977 was not a pensioner but had attained twenty-five years of age and resided in Canada or, if that person did not reside in Canada, had resided in Canada for any period after attaining eighteen years of age or possessed a valid immigration visa,

> **(ii)** has attained sixty-five years of age, and

> **(iii)** has resided in Canada for the ten years immediately preceding the day on which that person’s application is approved or, if that person has not so resided, has, after attaining eighteen years of age, been present in Canada prior to those ten years for an aggregate period at least equal to three times the aggregate periods of absence from Canada during those ten years, and has resided in Canada for at least one year immediately preceding the day on which that person’s application is approved; and

> **(c)** every person who

> **(i)** was not a pensioner on July 1, 1977,

> **(ii)** has attained sixty-five years of age, and

> **(iii)** has resided in Canada after attaining eighteen years of age and prior to the day on which that person’s application is approved for an aggregate period of at least forty years.

Here are some portions of the text and the challenges they create for Rules as Code tools.

### “Subject to this Act and the regulations”

Yeah, we’re starting right at the very beginning. And this section of text actually creates two different problems.

#### “Subject to” : Defeasibility

The first problem is that I want to be able to say, inside the encoding for section 3(1), that section 3(1) is a default— that if there is another rule that concludes to the contrary, the other rule wins.

Most systems do not provide a simple way of encoding that _inside the code that represents section 3(1)_. Most require you to go elsewhere. A great tool for Rules as Code would allow me to say this inside (or at least alongside) the encoding for section 3(1), to maintain a 1:1 relationship between small sections of law and small sections of code.

#### “this Act and the regulations” : Scope

Even the tools that I’m aware of that have features for defeasibility, and those tools where I have implemented defeasibility features on my own, I’m not aware of any tool that allows me to describe which rules defeat which other rules at a level of abstraction higher than the specific rule itself.

You could argue that “this Act and the regulations” is pretty darned close to “anything”, and probably doesn’t need to be specified. But even if that was good enough (and for a lot of purposes it wouldn’t be), legislation regularly does things like day “for the purposes of Part 3”, or “subject to Division II”.

A great Rules as Code tool would allow me to refer to specific rules, or groups of rules, in a way that respects the structure of the original legislation. This is one of the places where I feel that tools like Akoma Ntoso have something to add to the Rules as Code toolkit.

### “every person who was a pensioner on July 1, 1977”

Any time that you see the phrase “on {date}” in legislation, you are dealing with a variable that changes over time, called a “fluent”. My birthdate doesn’t change, no matter how long I am alive. But my age does. Age is an example of a fluent.

Some Rules as Code tools out there do a really good job of making it straightforward to represent fluents. OpenFisca takes the approach of simply forcing all variables to be represented as fluents. That works, generally, but can cause some overhead. Oracle Intelligent Advisor creates a fluent whenever you explicitly calculate one based on a series of values and times. Then, if you calculate anything else using the value of that fluent, the things which depend on it become fluents automatically, too.

Those methods work by calculating a list of values, and the corresponding dates at which those values began to be true. There are other approaches, such as event calculus, which can be used in tools like LPS.

Whatever tool it is, you frequently need to be able to express how the rules change the state of the world over time, and check to see what the state of the world was as of a particular date. A great Rules as Code tool would make that easy.

### “not a pensioner”

Negation is extremely complicated. When you say “not a pensioner”, does that mean “there are no facts in the system from which it is known or derivable that the person was a pensioner?” Or, does it mean “we know or can derive that the person was categorically not a pensioner?”

Being able to express those two different meanings of “not”, and being able to reason about their combinations and implications, is something that a great Rules as Code tool needs to be able to do.

### “or” (and “and” and “but”)

The rule sets out non-exclusive disjunctive alternatives, where “at least one” needs to be satisfied. Most Rules as Code tools are able to give you a clear way to express “at least one” disjunction. Some also give you a way to specify mutually exclusive disjunction, where “only one” is true. All of them give you the ability to express conjunction, which is sometimes explicit when the legislation uses words like “and” and “but”, but is most often implied in legislative text.

### “has attained sixty-five years of age”

This shouldn’t be surprising, but there are a lot of occasions on which the specific piece of information that a legislative clause expresses is a duration. In this case, “having attained sixty-five years of age” means that the person’s age is greater than or equal to 65. But what is a person’s age? It is is the difference between the relevant date (in this case, “today”), and the person’s birthdate, measured in the units of years. But you could choose a different unit. You could say how old is the person in years and months?

The ability to easily express and manipulate time periods and durations is critical to being able to encode legislation with things like deadlines. Pieces of legislation will often create their own defined periods like “work week” or “holiday period”. In fact, the very next section of the OAS Act refers to a time period defined in the Act, called a “payment quarter”. A great Rules as Code tool need to be able to facilitate that sort of conversation about time periods.

### “an aggregate period”

In this case, what the legislation is asking is for the sum of all of the relevant time periods. That ability to easily calculate aggreagates (sum, count, average, etc.) is something that a good Rules as Code tool needs to have.

### “at least equal to”

This is a mathematical comparitor, in this case the “greater than or equal to” comparitor, `>=`. A great Rules as Code tool needs to have the ability to do mathematical comparisons against values.

### “three times”

This is an arithmetic operation, specifically multiplication. A great Rules as Code tool needs to give you the ability to express arithmetic operations against values. Ideally, it would be possible to express them as relationships, rather than as imperative procedures. That way “X * 3 = 15” could be used to find 5, in addition to “5 * 3 = X” being used to find 15. That might require the use of constraint programming, for example.

In this case, you may be interested in answering the question “does bob qualify?” But you may also be interested in asking “when will bob qualify?” And in order to calculate both answers on the basis of the same rules and the same facts, the arithmetic relationships as expressed need to be reversible.

### “ten years immediately preceding”

Again, we have a duration, but now we also have language that is dealing with a series of periods, and the relationships between them and dates. In this case, the ten year period immediately preceding a date. A great Rules as Code tool needs to understand phrases like “previous month,” and “subsequent work week”.

There are different ways of doing this. One more advanced approach is Allen Interval Logic, which allows you to express that one period immediately precedes another, without needing to know precisely what the start and end dates of those periods are. Most tools would require you to specify at least two of start date, end date, and duration, and calculate the third for you.

### “the day on which that person’s application is approved”

This statement requires the consideration of counter-factual or hypothetical facts. Section 3 sets out when a person may be paid a full monthly pension. Presumably, you cannot approve the application of a person for whom section 3 does not make paying the pension possible. But the decision of whether they qualify needs to be made in advance of the actual approval. Which means that while section 3 is being considered, there is only a hypothetical “day on which the person’s application is approved.”

If you ask, without using hypotheticals, whether the person qualifies under section 3 immediately prior to being approved, the answer is at best “unknown”, because there is no date on which the person’s application is approved, so residency for the year prior cannot be calculated. What you need to do is to be able to express the idea “assuming I approved this application now, would the person qualify under section 3?”

That is a different question, and a great Rules as Code tool would allow you to ask that question in particular.

### All that in section 3(1)?

Just from this one small section of law, we find we need:

*   defeasibility,
*   scope,
*   conjunction,
*   disjunction,
*   negation,
*   durations,
*   dates,
*   aggregates,
*   comparators,
*   arithmetic,
*   interval relations, and
*   hypothetical reasoning

And there are probably more things I’m leaving out.

What’s frustrating is I can count on the fingers of zero hands the number of Rules as Code tools I am aware of that make it easy to do all of those things. I can count on the finger of zero hands the number of Rules as Code tools in which this is all currently even possible. Which is frustrating, because I don’t think it needs to be that way.