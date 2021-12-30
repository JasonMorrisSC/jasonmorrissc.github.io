---
toc: true
title: Are Smart Contracts Legal?
description: >-
  I was pointed by a colleague to a paper on SSRN by two law professors
  proposing a method for making smart contracts “legal.”
date: '2021-02-02T02:21:44.129Z'
categories: []
keywords: []
slug: /@roundtablelaw/are-smart-contracts-legal-263c6d118d7
---

I was pointed by a colleague to a paper on SSRN by two law professors proposing a method for making smart contracts “legal.”

What they mean, I take it, is that they want to make sure that smart contracts are enforceable in court the way “normal” contracts are. And the method they propose is a combination of natural language and code.

This is similar to the way [clause.io](https://clause.io) uses blocks of natural language text combined with parallel blocks of code to allow you to generate smart contracts.

I don’t get it.

#### An Example Smart Contract

Let’s say you have a “smart contract” that is completely automated. Let’s say, for the sake of illustration, it sells small pieces of candy. If you provide it with exclusive access to some specific amount of the specified currency, it delivers a piece of candy to you. There is no natural language version of the contract. There was no signing of any paperwork.

Then, the smart contract malfunctions. You provide it with the money, it does not provide you with the candy. Do you have a remedy at court under the laws of contract?

Usually, yes. Unilateral contracts are a type of contract in which it is recognized that the meeting of the minds and the offer are taken care of, and acceptance is the only thing required for the contract to form. The buyer, by sending the money, accepts the contract. At which point it is legally enforceable.

> The issue is not whether or not a legally-enforceable contract _exists_. It clearly does. The issue is whether the court can ascertain what the _terms_ of the contract were.

Was it obliged to provide the candy immediately? Or if delivery is delayed is that not a fundamental breach? Hard to know if no one wrote anything down. But the court has ways of solving that problem, too, because it can take not of typical practise in order to ascertain what was likely in the minds of the parties at this hypothetical meeting of the minds.

So if you go to court and say “the smart contract took my money and didn’t give me a candy”, the court will say “that is a unilateral contract, standard practice is for delivery immediately on payment, that standard practice was breached, the operator of the smart contract must return the money.”

And yes, I have been describing a gum-ball machine. Not a familiar piece of equipment in Singapore, but still, in my mind, a prototypical example of a smart contract.

![](/1__jN6moxQAcaHpiRAKJVKbOw.jpeg)

If the machine is code, the money is a blockchain token, and the candy is some other digital representation of something of value, who cares? The exact same principles should apply.

#### Back to Natural Language

So why are we so worked up about smart contracts having natural language parts?

I think part of it is motivated by lawyers who think that courts are the only way to mitigate risk. That’s patently untrue. They are also an incredibly expensive and uncertain method of mitigating risk.

But the other, to my mind “real” motivation for having natural language associated with smart contracts is communication of shared intent. Both between the parties before the smart contract is implemented, and among the parties and the court should there be some need to have the court resolve a dispute.

The value of that, though, is proportionate to the risk of relevant failures in communication. Not every contract needs to be written down. Not every smart contract needs a natural language version. You don’t need a written contract before you feel comfortable with the risks of using a gumball machine.

And it is important that the failure in communication be “relevant.” If the person thought they were going to get one candy, and got three, that is a miscommunication, but it is not relevant, because there is not going to be any dispute. The person got more than they bargained for.

Which means that you have to take into account the fact that smart contracts, because they are automated, and particularly if they can be tested and formally verified using the techniques we are studying at SMU Centre for Computational Law, are fundamentally less risky than human-executed contracts.

There may be things that it would make sense to have a natural language contract for if you were dealing with a human being, but which no longer are sufficiently risky when you are dealing with a high-quality automation.

#### You Say I Can’t Go To Court, I Say I Don’t Want To

Do smart contracts need to have natural language versions in order to be “legal” or enforceable at court? No.

Can it be helpful to have natural language versions in order to communicate the terms of the contract with the court when something goes wrong? Yes, but with the right technology, smart contracts can be considerably less risky than “normal” contracts, which means the importance of “papering” the terms is lower.

And papering the terms is a way of passing the buck to the courts when the contract goes south. With smart contract technology there is far more that can be done in advance to ensure that in any scenario that matters to you, the contract will result in an acceptable outcome. Either the contract is completed, or reversed.

Then, you just need to be willing to accept the risk that you have failed to consider something relevant in the smart contract, and the risk that the underlying infrastructure fails. If those risks are acceptable to you, you can just take your losses and avoid court entirely. At which point the need for a natural language version of the contract evaporates. Instead of communcating defensively, you can communicate offensively, and make sure that the users of your smart contract understand how it behaves, so their expectations aren’t violated. Then you don’t have to go to court.

Which any good contract lawyer should tell you _is the whole idea_.