---
toc: true
title: 'Encoding “For Greater Clarity” Provisions'
description: >-
  Today has been a very interesting day in Alberta. News broke that the rules
  for dealing with Covid-19 had changed last Saturday, just…
date: '2020-09-04T18:41:01.338Z'
categories: []
keywords: [flora2]
slug: >-
  /@roundtablelaw/computational-law-diary-encoding-for-greater-clarity-provisions-698a83f4612c
---

Today has been a very interesting day in Alberta. News broke that the rules for dealing with Covid-19 had changed last Saturday, just before the start of school this week. The new order reads, in part, as follows:

> “… an operator of a school does not need to ensure that students, staff members, and visitors are able to maintain a minimum of 2 metres distance from every other person when a student, staff member or visitor is seated at a desk or table.”

This has caused a political furor in Alberta. That is to some great degree because people believe that the above language changes the rules, to make them less protective of children. I don’t think that’s actually the case. To my reading, the rules never required 2 metres of physical distancing among students and staff if the students were at their desks.

This raises an interesting question from the point of view of someone looking to encode rules. Not every rule that you encode changes the outcomes of any legal questions. If that’s so, do you need to encode it? And if you do, how do you write automatic tests to see if you did the encoding properly?

#### Encoding What Not To Consider

In contracts it’s not unusual to run across a clause that says “X does not include Y.” An example I’m familiar with is Y-Combinator’s Simple Agreement for Future Equity (SAFE) contract, which says in the definition of “Liquidity Capitalization” that the term “excludes the unissued option pool.”

When creating the rule that defines the value of liquidity capitalization, in most computer languages it is very easy to say `X=A+B+C`, and very difficult to say `X=A+B+C, but not D`. Most languages do not have a simple way to express information that should be ignored when considering something. And why would you want to?

Well, if someone later attempted to redefine Liquidity Capitalization as including the unissued option pool, in contradiction of this explicit exclusion, you would want your software to be able to notice that contradiction. So despite the fact it won’t change the answers to any questions now, we need to encode the rule now, in the hope that it will allow us to detect contradictions later.

One way to approach this would be to say that Liquidity Capitalization is defined as the sum of the value of a category of securities, where unissued options are defined out of that group. That might look like this in Flora-2:
```prolog
Liquidity_Capitalization_Part::Thing.  
Captial_Stock_Shares::Liquidity_Capitalization_Part.  
Issued_Options::Liquidity_Capitalization_Part.  
Converting_Securities::Liquidity_Capitalization_Part.  
\neg Unissued_Options::Liquidity_Capitalization_Part.
```
Saying that `Unissued_Options` are not (`\neg`) a type of (`::`) `Liquidity_Capitalization_Part` won’t change any answers now. But if later we introduce a rule that attempts to include them, we will be able to detect a conflict in the rules.

#### Programming vs. Knowledge Representation

This demonstrates the risk of encoding legislation with a view to the question or questions you are looking to answer. If you are just trying to get the answer to the question, efficiency demands you ignore factors in the law that are not relevant to that calculation. But you can’t know when they might _become_ relevant.

If you leave out the parts that aren’t important now, then when something changes in the rules you need to:

*   encode the change, and
*   go through _all the rules_ and _all your code_ to make sure the change didn’t make anything relevant that wasn’t before.

You want to avoid that second step, if at all possible. Which is why you want to encode legislation and contracts isomorphically. Each section of law or contract should have one corresponding section of code that encodes all the relevant knowledge.

Some methods of formalizing legal rules essentially force you to decide what question you are trying to answer before you can get started. Others let you say what the rules are, without requiring you to decide now what question you’re trying to answer. One short-hand way of thinking about it is that the first set of tools are for “programming”, and the second set are for “knowledge representation”.

Knowledge representation is better than programming.

#### Testing for Meaningless Code

Test-Driven Development is a methodology in software development where before you implement a feature, you write software to test whether or not you have successfully implemented it. Then you automate those tests to re-run every time you make changes to your code. This has a couple of benefits. First, it allows you to see when the code is working, as soon as it is working. Second, it allows you to see clearly when later changes “break” something that was previously working.

So let’s imagine you have a rule that does not currently impact on any of the sorts of answers that you might want to generate. You could write a test just to see if that rule is there. But that doesn’t tell you whether or not it ever does anything. But what do you want it to do? Well, at some point in the future, if there was another rule introduced with which the existing meaningless rule conflicted, you would want to notice the conflict.

So you can create a test that loads your code, then adds a conflicting rule, and then checks to see whether or not the conflict exists. If it does, the rule is doing what it is expected to do. If later that test begins to fail, either you have made changes that you did not intend to that eliminated the potential conflict, or your test has told you that the rule is no longer capable of being meaningful even in the face of that contradicting rule, and perhaps it should be removed from both the code and the natural language version.

This is another feather in the cap of tools like Flora-2 that have the ability to change the rules in the database at run-time. Being able to write code that will load a file, add a rule, run a test, then delete the added rule, will make it a lot easier to test for accurate encodings of “meaningless” rules.

#### People Matter

The controversy in Alberta today is not based on a difference between what the rules used to say and what they say now. It is based, instead, on a difference between what people _thought_ the rules used to say and what they _actually said_. The amendment made over the weekend may not have been intended to change what the rules mean, but to clarify it.

That this attempt at clarification is being characterized as a change in meaning demonstrates the challenge of communicating clearly to human readers when writing laws and contracts. It is not easy to write rules that both work well technically and are effective at communicating their own meaning to people.

And as long as people are going to want to rely on natural language versions of rules (as opposed to automated question-answering apps built with the encoded version) to know what to expect from one another, we will need to be able to include statements in encoded laws and contracts that truly are “for greater clarity.” And it would be nice to be able to test to see if we did it right.

### Quick Notes

#### Blawx Demo at Rules as Code Session of Legislative Drafting Conference

Preparations for the 2020 Legislative Drafting conference held (virtually) by the Canadian Institute for the Administration of Justice (see [their website](https://ciaj-icaj.ca/en/upcoming-programs/2020-legislative-drafting/) for registration details) are now in high gear. I will be presenting on Rules as Code and demonstrating [Blawx](https://www.blawx.com/). Join us if you can!

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._