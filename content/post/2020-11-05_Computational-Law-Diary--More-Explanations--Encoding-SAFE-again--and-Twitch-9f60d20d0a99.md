---
toc: true
title: 'More Explanations, Encoding SAFE again, and Twitch'
description: >-
  If you read my last blog post about generating explanations for Flora-2
  statements, you may recall that the explanation was full of a lot…
date: '2020-11-05T19:01:46.714Z'
categories: []
keywords: [flora2]
slug: >-
  /@roundtablelaw/computational-law-diary-more-explanations-encoding-safe-again-and-twitch-9f60d20d0a99
---

![](/1__Su7Gk6bZYSY5kzHyZZtJdA.png)

If you read my last [blog post about generating explanations for Flora-2 statements](https://roundtablelaw.medium.com/computational-law-diary-progress-on-explanations-2ad8e37e30ad), you may recall that the explanation was full of a lot of symbols that were difficult to read. That problem has been greatly improved, so the explanation for “is Pugsley a sibling of Wednesday” looks like this:
```
Goal: Wednesday has a sibling, Pugsley is satisfied.  
We know this because there is a rule: two people are siblings if all of the parents of the first person are also parents of the second person  
 Subgoal: (Wednesday is a Person and (Pugsley is a Person and (it is not true that ((an object A has a child, Wednesday and an object A is a Person) and (it is not true that an object A has a child,   
Pugsley))))) is not satisfied.

  Subgoal: (Pugsley is a Person and (it is not true that ((an object A has a child, Wednesday and an object A is a Person) and (it is not true that an object A has a child, Pugsley)))) is not satisfied.

   Subgoal: (it is not true that ((an object A has a child, Wednesday and an object A is a Person) and (it is not true that an object A has a child, Pugsley))) is not satisfied.  
     
    Subgoal: ((Gomez has a child, Wednesday and Gomez is a Person) and (it is not true that Gomez has a child, Pugsley)) is not satisfied.  
      
     Subgoal: (it is not true that Gomez has a child, Pugsley) is not satisfied.

      Subgoal: Gomez has a child, Pugsley is satisfied.  
      We know this because it was stated by the user.  
     Subgoal: (Gomez has a child, Wednesday and Gomez is a Person) is satisfied.

      Subgoal: Gomez is a Person is satisfied.  
      We know this because it was stated by the user.  
      Subgoal: Gomez has a child, Wednesday is satisfied.  
      We know this because it was stated by the user.  
   Subgoal: Pugsley is a Person is satisfied.  
   We know this because it was stated by the user.  
  Subgoal: Wednesday is a Person is satisfied.  
  We know this because it was stated by the user.
```
In addition to the problems noted last week, I’m realizing that the tree is very difficult to read. Here’s an example of why.

The main question here is “is it true that Pugsley is a sibling of Wednesday?” The explanation says the answer is yes, because it is “satisfied.” In explaining how it knows that, it describes the rule that it used to come to that conclusion. That rule has a single condition, which is “not satisfied”.

Why, if the only subgoal is not satisfied, is the main goal satisfied? Because the subgoal NOT being satisfied is what the tool was looking for, and that’s what it found. But that information is implicit. The only way you can tell that not being satisfied is what the tool was looking for is because the goal itself is satisfied. Which works well enough when you have each goal with only one subgoal, or where all the goals and subgoals are satisfied. But if you have multiple sub-goals and their parent goal is not satisfied, there is no way to tell which of the sub-goals was the problem.

The data is there, we just need to find a reasonable way of displaying it so that it is legible. It’s surprisingly complicated.

#### SAFE in Flora-2

I have also begun work on implementing SAFE in Flora-2, which has been reasonably successful for the amount of time that I’ve been able to spend on it.

Flora-2 is so much more expressive than the other tools I’ve been playing with recently, that I have had a lot of opportunity to decide “how deeply do I want to model this?” My preference is to go too far than not far enough, so I’ve been defaulting to modeling it in a deeper way if I can see an obvious way of doing it. What that has meant is that I need to implement a bunch of utility code representing things like deontics, events, actions, and temporal relationships. That’s important work that I was going to need to do at some point, but it is slowing things down at the start.

Here’s an example of a section of the SAFE, and the corresponding section of Flora-2 code:

> THIS CERTIFIES THAT in exchange for the payment by [Investor Name] (the “Investor”) of $[_____________] (the “Purchase Amount”) on or about [Date of Safe], [Company Name], a [State of Incorporation] corporation (the “Company”), issues to the Investor the right to certain shares of the Company’s Capital Stock, subject to the terms described below.
```prolog
\#:Obligation[  
    action->\#:Transfer[  
        transferor->?com,  
        transferee->?inv,  
        object->rights_to_certain_shares,  
        quantity->1  
    ]  
]  
 :-  
  ?_safe:Safe[  
    investor->?inv,  
    company->?com,  
    purchase_amount->?pa,  
    date_of_safe->?date],  
  ?_event:Event[  
    action->?ea,  
    date_of_event->?de],  
  ?ea:Payment[  
    payee->?com,  
    payor->?inv,  
    amount->?pa],  
  on_or_about(?date,?de).
```
The pseudo-code for the Flora-2 above might read:

If there is a Safe, and there is an event in which the investor in the Safe paid to the company in the Safe the purchase amount of the Safe, and the payment happened on or about the date of the safe, then the company is obliged to transfer to the investor the rights to certain shares.

#### Take-Aways So Far

There are two things that have immediately been reinforced for me in starting to encode SAFE in Flora-2. The first is the degree of choice that must be exercised by the person doing the knowledge representation for existing rules. For example, does “issuing rights to certain shares” mean anything? What does it mean in a way that the computer can understand? Is “issuing rights” analogous to a transfer of something? Does it create an obligation for the company to take an action, or does it create a fact that must be true before any other obligations take effect? And what does “subject to the terms described below” mean?

All of those are questions that I needed to come up with answers do just in order to choose a first draft representation. There is no way of determining what the right answer to all of those questions is. It depends on what you are trying to accomplish, and even then there is more than one way to get to the same result. Some of them are underspecifications in the contract, holes that I need to fill. Others are just places where I have options and need to choose one.

The second take-away is the corollary to the first. If

1.  there are things that need to be decided by the person doing the encoding because the contract doesn’t make them clear, and
2.  those decisions are not merely modelling choices, but underspecifications that have an effect on the behaviour of the encoding,

then those decision points represent places where encoding the contract in a programming language at the time of drafting, instead of afterward, would have resulted in a better-drafted contract.

And there’s a lot of them.

I’m thinking that it might make sense to track those decisions as I’m making them, and then provide an example re-drafting of the SAFE on the basis of those decisions, to show concretely how encoding the contract might have made it better.

### Twitch Streaming

I’m going to be doing some recording of my coding sessions for the benefit of some of the junior researchers at SMU, as a way of helping them get up to speed on knowledge representation and Flora-2 in particular. It turns out that it is literally only one-click more complicated to also stream those sessions live on Twitch, so I’m going to be doing that, too. If you’re interested in following along, I’m Gauntlet173 on Twitch. Give me a follow. The videos will be recorded and probably made available on YouTube later. I’ll update you here when I know where they will end up.

### Quick Notes

I haven’t heard yet whether the CodeX presentation I did last week is going to be up on the web, but I’ll provide a link if and when I have it. The Cyberjustice Laboratory event has been pushed back to a date TBD, but probably in January 2021.

*   Nov 17: European Commission Rules as Code [Blawx](https://www.blawx.com/) demo in conjunction with the OECD’s “[Government After Shock](https://gov-after-shock.oecd-opsi.org/)” event.
*   Nov 17&18: Guest lecturing for Megan Ma’s course at Sciences Po.
*   Nov 24: Rules as Code session with Canada School of Public Service.
*   TBD (Jan ’21): Cyberjustice Laboratory (University of Montreal) webinar on [Blawx](https://www.blawx.com/).

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._