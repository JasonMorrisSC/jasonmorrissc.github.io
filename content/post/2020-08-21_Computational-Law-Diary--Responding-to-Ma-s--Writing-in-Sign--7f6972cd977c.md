---
toc: true
title: 'Responding to Ma’s “Writing in Sign”'
description: >-
  Last week a paper I wrote for the MIT Computational Law Report about using
  Blawx for Rules as Code was published. Please, please check it…
date: '2020-08-21T18:06:09.071Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/computational-law-diary-responding-to-mas-writing-in-sign-7f6972cd977c
---

Last week a paper I wrote for the MIT Computational Law Report about [using Blawx for Rules as Code](https://law.mit.edu/pub/blawxrulesascodedemonstration) was published. Please, please check it out and let me know what you think. Your feedback would be extremely valuable.

I had been hoping that the piece would start a conversation, but little did I know there was another paper published in the same release that was also talking about [Blawx](https://www.blawx.com). The conversation had started already!

Megan Ma’s paper is entitled “[Writing in Sign](https://law.mit.edu/pub/writinginsign/release/1): Code as the Next contract Language? ✍️ ⏭ 💻.” She is a PhD student and lecturer at Sciences Po. I have to commend her on her description of Blawx, which apart from a couple of technical niceties, was very accurate. Unfortunately, she analyses the ability of both Blawx and Ergo to encode contract clauses on the basis of code samples that _aren’t even trying_ to encode contract clauses. In the case of Ergo, the actual encoding of the clause is hidden by an ellipses, and in the case of Blawx it was just left out.

Honestly, that problem doesn’t really cause problems with the rest of the paper. Her critique would be as well founded had she been looking at the right code. But I think her critique sits on a couple of assumptions that won’t be true in most cases.

### The Contract Doesn’t Cease To Exist Because You Encode It

A number of times in her paper Ms. Ma asserts that these encodings “reduce” contracts to the elements that the person writing the code has chosen to model. That is only true when the contract they are modelling ceases to exist after the modelling is done. Otherwise, the non-reduced parts continue to exist, as much as they ever did.

Ms. Ma’s critique about reducing the contract therefore only applies to those scenarios in which there is no natural language version. Which, with the notable exception of things like distributed autonomous organizations, or extremely formulaic automated trading systems, is rare. I think her arguments are based on that assumption, but I don’t think that assumption will hold in most cases.

### Hard to Read does not mean Hard to Understand

Ms. Ma points out a number of ways in which representing legislation or contractual clauses in code posed difficulties of interpretation. To my mind, these difficulties of interpretation are not criticisms of the programming languages so much as they are examples of the problems inherent in writing contracts in natural language.

But they do illustrate that what needs to be said in the encoding of a smart contract is different from what you typically say in the natural language version. And her paper expresses concern about what those differences in the language would do to the understanding of the contract. But she seems to presume that the smart contract will be understood by reading the code.

No one reads contracts now. Why would we start when the contracts are written in code? ;)

But more seriously, there is an assumption that we can only understand what a contract says by reading it. When it comes to smart contracts, that assumption is false. Smart contracts have both content, _and behaviour_. And you can use their behaviour to understand their content.

If I have a smart contract, and I want to know what happens when I deliver my widgets to the purchaser three days late, I can just ask. Generating an application that is able to take hypothetical fact scenarios and give legal outcomes for those scenarios is one of the benefits that comes from encoding contracts. It comes for free. And asking the computer “who is obliged to do what” and having the computer tell you, in a reliably way, is more likely to promote understanding than injure it.

People can learn the meaning of the content of a smart contract in the same way that they understand the meaning of the source code for an application like their web browser. They do not need to read the code in order to understand how it will behave when they press the back button. They can learn by experience, and testing.

Concerns about the legal theory underpinning contractual interpretation are important when you get to court in a dispute over what the contract means. And there are aspects of the common law of contracts that will need to grapple with how we assess the “intent of the parties” in the presence of an agreed text written in a strict semantics, with the advance opportunity to test it. The courts will need to decide how to deal with evidence of the intent of the parties that is contrary to the code that was written.

But when the parties both have access to an app that gives the right answers, and when they had the opportunity to help build that app and test it before the contract was signed, those sorts of disputes are going to be far less frequent.

### The Importance of Interdisciplinarity

I think what Ms. Ma’s paper demonstrates, is the importance of an interdisciplinary approach to the question of encoded legal rules. There are real issues to consider, here, and probably good responses to them. But if the people who are studying the legal risks of encoding contracts cannot read the code, and if the people writing the code don’t understand the purpose and effect of the artifacts they are trying to digitize, we will be [debating shadows](https://en.wikipedia.org/wiki/Blind_men_and_an_elephant).

Ms. Ma is an example of someone doing the right thing. She reached out to chat with me some time ago, and I was happy to agree. Our conversation improved my understanding of her paper, and this post, dramatically. I’m grateful for it. Starting conversations like that is the right approach. And to be honest, it’s one I’m not particularly good at following.

If you are someone who is writing about smart contracts, rules as code, expert systems, and other forms of automation of legal reasoning, please get in touch and let me know. I’m always happy to chat, even if I’m better at receiving invitations than sending them. Especially if you are a beginner, and you’re not sure where to get started. That’s part of my job description at the Centre for Computational Law at Singapore Management University, and I can’t think of a way I would rather spend my time.

### Quick Notes…

*   My [interactive demonstration of Rules as Code and Blawx](https://law.mit.edu) is live. Please check it out and give me your feedback.
*   A piece I wrote for [Slaw.ca](https://www.slaw.ca/) about the SMU Centre for Computational Law will be coming out in late August.
*   I will also be participating in a Rules as Code panel on the first day (September 10) of the 2020 Legislative Drafting conference held (virtually) by the Canadian Institute for the Administration of Justice. Check [their website](https://ciaj-icaj.ca/en/upcoming-programs/2020-legislative-drafting/) for registration details. There are sponsored tickets available for students, I understand.

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._