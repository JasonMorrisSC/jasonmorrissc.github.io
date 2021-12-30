---
toc: true
title: 'Progress on Explanations'
description: >-
  So this week I have been working on automatically generating explanations for
  answers generated from Ergo Lite queries. I decided on a…
date: '2020-10-27T22:28:18.831Z'
categories: []
keywords: [flora2]
slug: /@roundtablelaw/computational-law-diary-progress-on-explanations-2ad8e37e30ad
---

![](/1__Js2Ojutpj__hpWnft1Mkwpg.png)

So this week I have been working on automatically generating explanations for answers generated from Ergo Lite queries. I decided on a working title for the explanation module of “xf2”, for “eXplainable Flora-2.”

My working repo is [here](https://github.com/Gauntlet173/xf2), if you’re interested in looking at the code.

So far, what we’ve got is explanations generated in a tree structure, using rule descriptions provided by the user, for variable-free queries that were answered using rules, conjunction (and), disjunction (or), negation (not), and user-specified facts.

That’s a sort of low-level list of things that Ergo Lite can do, but it’s a good start to prove the concept.

#### The Addams Family Example

The idea here is that you should be able to take any Ergo Lite code and use XF2 to generate explanations for answers that were based on that code. So for our example code, we will describe the Addams family, and then create a rule for what qualifies as a sibling.

First, we will say that a Person can have a child, which is also a Person.

Person[|child=>Person|].

Then, we will explain that Morticia, Gomez, Wednesday, and Pugsley are people.
```prolog
{Morticia,Gomez,Wednesday,Pugsley}:Person.
```
Then we will explain that Wednesday and Pugsley are children of both Morticia and Gomez.
```prolog
{Morticia,Gomez}[child->{Wednesday,Pugsley}].
```
Now we just need to provide the rule. We are going to add a name and a “description” as meta-data to the rule. The rule itself will say one person is the sibling of another, if they are both people, and all the parents of the first are also parents of the second.
```prolog
@!{Siblings_Share_Parents[  
    description->"two people are siblings if ..."^^\string]}  
?X[sibling->?Y] :-  
  ?X:Person,  
  ?Y:Person,  
  forall(?parents)^(?parents:Person[child->?X] ~~> ?parents[child->?Y]).
```
The only thing that is specific to XF2 in this code is the way the description is attached to the rule. And that is optional.

Now if we ask whether Pugsley is Wednesday’s sibling, we get the answer “yes.”
```
flora2 ?- Wednesday[sibling->Pugsley].

Times (in seconds): elapsed = 0.007; pure CPU = 0.000

Yes
```
So that’s the code that can answer questions. Now we add XF2 to the mix and see if we can get explanations for them.

#### Using XF2

XF2 gives the Ergo Lite user a type of object called an xf2Explanation. To use XF2, you just create an object of that type, and tell it what the query is that you would like explained. The query is called the “goal.”
```prolog
e:xf2Explanation[goal->${Wednesday[sibling->Pugsley]}].
```
Then, you ask the explanation object to display itself, and you get the explanation.
```
flora2 ?- e[display].

Goal: ${Wednesday[sibling->Pugsley][@main](http://twitter.com/main "Twitter profile for @main")} is satisfied.  
There is a rule: two people are siblings if all of the parents of the first person are also parents of the second person  
 Goal: (${Wednesday:Person@main}, (${Pugsley:Person@main}, (\naf ((?A[child->Wednesday][@main](http://twitter.com/main "Twitter profile for @main"), ?A:Person@main), (\naf ?A[child->Pugsley][@main](http://twitter.com/main "Twitter profile for @main")))))) is not satisfied.  
 Conjunction.  
  Goal: (${Pugsley:Person@main}, (\naf ((?A[child->Wednesday][@main](http://twitter.com/main "Twitter profile for @main"), ?A:Person@main), (\naf ?A[child->Pugsley][@main](http://twitter.com/main "Twitter profile for @main"))))) is not satisfied.  
  Conjunction.  
   Goal: (\naf ((?A[child->Wednesday][@main](http://twitter.com/main "Twitter profile for @main"), ?A:Person@main), (\naf ?A[child->Pugsley][@main](http://twitter.com/main "Twitter profile for @main")))) is not satisfied.  
   Negation.  
    Goal: ((${Gomez[child->Wednesday][@main](http://twitter.com/main "Twitter profile for @main")}, ${Gomez:Person@main}), (\naf Gomez[child->Pugsley][@main](http://twitter.com/main "Twitter profile for @main"))) is not satisfied.  
    Conjunction.  
     Goal: (\naf Gomez[child->Pugsley][@main](http://twitter.com/main "Twitter profile for @main")) is not satisfied.  
     Negation.  
      Goal: ${Gomez[child->Pugsley][@main](http://twitter.com/main "Twitter profile for @main")} is satisfied.  
      Answer was provided by user.  
     Goal: (${Gomez[child->Wednesday][@main](http://twitter.com/main "Twitter profile for @main")}, ${Gomez:Person@main}) is satisfied.  
     Conjunction.  
      Goal: ${Gomez:Person@main} is satisfied.  
      Answer was provided by user.  
      Goal: ${Gomez[child->Wednesday][@main](http://twitter.com/main "Twitter profile for @main")} is satisfied.  
      Answer was provided by user.  
   Goal: ${Pugsley:Person@main} is satisfied.  
   Answer was provided by user.  
  Goal: ${Wednesday:Person@main} is satisfied.  
  Answer was provided by user.

Times (in seconds): elapsed = 4.828; pure CPU = 4.265

Yes  - undefined
```
#### Take-aways

It’s only a start, but I’m very pleased with it for a few reasons. First, it is not a printout of a trace. It is a recursive printout of a tree-structured xf2Explanation object. Because of that, it could easily be exported to allow it to be displayed as a collapsible tree explanation in a web interface, for example.

I did a similar explanation interface with my ABA Innovation Fellowship project [docassemble-openlcbr](https://github.com/Gauntlet173/docassemble-openlcbr), which you can [read about here](https://medium.com/@jason_90344/legal-expert-systems-just-got-smarter-e7e12b75e872).

Second, it is taking meta-data that the user provided in their rules and using that to build the explanation. So we know we can improve the output to be as readable as the user would like, and we can include links to source materials for rules to enhance the transparency of the encoding.

Easily generating explanations, with references to authoritative source materials, is just one of the major advantages of using declarative logic programming methods for Rules as Code. And a big one. But telling people that is never as effective as showing it to them, so I’m glad we are on our way to being able to demonstrate it.

#### Next Steps

There’s obviously still a lot to do. We need to add more sorts of statements that can be explained, with a priority on mathematical operators and aggregate functions, probably. We need to clean up the reification and module text that Ergo Lite uses: `${this}@module` should be displayed as `this`. That turns out to be a _little_ complicated because Flora-2 does not come with string replacement libraries.

We need to address the way that `a and b and c` gets explained as if it were `a and (b and c)`. That’s unintuitive. We need to see if there is a way to make the explanation of a universal `forall` statement more intuitive.

And then we need to see if there is a way that we can add meta-data for non-rule statements, so that `Wednesday:Person` can be displayed as `Wednesday is a Person`.

What I will probably do going forward is re-implement our encoding of SAFE in Ergo Lite, and use that encoding as a motivating example to be able to generate explanations for various outcomes.

### Quick Notes

*   Oct 29 (This Thursday): Stanford CodeX lunch webinar on [Blawx](https://www.blawx.com/) (free to join, if you’re interested)
*   Nov 4: Cyberjustice Laboratory (University of Montreal) webinar on [Blawx](https://www.blawx.com/)
*   Nov 17: European Commission Rules as Code [Blawx](https://www.blawx.com/) demo in conjunction with the OECD’s “[Government After Shock](https://gov-after-shock.oecd-opsi.org/)” event.
*   Nov 17&18: Guest lecturing for Megan Ma’s course at Sciences Po.
*   Nov 24: Rules as Code session with Canada School of Public Service

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._