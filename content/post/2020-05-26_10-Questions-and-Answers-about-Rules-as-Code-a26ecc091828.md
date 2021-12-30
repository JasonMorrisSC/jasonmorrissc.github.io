---
toc: true
title: 10 Questions and Answers about Rules as Code
description: >-
  Brainbox.Institute, in a recent Medium post, put forward a test case that they
  had been working on and asked for feedback. This post is my…
date: '2020-05-26T07:20:51.035Z'
categories: []
keywords: []
slug: /@roundtablelaw/10-questions-and-answers-about-rules-as-code-a26ecc091828
---

_[Edit: A previous version of this post incorrectly identified_ [_Brainbox.Institute_](https://brainbox.institute) _as the people doing the research project described below. In fact, only two of the three researchers are associated with Brainbox. Apologies.]_

Tom Barraclough, in [a recent Medium post](https://medium.com/@tom_5050/a-rules-as-code-case-study-from-new-zealand-92c7bbc74ba7), put forward a test case that a team of independent researchers funded by the New Zealand Law Foundation had been working on and asked for feedback. This post is my attempt to contribute to that conversation.

It, and the Twitter conversation that inspired and surrounded it, have revealed a number of questions about Rules as Code. Here’s what the questions are, and what I think the answers are.

### Question 1: Do we Digitize the Old, or Only the New?

There is a debate among advocates for digitizing legislation about whether it should be done at the time of drafting, or it should be done with existing laws.

The “time of drafting” camp argues that you cannot get the full benefits of digitizing rules if they are drafted in a way that lacks the discipline that encoding imposes, and then you later try to make them fit into a digital mold. This group’s answer to the complaint “legislation is hard to translate into code” is “write it in code (also) from the beginning.” The major implications are that the techniques should primarily be used on new legislation, and the effect is not only better and less costly automation, but also better laws. Which is why proponents of this approach tend to prefer the term “Better Rules” over “Rules as Code.”

The “after the fact” camp argues that we should be using these techniques with regard to existing legislation. This group’s answer to the complain “legislation is hard to translate into code” is “yes, but trying will show you exactly where the problems are, and then maybe you can fix bad laws.” The major implications is that there is a lot more work to be done, but the benefits of doing so may be less pronounced with regard to a given law in the short term.

The research project falls squarely into the second camp. They are attempting to see how much of the case study law can be automated, and they are trying to see what advice the process can give them on how the law ought to be changed.

I think we need to be realistic about how Rules as Code can progress. It cannot and should not adopted until it can be shown to be better than the alternative. There are a number of questions that need to be answered. First, if you have a high-quality encoding of a piece of legislation, does that make your life better when you are trying to automate things that need to comply with it. Second, is that high-quality encoding easier to get if you code at the time of drafting.

I suspect that the answer to both of these questions is yes. But more importantly the first question can be answered without doing the encoding at the time of drafting. You can take an existing piece of legislation, encode it, then reverse engineer a piece of legislation that accurately represents the encoding. Then, that piece of legislation can be implemented using rules as code and traditional techniques, and see whether there is a difference.

So even if ultimately Rules as Code is better applied to new legislation, demonstrating that it works will require encoding what we’ve got. And it may also be the case that even though the largest benefits come from encoding from the beginning, the benefits that come from encoding after the fact are significant enough to be worthwhile.

### Question #2: Can “Rules as Code” or parts of it be done alone?

One of the things the experiment is trying to determine is whether or not the Rules as Code process, or significant parts of it, can be done by one person on their own. This, I believe, is a reaction to the interdisciplinary approach of the “Better Rules” camp which suggests that you should have a number of people of different backgrounds, including policy, legal drafting, and coders, in the room at the same time in order to get the desired result.

The investigators express a concern that imposing an interdisciplinary approach expensive, and does not scale efficiently.

I think this misunderstands why the Better Rules approach calls for interdisciplinary approaches to Rules as Code, in two ways.

First, interdisciplinary means that the various perspectives need to be taken into account. It does not mean that those perspectives need to be held by different people. But as it happens, there are very few people with knowledge of policy objectives, implementation details, knowledge representation formalisms, legislation drafting conventions, principles and precedent of statutory interpretation, etc.

It can be done by one person if the one person knows all those things. But usually the one person doesn’t. Nor should this be surprising. In most software creation projects the person who is writing the code is not also the intended user. You need different people with different perspectives to build something useful.

Second, the current phase of the Better Rules approach is to demonstrate that this is an approach that has potential value in implementation, and then use the resulting buy-in to develop the tools and techniques for doing it more efficiently. I don’t think the plan is to be drafting and coding in giant committees for days at a time. Ford’s first car wasn’t built on an assembly line. He had to build one the hard way to know how the assembly line should be built.

So I’m not sure that there was ever any question about whether parts of the process could be done by a single person. They certainly can.

### Question 3: What Do You Need to Know to Do Concept Modelling?

Concept modelling is described as the first part of the Rules as Code analysis process, and the research project is designed to test whether that specific part can be done by a person with legal expertise and some knowledge of how computers work with data, on their own.

Regretfully, I’m not sure this question was properly conceived, or if the experiment gives us a useful answer.

All of the experiments that I have seen done in the Rules as Code space, including those in New Zealand and Canada, have implicitly taken the same position on this question: “Nothing much.” It is as if we expect that if we put the words “concept model” in your head, give you a whiteboard and a marker, and set you loose, we can expect the result to be of consistent and high quality.

What makes a _good_ concept model is not suggested anywhere in the documentation that I’ve seen. Here’s my suggestion: A concept model is a good concept model of legislation if it simplifies the process of digitizing the relevant rules. It will simplify the process of digitizing the relevant rules if it is easy to make, and it has a high similarity to what eventually will need to be encoded. Whether or not it is similar to what eventually needs to be encoded depends on whether the relationships between the concepts in your concept model are similar to relationships between elements of code in the semantics of the language or category of language you intend to use for the encoding. And the complexity of that semantics will also determine whether or not the concept model is easy to make.

Let’s take an extreme example. Let’s say you’re going to encode the rules for Rock Paper Scissors. And your target language is Assembly language, which is basically impossible for any normal human being to use. There is no hope that your concept model will be useful. Human beings do not think in the low-level way that assembly language does, because the semantics of that language is limited to bit-level manipulations of data, and reading and writing to memory addresses.

Now let’s say your target language is C. The concept model will be useful if its parts relate to things you can create in C, like imperative procedures, pointers, functions, inputs, outputs, etc.

What if your target language is Python? The concept model will be useful if what your are describing can be implemented as object-oriented objects, which are combinations of categories, properties, methods, and instances.

If your target language is Flora-2, the concept model will be useful if what you are describing can be implemented as ontological categories and properties, and rules.

What makes for a good concept model depends on the target representation language. A good concept model for C is not a good concept model for Flora-2. And there is no such thing as a good concept model for Assembly.

Can someone create a useful concept model by themselves? Yes. If they know the semantics of the language in which the model is being built, then they can build a concept model that is consistent with those semantics. But if they don’t have a semantics, it will be pure chance whether the concept model serves any purpose at all.

Without meaning to be unduly critical, this failure to specify what makes a good concept model is well demonstrated by the concept model that the process generated. As a person who has used a few different technologies for encoding legislation, the concept model would not be helpful to me in implementing any of them.

Here’s what I think we should be doing at the concept model phase: We should be using ontologies: Classes, objects, properties, and relationships. We should set out the ontology that we need in order to say the things the law says. But this is one of those things where everyone who has their own favourite hammer probably sees the world as a corresponding type of nail.

### Question 4: What, exactly, are we modelling?

At one point, the case study says that the person building the concept model was not trying to encode for policy intent, but for the concepts used in the legislation and the relationships between them.

At another point, the study mentions that there were concerns about whether what was being modeled was the law, or the modeller’s interpretation of the law.

This is an idea that is suggested again and again, and each time it is suggested as somehow less than acceptable that what is encoded would be an interpretation of the law, as opposed to what the law truly means. It strikes me as very silly.

Here’s a thought experiment: Think of a law that you are pretty sure you understand the meaning of. Now, remove human beings from the universe. Does that law still have a significant meaning?

For my part, I’m comfortable that the answer is no. Laws do not have meaning in and of themselves. They are given meaning by interpretation. It is therefore not possible to encode the meaning of the law and not merely an interpretation of it. Every possible meaning of the law, including the correct one if such a thing exists, is an interpretation.

So what we are encoding is interpretations. Full stop.

That might seem like a sort of academic or philosophical argument, but it has real world implications. If you live in a world where laws have real meanings outside of interpretations, then it is possible for an encoding to represent something other than the true meaning of the law, and that might for some reason be bad, and a think you could be persuaded not do.

If you live in a world where laws only have interpretations, the fact that you are encoding one should be no cause for concern.

Furthermore, if you live in a world where the true meaning exists, and the true meaning cannot be represented in the technology you are using, then that can be used to criticize the technology. If you live in a world where the true meaning does not exist, that criticism holds no weight.

And as it happens, a great deal of the legal academic literature surrounding the dangers of encoding law falls into exactly these traps. The law has a true meaning, hitting on that true meaning is unlikely or impossible, and therefore encoding is risky and should be done.

Nonsense.

All that a lawyer can use is their own interpretation, which can easily be wrong, and somehow using a wrong interpretation is a sin if instead of a lawyer it is a computer? Even if the lawyer and the computer would give the exactly same advice in the exact same circumstances?

Nonsense.

### Question 5: Who should do the modelling, and what should it be used for?

If what your modelling is an interpretation of the law, who should be involved in figuring out what interpretation you are going to model? Who decides what interpretation is the right one?

This is one of those places where we need to guard against holding the new thing to a higher standard than the status quo.

Right now, people in government try to figure out what laws mean, when implementing them. If they can’t figure out what the law means, they ask around. If no one can figure out what the law means, they ask for legal advice. And if the legal advice is “it depends,” they figure out who’s going to take responsibility for deciding what the policy is going to be on what the law means, get that person to cover their butts, and then proceed. Because they have to do something. Or, sometimes they don’t notice that there is ambiguity, and they think the law is clear, and they just do what they think it says and no one ever questions it. And sometimes people come to different conclusions, and do completely different things with the same piece of law.

In that story, sometimes the persons involved are writing software, and sometimes they aren’t. It doesn’t change anything. That’s still how it works.

Rules as Code doesn’t need to be better than that, it just needs to be no worse, and be better than that in ways other than whether the interpretation is good.

Also, if you use Rules as Code at the time of drafting, or if you use it to perform analysis of existing legislation, it has the power to make the sources of confusion more apparent, and give you the option to avoid or repair them. So the answer to “who should decide what interpretation gets modeled” is “whoever does it now, because if they are doing it right they are going to end up with a better one anyway.”

### Question 6: How do we make sure the model is high quality?

This question is similar to the last one, in that part of the reason for worrying about who should do it is worrying about the quality of the result. But the question of who also has implications for responsibility and accountability for decision-making.

This question is different. This question is what techniques can or should we use in order to test the quality of our model?

Let’s take the story above of the interpretation problem, and focus on the part of the story where legal advice as to the meaning of a law is sought. Let’s imagine that there is a single lawyer responsible. What are they going to do?

Part of what they are going to do is come up with an interpretation, and then test that interpretation against fact scenarios that might complicate it. They will try to think of scenarios in which their interpretation doesn’t make sense. If they can’t find any, that will give them confidence. How many possibilities will they consider? They might be aware of 10 relevant factors, each of which is binary. Are they going to specifically go through all 1024 different possible fact scenarios? No. Because they don’t have that kind of time.

Now let’s imagine that the rule is encoded. Can you go through 1024 fact scenarios and calculate the results? Easily. Computers are great at that. Can you search those results for something that looks weird? Absolutely. If you know what “weird” would be, you can absolutely search for it.

So the fact of the matter is that using Rules as Code allows you access to much better and more exhaustive tools in order to test whether or not your interpretation matches with your expectations.

And what’s more exciting, you can more easily test for whether or not the interpretation meets with your policy objectives. Again, “better rules.”

Rules as Code also calls for the encoding of the legal logic part of applications to be compartmentalized away from the rest of the application. Which means you can have someone who understands the law lay out 100 tests for what they think ought to happen, and ask the computer whether the encoding of the rules (on its own) matches those expectations automatically, without the person needing to use the application 100 times and reviewing the outputs manually. If the encoding is good, it can power any number of applications that need to comply with those rules.

So the question of how we make sure the rules as code model is of high quality is “much more easily than how we do it now.”

### Question 7: Is there a semantic ambigutity about the use of indefinite and definite articles in legislation?

A considerable amount of time was spent discussing the possibility that in a piece of legislation that uses both the words “A claimant” and “the claimant”, the words may not refer to the same person.

They ask for feedback on whether this is a recognized issue, and I think the answer is “no.” But the more important thing is that it doesn’t matter.

This is a good example of a problem that doesn’t have anything to do with Rules as Code, and another reason that it is important to recognize that what we are encoding is interpretations, not the meaning of the law.

If, in the interpretation of the person creating the model, the words “the claimaint” and “a claimant” in a single sentence refer to the same claimant, then they should be modeled accordingly. If not, they should be modeled accordingly. Just do whatever your best interetation calls for, which is the same thing you would do regardless of whether you are encoding it or not.

This problem, if it is a problem, exists to the exact same extent when the law is applied without the use of computers. This is not a problem with encoding, it is a problem with interpretation.

The truth is that when you start encoding legislation, you immediately begin to realize how much of law is implicit. And all that implicitness needs to be made explicit. And you might do it wrong. But at least now you are thinking clearly about it instead of using unconscious assumptions that might have differed from those of the drafter without your noticing.

### Question 8: Are we building an application, or encoding knowledge?

In the appendix to the report the author notes difficulties deciding how to model sections of the law that don’t matter with regard to the intended application of the project, which is answering the question of whether applications are valid.

Implicitly, the author has taken the position that what needs to be encoded is the parts of the law that are necessary for answering the question. We can call this the “application” approach. There is a competing argument that what you need to do to get the full benefit of Rules as Code is to encode everything that is there, because you don’t know yet at the time of encoding what _other_ application the encoding might be used for later. Encoding the entire act allows the code to be reused in a larger number of possible applications.

This has really significant implications for the technology choice, too. Consider the difference between an imperative language, and a declarative language, and let’s imagine that the only thing that matters for an application to be valid is that the applicant is at least 18 years of age.

In an imperative model, you can encode that in a flowchart-like process in a language like Python.
```python
if applicant.age >= 18:  
  application.valid = True
```
In a declarative model, you would encode that same idea in a formal logic-like rule in a language like Flora-2.
```prolog
?X[valid->\true] :-  
  ?X:Application[applicant->?_[age->?_age]],  
  ?_age >= 18.
```
There doesn’t on their face seem to be a lot of difference between the two. But the semantic difference is huge. Both of them can be used to determine whether or not an application is valid on the basis of the age of the related applicant.

But the second representation can be used to answer a different question like “what matters for determining validity.”

The difference is that imperative encodings typically include the thing you are trying to determine. Declarative encodings don’t require one. The code doesn’t need to “do” anything except represent the meaning. And it will do something once it is given a fact scenario and asked a question. And it will do more than one thing.

If you encode in an imperative way, you may find yourself needing to write new software in order to answer another question. If you do it in a declarative way, the same code will work for a lot more different sorts of applications.

For my money, if we encode applications, we are leaving benefits on the table for no good reason. We ought to be writing code that can answer the same questions the law can. And that means either using declarative languages, or doing a lot of work to make the imperative languages behave like one.

### Question 9: What Kinds of Questions Do We Want the Model to Answer?

A great deal of ink is spilled on whether or not it’s feasible to use these sorts of techniques to answer difficult legal questions.

It might be. It might not be. Whether or not the model can predict what a judge would decide is not the point. The point is whether it can provide a service that otherwise a person would need to provide, or more likely, wouldn’t get provided at all.

These sorts of technologies are not going to be used for predicting the outcome of close cases at court any more than WebMD is used for performing brain surgery. The opportunities are at the other end of the complexity scale.

We want it to answer easy questions to start.

### Question 10: Do we want to create natural language from code, code from natural language, or have people write both?

As of right now, only the last is feasible. Natural language automatically generated from code is going to be either very difficult to read and un-natural, or you are going to have to do a lot of otherwise unnecessary work in the encoding process. It’s not clear to me why the ability of the code to say back what you just said to it in English or any other natural language is a plus.

Generating code from natural language may one day be possible, but not until there is a very large rosetta stone database of existing translations, and for that, we need to go with having people writing both. Maybe the same people, or maybe different people, depending on your answer to Question #1.

### Conclusion

The last couple of days have seen far too much activity on Twitter and elsewhere and I need you all to go slower.

Also, Tom Barraclough, who authored the Medium post above, was exhorting people on Twitter that someone needs to go and actually code the stupid thing.

I might. Actually, I more than likely will. But I need some time.

In the meantime, if you’re interested, you can check out this series of posts where I [played along with Canada’s recent Rules as Code experiments](https://medium.com/@jason_90344/playing-along-with-rules-as-code-6c837b42a33e).