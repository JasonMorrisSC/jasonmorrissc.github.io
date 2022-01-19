---
title: "The Bad News"
date: 2022-01-18T19:07:55-07:00
draft: true
toc: true
keywords: ['ProLaLa 2022']
---

In my [post about ProLaLa](/post/prolala) I talked briefly about some research done by the Human Computer Interaction Lab at the University of Michigan, studying what creates legal errors in the creation of legal automation tools.

The experiment seems well-designed, and the results are preliminary, but they are already stark.

* The vast majority of the resulting tools had legal errors
* Even teams that had specifically discussed a problem and how to avoid it ended up failing to avoid those problems
* Most law students suggested test cases for the tools, but never suggested enough to cover the scope of possible problems.
* Using interdisciplinary teams increased the quality of approaches to vague terminology
* The participants judged their tools as being far better in legal quality than they actually were
* Law students were unanimously opposed to automating decision making using their own tools.
* Computer scientists were 75% in favour of automating decision making on the basis of their own tools.

## A Story

Imagine, if you will, a lawyer named Jill. Jill needs to create a contract for a client. But the client says they need the contract in Klingon. Jill doesn't speak Klingon.

Jill agrees to work with a translator to draft a contract for the client. Jill drafts the contract in her own native language, and then sets a meeting with the translator. The translator asks Jill some clarifying questions about a few of the terms, and creates a translation of the contract into Klingon. The client reads the contract over, and is satisfied. They execute the contract with a counterparty.  The party promptly sues for breach of contract.

Jill's client discovers that the contract that they signed in Klingon was missing a standard disclaimer of liability because of a mistranslation.

### What Went Wrong?

In this story there is no one person who knows all of these things:

1. What contractual terms will protect Jill's client?
1. How are those terms expressed in Jill's first langauge?
1. How are the words of the contract translated into Klingon?
1. What legal meaning do the Klingon words have?

Jill knows the answer to 1 and 2. The translator knows the answer to 2 and 3. The client doesn't have an answer for any of the questions. No one has an answer to 4.

Jill can't verify the quality of the Klingon contract, because she doesn't read Klingon, and doesn't know what the Klingon words mean. The translator can't verify that the translation has the required legal intent, because they don't know what the required legal intent is. The client can't verify the intent meets their requirements, because they don't know what is required of the contract, or what the legal meaning of the Klingon text is.

This process has no hope of success. It is an obvious recipe for disaster.

## Legal Apps

Now imagine that the target langauge is JavaScript, not Klingon, the translator is a programmer, and the Klingon contract is an app. The same disconnects apply.

1. What are the rules?
2. How do those rules apply to the task that the application is trying to accomplish?
3. How do we express the relationship between the rules and the task in code?
4. Does the application adhere to the legal effect intended?

What are all the ways that this process can go sideways?

1. The lawyer can misunderstand the client's intent for the app.
2. The lawyer can misinterpret the law.
3. The lawyer can misunderstand the application of the law to the client's problem.
4. The lawyer can fail to communicate effectively with the programmer how the law applies to the client's problem.
5. The programmer can misunderstand the client's intent for the app.
6. The programmer can fail to comprehend the lawyer's advice about how the law applies to the client's problem.
7. The programmer can fail to faithfully encode the understanding they have of how the law applies to the scenario.
8. The programmer can write buggy code.

That's a lot of points of failure, and that's not nearly all of them. 

This is a problem that has been identified in the literature about Expert Systems since the 1980s, when it was referred to as the "knowledge aquisition bottleneck." If the programmer knows what they need to put into the code, they can. Getting that knowledge out of the
head of the subject matter expert and into the head of the programmer
is slow, error prone, and expensive.

Why? Because communication between human beings is lossy. We don't actually know whether or not the thing we intended to transmit is the thing that was received.

## Mitigations

There are a bunch of things that we can do to improve upon this.

Some of them are even done, but most aren't. The ones that are done tend to be half-solutions.

User testing is expensive, slow, and covers a small range of the possible inputs. Unless the users are subject matter experts, it won't discover problems in the legal interpretations.

Automated testing tends to check just whether the code does what the programmer meant to tell it to do. It doesn't tell you anything about whether the programmer meant to tell it to do the right thing.

More formal verification methods just aren't used, because the expertise for them is not widely shared, and they transfer the knowledge acquisition problem from the code to the verification model.

## Back to the Study

What the study tells us, in this context, is that these methods are maybe critically flawed.

So how can we eliminate, to the degree possible, the range of things that can go wrong, above?

Problems #1 and #5 are both problems where someone doesn't understand what the client wanted. The best solution to that problem is probably user testing with the client, and fast iteration and frequent feedback. agile development methods will reveal misunderstandings of the client's objectives relatively quickly and clearly, because the client likely does know whether or not the app is doing what they want, when they use it, even if they aren't able to express what that is in advance.

Problems #2 and #3 are endemic to legal practice, whether automated or not. Sometimes lawyers get things wrong. The usual mitigations are professional negligence insurance, and second opinions. For many automation purposes, insurance is not an option. If you have the resources, you can involve more than one legal expert in reviewing the interpretation that is used. But even that requires documenting the interpretation apart from both the law, and the code that implements it, in a way that a different legal subject matter expert can review for accuracy. There is no standardized methodology for doing that.

Problems #4 and #6 are opposite sides of the same coin. A lawyer with a good interpretation of the rules can fail to communicate it to the programmer, or the programmer can fail to comprehend it, or both. This is a really hard problem. Essentially, it is equivalent to the pedagogical question of measuring learning. It's very hard to know when a message has been received. It is very easy to get false positives, depending on how you test.

One way to mitigate this problem is to have the programmer write code in a langauge the lawyer understands, and can use to verify the consistency of that language with their own understanding.

This is the basic insight behind a lot of the methods that are recommended for the interdisciplinary Rules as Code drafting processes. Flowcharts, and sticky notes with lines between them, and these sorts of techniques are all language that increase the specificity of what is being said, and are as accessible to lawyers as they are to the programmers.

We can push this idea further, into stricter and stricter langauges for this purpose.

Another way is to eliminate the gap between the programmer and the legal subject matter expert, by having them be the same person. This requires teaching the law to the programmer, or teaching coding to the lawyer, both of which are non-trivial. But if we had this hybrid breed of what Susskind called "legal engineers", we would need to worry far less about problems #4 and #6.

Problem #7 is very similar. The problem here is that the person who knows the meaning of the rules can't speak the language that was used to express the idea, and so can't verify it after it is written. The solutions to problems #4 and #6 are solutions to this, too.

Another approach here is to separate the code that is used to implement the app from the code that is used to verify whether or not the app adheres to the legal interpretation, and do only the verification code in a language that is understood by at least the legal subject matter expert, and which is usable by the programmer.

Problem #8 is the same problem that comes with all software, and all the usual approaches to solving that problem apply here, too.

## So What Do We Do?

### Keep Researching Legal Software Quality

The first thing to say is that things like this research at Michigan
are intensely important, and we absolutely must continue to fund this
sort of research. Legal application quality assurance is a public
good. These funds need to come from legally-focused funding organizations, and the largest future users of the results of these
studies, which are rule-makers like governments and regulatory bodies.

### Encourage Legal Logic Hygiene in Software Development

Programmers are aware of the concept of separation of concerns. Pieces
of code that are for the same thing should be separate from pieces of
code that are for something else, so that when you have a thing you
want to change, you know where to find it.

This principle needs to be applied to the legal reasoning aspect of
software development. We need to start expecting that front end code
is written in ignorance of the structure of the legal rules, aware only 
of the structure of their API.

This will allow us to develop domain specific languages for the task,
and it will allow us to deploy encodings centrally and re-use them
across multiple different applications.

### Develop Domain-Specific Langauges for Legal Software Tasks

Expressing legal concepts is a different task than writing applications.
Legal knowledge representation would benefit from
having its own languages.

We do not need just one language, we need many. We need languages that
are for doing legal calculations based on isomorphic encodings of rules. We also need languages for writing legal tests for other pieces
of software. We also need languages for proving the behaviour of other
pieces of software, by modelling that software. We also need languages
in which it is possible to encode the calculations and verify the encoding's consistency with a model of legislation.  And we need those
langauges to themselves be verifiable.

### Make these DSLs Accessible to Non-Programmers

Whatever languages we need to develop, we need to abandon the idea that
the right way to express them is by typing text into a text file in a
coding tool.

On top of whatever DSLs are created for law, we need to create interfaces
for using those languages that flattens the learning curve, and lowers
the threshold of entry for getting benefit from using them.

We need to focus on tools, not just languages.

### Open Source Authoritative Encodings

Whatever codings we create in these languages, we need to share them
openly, online, to encourage re-use and get sufficient value from the cost of creating them. Because these encodings are public goods,
they need to be generated by appropriate bodies, such as governments,
law societies, legal research organizations, legal data associations,
etc.

### Open Source the Tools and Technologies

In order to promote confidence in these encodings, not just the code, but the technologies on which those codings are built, need to be open
source. To the extent the tools we are using are not open source, we
need to replace them with open source tools. To the extent that we are
building new tools that don't yet exist, we need to build them as open
source tools. Trustworthiness in code is earned by accountability, and
accountability comes from openness. Openness has challenges, but the
downsides of everything else are worse.

### Teach Legal Quality Assurance

Once we have these tools, and they are easy enough for people to use, we need to teach them to people. 