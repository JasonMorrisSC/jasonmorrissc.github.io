---
toc: true
title: Legal Drafting to Avoid Computational Complexity
description: >-
  I was working on an s(CASP) encoding last week, and learned something that
  gave me a better intuition for how to encode things well.
date: '2021-03-24T07:11:46.633Z'
categories: []
keywords: [scasp]
slug: /@roundtablelaw/legal-drafting-to-avoid-computational-complexity-115a55818493
---

I was working on an s(CASP) encoding last week, and learned something that gave me a better intuition for how to encode things well.

I needed to encode what counted as a business, and there were two things. I needed to encode what counted as a business entity, and there were 7 things, each of which required a business, which meant there were 14 possibilities total. Then I needed to encode what counted as an executive appointment, and there were three. One of them required a business, which meant that branch had two options. One of them required a business entity, which meant that branch had 14 possibilities. And the third required a law practice, which meant it had one, for a total of 17 possibilities.

Later, a rule applied to “executive appointments in a business entity.” So I wrote that code to check to see whether the first thing was an executive appointment, whether the other thing was a business entity, and whether the first thing was “in” the second thing.

There are 14 ways for something to be a business entity, and 17 ways for it to be an executive appointment, giving 238 possible ways that it could be an executive appointment in a business entity. That question was a degree of magnitude slower than the others to answer.

I want to point out how fast this happened. There are only four definitions: business, business entity, executive appointment, and the rule. With those four definitions there are 238 possible paths to concluding that the rule is true. That is an example of what is called “combinatorial explosion.” Combinatorial explosion is bad, because it means that how quickly your code can answer questions grows exponentially with how complicated the rules are. It means that your technology does not scale with complexity, and law is nothing if not complex.

This combinatorial explosion, though, was completely unnecessary. Because 14 of the 17 ways of being an executive appointment are by virtue of the different ways of being a business entity. So by using a lower-level predicate, I was forcing my code to consider 14² ways in which a business entity was a business entity!

The solution to that problem was to add three concepts to the encoding that didn’t exist in the law: “executive appointment associated with a business”, “executive appointment in a business entity”, and “executive appointment in a legal practice.” Then, an “executive appointment” was re-defined as any of those three things. And in my rule, I was able to refer only to “executive appointment in a business entity”, and reduce the number of things to check from 238 to 14.

#### Caveat

Whether or not you actually have this unnecessary combinatorial explosion problem while trying to encode legal texts is going to depend significantly on the tool you are using to encode them. Tabled logic languages, for example, notice when they are trying to find a question to an answer that they have already found before, and use the previous answer as a short-cut. So there are technological solutions to unnecessary combinatorial explosion.

### The Question

Let’s imagine for a second that’s not the case, and this is a sort of unavoidable problem. Or, it is a problem in the technology that you have chosen to use to encode your legal rules. But for some reason, it’s a problem that applies to you.

> Rules as Code holds up the idea that things that make the code better might also make the law better. Here’s my question: is this one of those things? Does the change that reduces the computational complexity of a rule also benefit the legal text for the same or other reasons?

There are two parsings of the natural language statement “executive appointment in a business entity”. One is the statement that I encoded originally:
```prolog
in(X,Y), executive_appointment(X), business_entity(Y).
```
What that means is the first thing is an executive appointment, for any reason, the second thing is a business entity, for any reason, and the first thing is “in” the second thing.

The other possible parsing, that I adopted later, was:
```prolog
executive_appointment_in_a_business_entity(X).
```
Here, “executive appointment in a business entity” is a different and more specific category of “executive appointment”. The thing is an “executive appointment in a business entity” because it is in a business entity, and it is an “executive appointment” because it is an executive appointment in a business entity.

The corresponding change to the text of the law that would have facilitated this would be to take the law that reads like this:
```
(b)   a legal practitioner must not accept any executive appointment in a business entity.  
...  
“executive appointment” means a position associated with a business, or in a business entity or Singapore law practice...
```
and change it to something like this:
```
(b)   a legal practitioner must not accept any executive appointment set out in section 10(b).  
...  
10 "executive appointment" means:  
(a)   associated with a business,  
(b)  in a business entity, or  
(c)  in a Singapore law practice
```
### The Answer: Probably Not?

> Tentatively, I think the answer is “no.”

Here’s why.

#### It Doesn’t Break Isomorphism

First, one of my primary concerns with encodings is whether they maintain isomorphism, by which I mean whether one section of the law corresponds to one section of the code.

What that means in this context is complicated. First, if you can re-write the code without breaking isomorphism, that is a reason to think that the re-write is acceptable without changing the law. If you cannot re-write the code without breaking isomorphism, that is either a reason not to re-write the code, or a reason that the natural language version might also need to change.

In this case, the first encoding maintains isomorphism, and so does the second encoding. The definition of executive appointment in the second version now has four defined terms instead of just one, but those four all relate to only that definition, and the definition relates only to those four. So isomorphism is still maintained.

So the loss of isomorphism is not a motivation to consider changing the text of the law.

#### There is No Corresponding Benefit to Human Understanding

Another reason you might want to change the text of the law is if the improvement offered by the change to the encoding can also be offered to readers of the natural language law.

So there, the question in my mind is this: “Is it possible that someone might mistakenly believe that this section of law applies to an executive appointment in a business entity where the reason it is an executive appointment is not because of the business entity it is in?”

The answer is “no”, because even if something can be proven to be an executive appointment in a business entity the long way, it is necessarily also an executive appointment in a business entity the short way. So the problem that you have in the code is a computational efficiency problem that does not correlate into any sort of misunderstanding or slow-down in the way humans read the rule.

Human brains are exceedingly well optimized for speed. Often at the expense of accuracy, but still.

#### The Benefit to Explanations Is Not Worth It

Another reason you might want to change the text of the law is to make it so that the steps in the automated justifications match the parts of the law. This is difficult to imagine abstractly, so let me give a concrete example. If we asked the same question using both encodings, and got the answer that CEO is an executive appointment at Apple, the justification using the first encoding would read something like this:
```
ceo is an executive appointment, because  
  ceo is in Apple,  
  Apple is a business entity, because  
    Apple is a corporation,  
  ceo is a position,  
  ...
```
So the explanation has sub-parts for the definition of executive appointment, and sub-parts for the definition of business entity, which corresponds to the fact there are rules defining those two terms.

If we use the new version of the encoding, the explanation will initially look like this:
```
ceo is an executive appointment, because  
  ceo is an executive appointment in a business entity, because  
    ceo is in Apple,  
    Apple is a business entity, because  
      Apple is a corporation,  
    ceo is a position,  
...
```
Now, we have an extra “because” with regard to the thing in our code that does not have a correlation in the written law, the definition of executive appointment in a business entity.

That’s good for debugging purposes, but potentially confusing for the purpose of explaining to end users how the legal conclusion is justified, particularly if it happens a lot, because it is inconsistent with the original law. If it cost us nothing to make the change to the legal text, and if there was no other better way to solve that problem, it might make sense to use as a justification for making the amendment above.

But neither of those things are true.

#### Cross-References are Costly to Understanding

First, the amended version of the text of the rule is clearly harder to read. Cross-references like `set out in section 10(b)` that are not required in order to avoid misinterpretation are not a good idea. They make it take longer for a human being to understand the law. So in that sense, there is a cost to making the change that might not be worth the benefit.

#### There are Other Better Solutions

On the other hand, there are two other ways of solving the explanation problem. One is to have your rules refer to the subsections of rules that they implement: so the explanation can include the extra explanation, and refer to a highlighted portion of a single specific rule to justify its existence. Or, you might be able to reasonably exclude it entirely, which is a feature of other tools I have seen in this space.

I think either of those approaches is probably better than using a cross-reference in the text.

#### Dividing Out Disjunctions Referred To Individually Might be Smart

Adding a cross-reference to the text of the rule seems like a bad idea. But it might still be a good idea to make the proposed change to the definition of executive appointment. The definition is disjunctive, and those disjunctions are referred to separately elsewhere in the law. You can divide them out in section 10, but then instead of using the cross reference, just continue to refer to “executive appointments in a business entity”, which is an implicit cross reference to the sub-part of the defined term. Then your explanation can point to section 10 when it explains “executive appointment”, and section 10(b) when it explains “executive appointment in a business”. The explanations are more consistent with how the law is written, and the disjunction has not made the definition harder to read.

### What About Computational Complexity Generally?

This particular computational complexity was avoidable without requiring significant changes to the legal text. I wonder if there are situations where there is an unnecessary computational complexity that cannot be resolved without changes to the text, and whether we ought, as a principle of legislative drafting, attempt to avoid such constructions because of the computational complexity created.

I think the first answer is probably “yes.” It seems very likely to me that laws have been written in ways that are easy for humans to understand and difficult for computers to compute. I’m not aware of any examples, yet, but I’ll keep my eyes open.

Assuming there are these sorts of unnecessary combinatorial explosions in the law, does avoiding them seem like a good reason to change the text of natural language laws?

Here, I’m part skeptical, and part uncertain. I’m skeptical, because the existence of the combinatorial explosion problem depends, in part, on the technology you are using. Very similar encodings may explode using one technology and not using another. And I don’t think that we should be changing the text of natural language laws to fit arbitrary technologies.

I’m uncertain, because I do not know what effect avoiding combinatorial explosion in legislative drafting would have. I don’t know what the effect on the style of the writing would be, and whether the effect of adopting that style would make the natural language versions easier or harder to read.

The more general idea, however, that there are unnecessarily computationally-expensive ways of expressing ideas in laws, and low-cost ways of resolving those problems (like dividing disjunctions into separate sections), is very interesting. I think as the tools for doing encodings get better, we are going to be able to get more experience doing encodings and finding those kinds of problems, and how to avoid them.