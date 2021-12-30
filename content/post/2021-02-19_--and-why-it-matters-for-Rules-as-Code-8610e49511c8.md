---
toc: true
title: … and why it matters for Rules as Code
description: >-
  Yesterday I posted about what justified stable model query-driven constraint
  answer set programming is. Now let me tell you a little bit…
date: '2021-02-19T06:35:28.776Z'
categories: []
keywords: [scasp]
slug: /@roundtablelaw/and-why-it-matters-for-rules-as-code-8610e49511c8
---

Yesterday I posted about what justified stable model query-driven constraint answer set programming is. Now let me tell you a little bit about why I think all of that matters.

### Defeasibility

The higher-order logic features of s(CASP) make it very easy to implement forms of defeasibility the way it is implemented in legal writing, which is that the relationship is noted only in the default, or in the exception, but not in both.

### Abstract Reasoning and Formal Verification

In law we need to be able to ask questions about laws that don’t involve specific fact scenarios. For example, “under these rules, can a person be both a witness and the signer of a will?” We don’t need a specific person in order to answer that question, and we don’t want to have to spend the time that it would take to generate every imaginable scenario and then search through them. s(CASP) allows you to ask those questions in a very straightforward way.

And those questions, and the answers, in the absence of any specific information, are a form of formal verification of the rules as encoded. If it tells you that a thing cannot happen in a ruleset, that has the force of a logical proof of the assertion. That is a level of certainty as to the behaviour of a law or contract that has not previously been readily available.

### Concrete Reasoning

We also need to be able to use laws to answer questions about specific fact scenarios, and the legal conclusions reached in those fact scenarios. s(CASP) maintains the ability to do that, although probably at a speed disadvantage compared to tools that are designed only for that sort of applications.

### Planning

In law, we have facts, rules, and legal conclusions. If you have the facts and the rules, you can derive the legal conclusions. If you have the rules and a legal conclusion, usually a desired future legal state, you can derive the facts that are necessary in order to achieve that objective. s(CASP)’s abduction capabilities give you both kinds of reasoning, with no almost overhead in the code that you have to write. That is exceedingly rare, and incredibly useful in legal applications.

### Explanations

Being able to explain your reasons for your legal conclusions in an automated system is absolutely critical to having those systems be trusted and trust-worthy. s(CASP) gives you the ability, again with almost no coding overhead, to automatically generate explanations for any conclusion that the system can reach.

### Debugging

The task of legislative drafting is very analogous to the task of writing software. Rules as Code allows you to bring the tools of software development to the task of legislative drafting. But defeasible logic programming presents a challenge to the way that testing is usually done in software development. Software developers usually write code that is strongly isolated from the other parts of the code. These parts are called “units”, and they can be tested by themselves, and when you know they work, you can pretty confidently combine them and just worry about whether you combined them properly.

If you are writing rules, and the rules are capable of interfering with one another’s conclusions, you can’t “unit test” the rule. You need to know how it behaves in combination with other rules.

So the kinds of questions that you need to ask in order to debug a ruleset are not “if I give this input, do I get this output.” The kind of questions you need to ask are “if I give this input, do I get this output, and do I get it the right way.”

Because s(CASP) can explain its conclusions, and because it returns not just a good answer, but all good answers, and everything that it believes to be relevant with regard to each good answer, and because it allows you to as questions about your rules in the abstract, it gives the legal knowledge engineer far more information about how conclusions are being reached, and makes it easier to ensure that the code is actually behaving as expected.

### Uncertain Information

In Law, we are frequently dealing with imperfect information. We don’t know all the facts, and the answer that we need is not what will necessarily happen, but how the facts we don’t know might affect it. For example, “if you are found to be in a relationship of interdependence, then you will owe spousal support.” s(CASP), because it is an answer set programming tool, and because of how it deals with the concepts of negation and proof, allows you to reason about things you know, and about things you don’t.

### Cause and Effect Over Time

You do not get very far in the encoding of legal rules before you start to run into issues with regard to the passage of time. Deadlines are the primary example. Legal processes are another, where the specific time is not important, but the order is important. Because s(CASP) has constraint programming features, you can implement abstractions like Event Calculus, which give you a way to describe how things change over time, and allows you to talk about sequences of events without needing to be specific about times.

### Open Source

For use in public Rules as Code applications, open sources technologies are absolutely necessary. s(CASP) meets that requirement, too.

### Conclusion

There are two things that are really remarkable about that list. First, I am not aware of any other tools that check off all those boxes. I am aware of some that I think check off all those boxes except explanations, but that’s as close as I’ve seen, and I don’t have any experience with those tools, so I’m not sure.

Also, my relatively naive understanding of how s(CASP) works compared to those other tools suggests that s(CASP) may be more scalable, in the sense that the processing time for answering a question does not grow as quickly with the complexity of the rules in s(CASP) as it does in those other tools, making it viable for larger problems.

Second, the list of boxes that s(CASP) does not check off is _really_ short. The only thing I don’t see an easy path to solving is ease-of-use. Which is a very challenging problem, but it is a very challenging problem with all of the tools that have this list of capabilities. And that’s the sort of problem that the Centre for Computational Law is supposed to help solve.