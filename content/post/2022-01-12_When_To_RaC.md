---
title: "2022 01 12_When_To_RaC"
date: 2022-01-13T17:27:58-07:00
draft: true
toc: true
keywords: ['basics']
---

## When Should I Use Rules as Code?

I was reminded today of this flowchart that was generated in New Zealand for 
explaining when Rules as Code might and might not be a good idea.

Basically, it asked whether or not you have rules that are prescriptive, and 
whether it would be helpful to automate them. Factors going into whether it
would be helpful included whether encoding would create service efficiencies
and whether multiple parties would use the encoded rules.

Factors for whether they were "prescriptive" included whether they were
"black and white", whether they require discretion or are subjective, and
could they be converted to an if-then logic statement.

## I'm Persnickety

I have problems with almost all of this.

### "Prescriptive"

When people say "prescriptive" in this context, I think they mean "determinative."
"Prescriptive," in the context of rule-making, means that the rule tells you
what to do, as opposed to proscriptive, which tells you what _not_ to do.

Some people have arguments about how prescriptive legislation and regulations
should be, in the sense of whether they should prescribe methods, or outcomes.

We don't want that conversation to overlap with the conversation in the case of
rules as code, which is not about prescriptivity, it's about the degree to which
the rules can be usefully encoded.

But even if you change the name of the criteria to "deterministic", it's too
simple. Let's say I have a piece of legislation and it has stuff in there that
is subjective, and vague. Things that would need to be decided by a judge's
opinion, or within the discretion of some other human being, on the basis of
factors that I can't really know in advance.

Is that a rule that you shouldn't use Rules as Code on? Not necessarily. Just
because a law has parts that cannot be calculated, because you don't know what
the formula would be, doesn't meant that you can't represent the output of
the formula, and usefully encode the rest.

Just because a law deals with discretion over whether a permission should be
granted doesn't mean that you can't usefully encode the parts of the law that
are deterministic, and can help the applicant figure out what their deadline
for filing an application is, or what the fine will be if they fail to.

All legislation has deterministic parts. And most legislation has subjective
parts. The mere existence of those parts is not a reason to do or not do
anything.

The question of whether to do Rules as Code needs to be answered in the context
of the reasons you're considering it. If what you want is to build an application
that will let people predict the decision of discretionary decision makers,
whose decisions are made on the basis of a wide array of factors that can't be
known in advance, then Rules as Code is less useful. If what you want to do is
build an application that helps people understand and navigate the process of
making those requests, Rules as Code can be helpful.

## Could They Be Converted Into "If This Then That" Logic Statement

They can. Of course they can. Rules aren't written in poetry. There may be
parts of those if then statements that you can't calculate, like "if the
judge likes you, you go free." You can't calculate "the judge likes you."
That can't be turned into if/then statements. But everything else can.

Again, what matters here is not whether the rules are possible to turn into
code (they are), what matters is whether the rules are possible to turn into
code that can do the things you want the code to do.

## If/Then

It bugs me that there are two different versions of if/then, and most people
talking about Rules as Code don't recognize and acknowledge the difference,
and that the difference is important.

Consider these two examples:

* "If the traffic light in front of you is red, stop
the car."
* "If a person is over the age of 18 then they are an adult."

Both of those are if-then statements, but the meaning of the "if-then" is
different.

In the first one, it is a series of steps, and conditions in which those
steps should or should not be followed. If sets out the conditions, and
then sets out the actions.

In the second one, it is a logical implication, setting out what we can
also know based on what we already know. If sets out the conditions, and
then sets out the conclusions that can be drawn.

You can see that these are different, because it makes sense to say
"if the person is an adult, then you know they are over the age of 18."
The relationship between the condition and the conclusion is bidirectional.
But it doesn't make any sense to say "if the car is stopped, the traffic
light is red." That is a uni-directional relationship through time.

So, can we turn legislation into if-then statements? Yes. Which kind?
Both kinds. Any computation can be implemented in any language that is
Turing complete, which means that langauges that use imperative if-then
and languages that use logical if-then are both capable of representing
laws.

The question isn't whether you can turn a law into if-then statements,
or whether a particular kind of if-then statement can be turned into code,
or whether a particular kind of if-then statement can be used to represent
a different kind of if-then statement. The answers to all of these questions
are "yes".

The question is what kind of if-then do laws use, and on that basis,
what sort of languages do we want to use to represent those rules?

And the answer is, laws use logical if-then more than they use imperative
if-then by a wide margin. If you write an imperative if-then statement,
you are only telling the computer a portion of what the person knows,
and you are limiting what the computer can do with that knowledge.

Every time someone takes a law and turns it into a decision tree, they
have converted all of the logical if-then statements in the law  
into imperative if-then statements, effectively reducing
the total amount of information available to the computer.

Decision trees are not how laws should be encoded, because that is not how
they are written, and it's not how they work.

## Service Efficiencies

Can you get the benefits of service efficiency by using Rules as Code? Yes.

Do you need to use Rules as Code to get those benefits? No.

If, in the entire world, only one person is ever going to write code to implement
your rules, and that code is only going to be used to do one specific thing,
and those rules are never going to change, there is no reason to do Rules
as Code.

You aren't "doing Rules as Code" unless the encoding of the rules is separate
from the application to which it is being used. There is no reason to separate
it unless you are going to use it twice, or it is easier to maintain that way.

You can also encode the entire statute to make it more re-usable for different
purposes, but that's also not worth it for doing one thing.

So Rules as Code is something that you can use to get service efficiencies, but
needing service efficiencies is not grounds to use it.

## Multiple Parties

Yes. Good. Mutliple parties is a good one. It makes much more sense to automate
using the Rules as Code approach, particularly the approach where you encode
legislation and turn it into an API, if there are multiple people who are going
to have use for those encodings.

Good. Now, you need to still be careful that the cost of doing the encoding is
worth the benefits. If multiple people will use it, but the benefits are very
small, it may not be worth the investment. But the number of people using it is
one important factor to consider.

## What else?

How much does it cost to do the Rules as Code approach as compared to the
alternatives? What are the benefits that come from using it? Is there an
appropriate balance? What do you want to do? Do you want to learn? Do you
want to improve the quality of your drafted rules? Do you want to do quality
assurance on built products? Build new web apps? Explain answers? Audit people?
Automate the navigation of complicated processes?

All of these things matter in terms of both whether and how to do Rules as Code.

