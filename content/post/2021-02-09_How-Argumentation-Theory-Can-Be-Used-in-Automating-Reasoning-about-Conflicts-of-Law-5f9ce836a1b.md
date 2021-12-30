---
toc: true
title: >-
  How Argumentation Theory Can Be Used in Automating Reasoning about Conflicts
  of Law
description: >-
  Flora-2 is a logic programming language that implements defeasible reasoning
  using a method called “defaults and argumentation theories.”
date: '2021-02-09T12:09:52.307Z'
categories: []
keywords: [flora2]
slug: >-
  /@roundtablelaw/how-argumentation-theory-can-be-used-in-automating-reasoning-about-conflicts-of-law-5f9ce836a1b
---

Flora-2 is a logic programming language that implements defeasible reasoning using a method called “defaults and argumentation theories.”

Here’s how it works.

### Choose An Argumentation Theory

The first thing you have to do is choose the argumentation theory that you want, but Flora-2 has a very intuitive default.
```prolog
:- use_argumentation_theory.
```
### Tag Your Rules

Flora-2 gives you the option of specifying that some rules override others either by identifying the rules, or by identifying the conclusions. If you want to identify the rules, you need to give them tags. They can take more than one, and more than one rule can have the same tag. That’s done like this:
```prolog
@{tag}  
conclusion :- condition.
```
### Say What the Contradictions Are

Third, you need to be clear about what constitutes a contradiction. Flora-2 knows that `A` and `\neg A` are opposites, and can’t possibly be true at the same time. But it might not know that `CatLover` and `DogLover` are equally incompatible. (I kid, of course. No one likes cats.)

So it allows you to specify contradictions of conclusions like this:
```prolog
\opposes(conclusion1,conclusion2).
```
### Say What Overrides What

Lastly, you need to tell Flora-2 which rule, or conclusion, should win in a fight. That is done using the `\overrides` predicate, like this:
```prolog
@{default}  
flies(?X) :- bird(?X).

bird(?X) :- penguin(?X).

@{exception}  
\neg flies(?X) :- penguin(?X).

\overrides(exception,default).
```
### Then what happens?

In the background, Flora-2 is re-writing these rules for you before you run them, in two simple ways. First, if you tagged your rule, the conclusion of that rule is being given that tag in a different predicate. Second, a requirement is added to your rule, that it not be defeated.

So this:
```prolog
@{default}  
flies(?X) :- bird(?X).
```
Becomes these two statements:
```prolog
rule_handle(default,flies(?X)).  
flies(?X) :- bird(?X), \naf defeated(rule_handle(default,flies(?X)).
```
Then, the argumentation theory does the rest of the work.

### An Argumentation Theory

The job of an argumentation theory is to take the opposes statements and the overrides statements that you have provided, and figure out which conclusions are `defeated`.

The default argumentation theory in Flora-2 works like this:
```prolog
defeated(rule_handle(?T1,?H1)) :-  
    
  ?B1, // the conditions of the defeated rule are true  
    
  ?B2, // the conditions of the defeating rule are true  
    
  // the conclusions of both rules oppose each other  
  \opposes(rule_handle(?T1,?H1),rule_handle(?T2,?H2)),  
    
  // the second rule overrides the first rule  
  \overrides(rule_handle(?T2,?H2),rule_handle(?T1,?H1)),  
    
  // And the defeating rule is not itself defeated  
  \naf defeated(rule_handle(?T2,?H2)).
```
This is the basic defeating relationship. One conclusion defeats the other conclusion if the conclusions conflict, one conclusion overrides the other, and the overriding conclusion is not also defeated.

Additional rules are created to fill out the logic of the argumentation theory. In the default version used by Flora-2, those rules say:

*   A rule is defeated if it is refuted or rebutted by a rule that is not compromised, or if it is disqualified.
*   A rule is disqualified if it defeats itself.
*   A rule is compromised if it is either refuted or defeated.
*   A rule is refuted if there is another rule that conflicts with it and overrides it.
*   A rule is rebutted if it conflicts with another rule, neither is refuted, the rebutting rule is not compromised, and there is no priority between them.

That’s it. It’s a bit technical, but the point is not to understand the details. The point is to realize that an argumentation theory is not a huge deal to create.

### Why Do We Care About This in Law?

There are a few things that are really useful about this approach to defeasible reasoning, particularly in the legal realm.

#### First, It’s Easy To Use

The first of them is that it is very easy to use. You don’t have to write the argumentation theory yourself, you can just use one that’s provided. And if it doesn’t work the way you can expect, you can create a new one. All the different ways of doing defeasible reasoning can be implemented as argumentation theories, which gives you access to all of them in one place, but you never need to use anything other than `\override`, and `\oppose` to implement them.

#### Second, It Maintains a One-to-One Relationship Between Rules and Code

Second, the `\override` command can go with the rule that it belongs to, when you are trying to encode laws. Take for example these two sets of rules:

1\. Birds fly.  
2\. Penguins are birds.  
3\. Notwithstanding 1, penguins don't fly.

…

1\. Subject to 3, birds fly.  
2\. Penguins are birds.  
3\. Penguins do not fly.

In many logic programming languages neither of these can be written as-is. The relationship between the exception and the default needs to be made explicit in both places. To illustrate, you might be forced to write this:
```prolog
flies(?X) :- bird(?X), \naf exception_to_flies(?X).

bird(?X) :- penguin(?X).

exception_to_flies(?X) :- penguin(?X).  
\neg flies(?X) : penguin(?X).
```
I had to create an exception in rule 3, and then use that exception in rule 1.

But Flora-2’s re-writing mechanism allows you to do it both ways:
```prolog
@{section1}  
flies(?X) :- bird(?X).

@{section2}  
bird(?X) :- penguin(?X).

@{section3}  
\neg flies(?X) :- penguin(?X).  
\overrides(section3,section1).
```
… or …
```prolog
@{section1}  
flies(?X) :- bird(?X).  
\overrides(section3,section1).

@{section2}  
bird(?X) :- penguin(?X).

@{section3}  
\neg flies(?X) :- penguin(?X).
```
This ensures that in either case, there is a one-to-one relationship between sections of code, and sections of the rules. That makes the code easier to write, because you don’t have to go back and change your encoding of rule 1 when you find an exception in rule 3. It makes it easier to read, and compare to the source material, because all the same things are being said in the same places. And makes it easier to update when the rules change, because in either case, if section 1 changes, you only need to rewrite the code for section1.

#### Third, Law Has Its Own Argumentation Theories

Law has its own argumentation theories, with different rules for figuring out which rule wins in a conflict. One rule is that a newer rule beats an older rule. (_Lex Posterior_). Another is that a more specific rule beats a more general rule (_Lex Specialis_). Another is that a rule issued by a higher authority defeats a rule issued by a lower authority.

Information about how specific a rule is already exists in the code. And information about who issued the rule, and when, can be added as meta-data when you tag the rules. So it may be possible to teach tools that use this style of defeasibility what the argumentation theory of law is in your jurisdiction, and then give it information about when rules were enacted, and by whom, and it will be able to figure out for itself, whether any of them override each other. How reliable that would be is up for experimentation and discovery, but it’s a promising option.