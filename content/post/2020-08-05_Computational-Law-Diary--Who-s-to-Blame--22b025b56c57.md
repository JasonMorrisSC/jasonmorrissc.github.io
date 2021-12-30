---
toc: true
title: 'Who’s to Blame?'
description: >-
  I’m re-reading a 2011 dissertation by Tom Hvitved entitled “Contract
  Formalization and Modular Implementation of Domain-Specific…
date: '2020-08-05T18:50:27.198Z'
categories: []
keywords: []
slug: /@roundtablelaw/computational-law-diary-whos-to-blame-22b025b56c57
---

I’m re-reading a 2011 dissertation by [Tom Hvitved](https://scholar.google.com/citations?user=v8xfBYkAAAAJ&hl=en) entitled “Contract Formalization and Modular Implementation of Domain-Specific Languages”.

Hvitved proposes a set of requirements for contract formalisms. Basically, it’s a list of capabilities a “way of describing a contract to a computer” should enable. Among his proposed requirements is a record of who is to blame if something goes wrong. As he writes:

> In the case where a contract is breached, the monitor should not only report a breach of contract, but also who among the contract participants is responsible. If a contract does not uniquely determine who is to be blamed when contract execution fails, then contract parties may have to go to court to solve the dispute-this is exactly the situation we want to avoid with formalised contracts.

I’ve been coming back to this paper for a few years, and this approach has always struck me as a little strange.

The motivation is pure. Avoiding the need to go to court to solve disputes is valuable. But “the software doesn’t know whose problem this is to solve, therefore the parties are in a dispute that cannot be resolved without recourse to the courts” doesn’t track for me.

People don’t go to court because there is a problem and they don’t know who is at fault. People typically go to court because there is a problem and they disagree over who is responsible for solving it. So I’m trying to understand how Hvitved has situated recording blame in such a way as to make it less likely that there are disagreements in that regard.

Now don’t get me wrong, the idea of blame has some theoretical appeal. I think for it to be really useful you would need to expand it away from “blame” (which has connotations of moral failing), and talk about expectations, the ability to create and modify legally binding expectations, and consequences for violations of them.

For instance, there is an expectation that I not jaywalk. If there is evidence that I have jaywalked, there is an expectation that a police officer is authorized to issue me a ticket. The ticket creates an expectation that I pay a fine. If I do not pay the fine, I may be subject to arrest. If I am subject to arrest, and a police officer finds me, they can arrest me. If they do not arrest me, nothing happens, because the expectation a permission, not an obligation. If I am arrested, there are expectations about how long until I am brought before a judge, and what will happen if those expectations are not met. There is an expectation the judge will be impartial, and expectations about what will happen if the judge isn’t. And there is a complicated system of who is allowed to author these expectations, and consequences, and in which areas, and how.

So the idea that violations of expectations give rise to different obligations, permissions, and prohibitions makes a lot of sense from a theoretical standpoint. There is a lot that you can model that way.

But what I don’t understand is how “blame” in Hvitved’s litany is different from the other methods of setting out expectations, like “conditional commitments”, “reparative statements”, and others.

Throughout the survey Hvitved notes that there is a difference between those formalisms that represent obligations as having contrary to duty obligations upon their failure, and those that represent the contrary to duty obligations as choices.

In practical terms, this is the “I make enough money to be able to afford the tickets” attitude toward speeding. It’s just one of the available options, obey speed limits, or pay for speeding tickets.

Some of the models have different mechanisms for encoding choices than they do for encoding contrary to duty obligations, or reparation requirements.

Then Hvitved gets to the point of explaining his approach to contract “breaches” and blame. He suggests that it is tempting to model reparation clauses as contrary-to-duty obligations, but that this involves an “implicit agreement that the _primary_ obligation … should be complied with first and foremost.” He goes on:

> A classical example that illustrates the subtle, but important, difference is the  
> “gentle murderer”: do not kill, but if you kill, kill gently. The gentle murderer is an actual contrary-to-duty obligation, because there is an implicit agreement that you should not kill-only if you have no other options than killing, then at least you should do so gently. _(Citations removed)_

I don’t see where the obligation to first not kill is “implicit”. It’s right there in the text. And if you’re modelling murdering gently as a contrary-to-duty obligation, then you are being explicit that it is subordinate to some other primary obligation. So not killing is explicitly primary. What is implicit?

He then proceeds to say “We argue, however, that contracts should not contain implicit agreements…” But that’s silly. Implicitness is the entire point of contracts. Effectively, a contract is a promise with the desired implication that if the promise is broken consequences for that breach will be determined and imposed upon you by the Courts. If an electronic contract does not have that component included, it is not a contract at all. And we don’t require contracts to say it explicitly.

Contrary to duty obligations are not implicit if you model them explicitly and give them a semantics. And even if they were, implicitness is not some sort of evil to be avoided.

Hvitved suggests that in his language, motivation should be created with incentives (as opposed to, I presume, moral imperatives). The gentle murderer contrary-to-duty obligation should be modeled as a choice of “either do not murder, or murder gently and go to jail”. And only failing to murder gently, or failing to go to jail, would be “breaches”.

So really, Hvitved’s formalism is modelling breach of contract as generating an unhandled exception. Which means that he is effectively adopting the “I can afford the ticket” theory of consequences, and placing the onus on the contract drafters to explicitly include the motivational consequences to make not speeding worth it.

Which suggests that perhaps the ability to create conditional requirements is all you need in order to be able to deal with blame assignment. But perhaps not. Because conditional requirements only deal with blame the contract knows how to remedy. Perhaps there is something in the way Hvitved is using blame in the context of a breach that makes it important for blame to be a separate capability.

The very next paragraph in Hvitved’s dissertation sets out that the language does not model cases of ambiguous blame. The example is given of two people, Bob and Alice, who are obliged to do some thing by the same given time, but the obligation is that at least one of them do it. If neither do the thing, is Bob to blame, is Alice to blame, is no one to blame, or are they both to blame?

The answer that Hvitved offers is “there are no good options here, so we’re just not going to let you “say” this in our formalism. He says this decision is motivated by how unlikely disjunctive contracts are to appear in the real world.

I’m not so sure. It seems to me that if you enter into a contract with a partnership, you have a disjunctive contract. A partnership is just multiple people. There is no other legal entity there. But you can enter into an agreement with a partnership, and if you do, at least one of the partners needs to meet the obligations placed on the partnership.

So in that sense, disjunctive contractual obligations are quite common.

And how do the courts deal with this problem? You may have heard the phrase “joint and several” before. If you are jointly and severally liable for something with a partner, that means a person who suffers an injury can go after you, or your partner, as they please. It’s up to them. If you want to get into an argument with your partner about which one of you is _really_ liable for the injury, you can. But the injured party can sue either of you.

In the context of breach, it’s for the courts to decide whether or not the party sued is _one of the parties_ liable for the breach. Not whether they are the only one, or the right one.

Hvitved later specifies that blame for the breach of the contract will be assigned on the basis of most-recent failure, if there is one, because they should have done what was possible to remedy the breach. This is problematic in a bunch of ways. First, there is no guarantee that a remedy was possible, so the reasoning is sort of broken. Second, the most recent breach may not be the most salient. Third, different breaches may be more or less excusable in the circumstances. And lastly, it’s still not clear to me what, exactly, the human beings are going to _do_ with the blame information once the software has stopped running.

From the perspective of a lawyer, I want to assign blame in the way that is going to get my client the best possible result at court. I don’t want to have agreed to the assignment of blame in advance, unless there was some sort of benefit from doing so, and it’s hard to see what that benefit would be.

But there may be another reason for this blame mechanism. Hvitved defines his language formally, providing a specific semantics for it. In the semantics of this language, the conjunction or disjunction of two contracts is also a contract. This means that if you have a contract that obliges Bob to do X, and a different contract that obliges Alice to do Y, and you tell the software “there is a contract that is either the Bob contract or the Alice contract”, that will work.

The reason for this is that it is useful to be able to think about the contract that you signed with your subcontractor as a part of the contract in which you were obliged to do a thing, so that they can be attached together, and you can analyze them as if they were a unit. Perhaps the subcontractor contract has a start date in it that is after the deadline for a relevant requirement in the main contract. If you can treat both contracts as one, and analyse it at the same time, you can detect problems like that.

So an important feature of this language may be that each contract fits, like a LEGO piece, into the contracts above it or below it in the same way. Which is why they need to be able to report what went wrong. Not because a person is going to know what to do with that information, but because there may be a remedy set out in a larger formalized contract of which this formalized contract is a smaller part.

That explains why a contract should report blame on a breach. But it doesn’t explain why we would use most-recent breach to determine where the blame lied. It seems that it would be more useful to provide the total list of unmet obligations, when they arose, when they were violated (all obligations have a deadline in Hvitved’s language), and who held them. That would be more useful information to have for both the code and the person. And it doesn’t require blame as a separate mechanism.

Knowing what went wrong, and who did not meet the expectations set on them by the contract is important. Characterizing that information as “blame” seems needless, and perhaps counter-productive. Deciding that the party to blame is the party with the most-recent breach is arbitrary, and will motivate people to draft contracts in which the other party always has a remedy, insulating themselves for responsibility for their own breaches.

I’m just not persuaded that assignment of blame is a necessary or even desirable feature for a computational law language. It doesn’t seem to me to correspond with how rules are used in the real world. From my perspective, if there is a way to represent obligations (or prohibitions) and whether they have been fulfilled (or violated), that is enough.

### Coming Up…

*   My [MIT’s Computational Law Report](https://law.mit.edu/) paper on Rules as Code and Blawx will be out really soon. It turns out I have one more feature to add to Blawx to make the user experience of the paper everything it can be, so that’s on the agenda for this week.
*   A piece I wrote for [Slaw.ca](https://www.slaw.ca/) about the SMU Centre for Computational Law will be coming out in late August.
*   I will also be participating in a Rules as Code panel on the first day (September 10) of the 2020 Legislative Drafting conference held (virtually) by the Canadian Institute for the Administration of Justice. Check [their website](https://ciaj-icaj.ca/en/upcoming-programs/2020-legislative-drafting/) for registration details. There are sponsored tickets available for students, I understand.

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._