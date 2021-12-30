---
toc: true
title: 'Modelling the World, the Law, and the Question'
description: >-
  I’ve been spending a lot of time recently thinking about the problem of
  choosing how to model things when doing automated legal reasoning.
date: '2021-03-06T07:12:49.556Z'
categories: []
keywords: [scasp]
slug: /@roundtablelaw/modelling-the-world-the-law-and-the-question-e24ea763120e
---

I’ve been spending a lot of time recently thinking about the problem of choosing how to model things when doing automated legal reasoning.

### What is a Model?

Just so we’re on the same page, when I talk about a model, there is a real thing, of some sort, and there is a representation of that thing in a language which is useful for some reason. The representation is the model, and the language is the modelling language.

The modelling language is not the thing itself, so you have to have a connection between the real thing you are modelling, and the terms you use in that language. The more consistent and precise the connections, the higher-fidelity your model will be.

### Laws are a Model

Laws do this. If there is a law that says “no vehicles permitted in the park,” that is a model, in English, of an idea. What the idea is, exactly, can be hard to discern. Does the law mean that you cannot ride a bike? Does the law mean that you cannot display a vehicle in the park? There are different possible real ideas that could be connected to that natural language.

Natural languages are not designed for precision. That’s why statutory and contractual construction are so difficult, and why there are so many techniques associated with modeling legal ideas in natural langauges like defining terms and using them in consistent ways. And that’s also a part of why contracts and statutes seem to be written in such an inhuman way. They are aiming at precision of communication, not speed, and so time needs to be spent making sure that the connections between the words used in the model and the real world things they represent are reasonably clear.

There is always a trade-off between these two pressures. On the one hand, you can spend a lot of time being extremely specific about what you are talking about. On the other hand, the longer a law or a contract gets, the less useful it is. Being able to answer important questions easily and fast by reading the rule is a virtue. So there is a point at which being more precise causes more problems than it solves.

### Computer Programs are a Model

Software written in a programming language is also a model. It’s just a model in a language that is used to communicate with machines instead of people. What is different about writing a computer program from writing a law is that you have to presume, when you start writing the program, that while the computer understands the grammar of the language, it has almost no vocabulary.

What is a program a model of? Typically, it is a model of an algorithm, which is a process for coming to the solution for a specific type of problem. Maybe the specific type of problem is “give me the last letter in this word,” or “add these two numbers together.” But it is a model of an algorithm.

Because computers don’t share a vocabulary with us, we create one. We write models for small concepts like addition, or repeating something a certain number of times. Then, we combine those smaller models to create things like multiplication, which is repeating addition. The things that are going to be useful to most people most of the time get included by default when you use the language, and are called the “standard library.” Everything else that is useful, but not all the time, gets shared as a “library.”

Standard libraries and libraries take the place of shared vocabulary. You don’t need to define them again in each program, you just copy and paste the work that someone else has already done.

### Encoded Laws are a Model, or a Model of a Model, or Both

So when I encode a law in a programming language, that is a model. But what is it a model of?

Let’s take the example of “no vehicles permitted in the park.”

Let’s assume that I use a constraint programming language to attempt to model it. I might decide “in the park” is a thing that can be true or false about an object. And “is a vehicle” is a thing that can be true or false about an object. And because a constraint programming language allows me to describe things that can never be true, I might decide to describe the prohibition as a thing that can’t happen. That might look like this:
```prolog
:- in_the_park(X), is_a_vehicle(X).
```
So what did we model?

The first thing to realize is that there is an unavoidable step that happens first, and that is that I consumed the model that was written in English, and that turned into an idea in my head about what the law means. Before I can model an idea, I need to possess it. So there is no hope of me directly modelling the one true meaning of a law unless I am a king. The only thing available to me was my interpretation.

So I had an interpretation in my head of what “no vehicles permitted in the park” means, and that interpretation is what I modeled in the programming language.

One way of being able to tell what you have actually modeled is by trying to answer “what questions can the computer now answer, based on what I have told it?”

Here, there is only one thing that the computer can do. It can object if I try to describe a scenario that includes a thing that is both a vehicle, and in the park. And the way it will object is to effectively say “that can’t happen.”

So what have I modeled? I have modeled a world in which that law is never broken. I cannot use that model to understand how a rule was broken, because the model does not admit of the possibility of broken rules.

So let’s say that I want to model the idea that a rule can be broken. I can do that in a very low level way by saying “there has been a violation”.
```prolog
violation :- in_the_park(X), is_a_vehicle(X).
```
This is not a constraint, this is a rule. It says that if something (called X) is both in the park and a vehicle, then there is a violation.

What sort of questions can this encoding answer? Well, now I can describe a scenario, and I can ask whether or not there are violations in that scenario. And if there are, I can ask why, and it can tell me that there is a vehicle in the park.

So what have I modeled? I have modeled the idea of “legally impermissible”, which is different from the idea of “impossible.” So instead of modelling a world where rules are never broken, I’m modelling how the rule generates legal conclusions from facts in the world.

If I tell it about a vehicle in a park, it will tell me that there is a violation, and that it is a violation because there is a vehicle in a park. But it will not tell me what the authority is for that rule.

If I want to know that, I can create a new concept in the model.
```prolog
according_to(the_sign,violation) :- in_the_park(X), is_a_vehicle(X).  
violation :- according_to(_,violation).
```
Now if I describe a vehicle in the park, the code can tell me that there is a violation, and that the violation arises from a breach of the requirements on “the sign”.

So what we are modelling now is the law itself, and its consequences.

Now imagine we wanted to be able to model both obligations and prohibitions. So we create words for those in our computer language, and we express the relation as a constraint:
```prolog
:- prohibited(X), obliged(X).
```
Now we have modeled a world in which it is impossible for something to be both prohibited and obliged. That is useful, if what we are trying to do is prevent people from expressing such incoherent ideas.

But what if that’s what the law actually does? Now I have a language in which it is impossible for me to express what the law says, and get an explanation for why the law is broken. If I want to be able to analyse laws themselves, I need to leave open the possibility that the laws are internally inconsistent in important ways, so that I can detect those internal inconsistencies and flag them for the drafter.

Being able to say something incoherent, and detect that you have done so, is very useful when you are drafting a law, or when you are analysing one. Being able to say something incoherent when you are trying to figure out what the law does in a particular circumstance, is less useful. If I want to build a web app that tells you if your object is allowed in the park, it is not useful for you to be able to say both that the object is a vehicle and that the object is not a vehicle.

In the one case, you have a language for modelling a world in which laws are coherent, and in the other case you are modelling actual laws as written.

You are constantly faced with these choices about what you want to model, and why. And things that are useful in one application and less so in others.

### Legal Modelling for User-Facing Applications

This problem then extends to the idea of how you model the real world in a way that will make it convenient to build user-facing applications from your encodings. For example, let’s assume that there is a law that says I am not allowed to build a deck without permission, but I can build a gazebo without permission.
```prolog
according_to(bylaw,permitted(X,build,Y) :- person(X), gazebo(Y).  
according_to(bylaw,prohibited(X,build,Y) :- person(X), deck(Y), not has_permission(X).  
according_to(bylaw,permitted(X,build,Y) :- person(X), deck(Y), has_permission(X).
```
Now let’s imagine that I want to build a user-facing website that knows what to do with this information. Based on this encoding, some of the questions that this tool might ask are “what do you want to build?” And “are you a person?” And “Is the thing you want to build a deck?” And “Is the thing you want to build also a gazebo?” And “are you a gazebo?”

A few of those will seem strange.

In the logical language, what I have said is that `person(X)` can be true, and `gazebo(X)` can be true. I haven’t said anything about whether or not they can be true at the same time about the same thing.

I have also not said anything about whether `gazebo(X)` and `deck(X)` can be true at the same time.

If we are trying to analyze the legal effect of the law, that second fact is probably a strength. Because the software can then detect the possibility that a thing might be both a deck and a gazebo, and that the rules would make the same thing both prohibited and permitted. Unless the bylaw has given a strict definition that distinguishes decks and gazebos, in which case that definition should get encoded, too.

On the front end, it might actually be a good idea to allow the user to say that the thing they want to build is both a deck and a gazebo, and then to detect that it is both permitted and prohibited, and to explain why, and to tell them that under the circumstances they should not rely on the tool’s output, and should go ask.

But there is no scenario I can think of in which it is useful to say that a person can be a gazebo. And that seems weird to you, too. But not to the computer. Why?

Because despite the fact I haven’t been talking about it explicitly, I have been constructing a correlation between the words I’m using in the programming language and the real-world concepts that they represent. And in the real-world concepts that I’m using for “person” and “gazebo” the categories are mutually exclusive.

We get that. The computer just sees a list of letters, and has no real-world understanding of what those things represent. So we can say explicitly that a thing cannot be both a person and a gazebo, or a person and a deck.
```prolog
:- person(X), gazebo(X).  
:- person(X), deck(X).
```
Now when I’m building a user-facing app, it won’t ask that particularly strange kind of question.

A language in which you need to say those sorts of things explicitly can be a little bit frustrating, because you might need to say things like that a lot. But on the other hand, a language that assumes that categories are mutually exclusive, might force you to say things that you weren’t aware you were saying, and that may not necessarily be true, like “a deck is not a gazebo.” That would be frustrating because it would prohibit you from being able to be uncertain about the overlaps between categories.

Let’s go back to our imagined app:

I want the app to ask “what is your name”, and then ask “what do you want to build”, and then ask for the details of each thing that they want to build, and then ask whether they have permission, and then tell them what they can and cannot build and why.

In that model I just described, a “thing you want to build” is an aspect of a person. I might also have said that the primary object is the construction project, and each construction project has a person trying to build it. Either way will work. The rules don’t care. But there is no information in our model that will allow the software to figure out which to use.

It is helpful, therefore, to be able to speak, separately from what the rules require, about how the vocabulary used in the rules should be presented to a user. Which predicates represent “things” in the real world that the user might need to tell us about, and which predicates represent legal conclusions that we should calculate and not ask for, and what the relationships are between the things that the user might need to provide, and how many of those relationships are possible, or required.

And it is useful to be able to say those things about a law in general, like “a thing that has permission to do something must be a person.”
```prolog
:- permitted(X,_,_), not person(X).
```
As long as you’re not worried about analyzing the rules to figure out whether corporations are allowed to do things that people are not, or vice-versa.

But it is also useful to be able to say things with regard to a specific application. Like if your application is supposed to tell someone if they are permitted to build something, you need at least one person, and you need at least one thing that person is trying to build.

### The Point

When we say “a person is not a gazebo” `:- person(X), gazebo(X).` we are modelling the laws’ model of the real world. We are confident that when the law refers to a person, and when it refers to a gazebo, it is referring to things that are mutually exclusive as a property of the real world, not as a legal result.

When we say that only a person can be permitted, `:- permitted(X,_,_), not person(X).`we are modelling the law’s model of the legal world. We are saying that according to this law, permissions apply to people.

When we say “there needs to be at least one person” `:- not person(X).` we are modelling the the scenarios that are relevant to the law’s model when trying to answer a specific question.

Some somethings you are modelling the law’s model of the world, sometimes you are modelling the law’s model of the legal conclusions that should arise from certain facts, and sometimes you are modelling a scenario relevant to a particular use of the law. Which is why this post is called:

> Modelling the World, the Law, and the Question

Being aware of what you are doing at each of those points, and the effects that it has on what else you can use your model for, is really important for getting the most out of an encoding of a law.

### Ontologies

That table having been very slowly set, let’s talk about ontologies. There is only one world. And there are things that we can usefully say about the real world that should be usefully true in a lot of different applications, like “a person is not a gazebo.” That sort of information can be encoded, and then put in a library. That library can then be used by other applications to collect and re-use that information.

When people do that, it’s called an ontology. These ontologies get more useful the more different ideas they connect together, and there are attempts to do that by building “upper” ontologies, that provide basic concepts that can be re-used with regard to more specific things in your application domain, and then to be able to get useful implications.

Like say, for instance, you want to write a law about a “park.” What you might be able to do is find an ontology in which the word “park” is defined already. And you could say that a “park” as described in the law is a “park” as described in the ontology. That would allow you to import ideas like a “park” is an area on one or more surfaces. But ontologies do not just contain abstract concepts, they also contain specific instances. So maybe the parks relevant to your law are already described, there, with a boundary using a set of GPS coordinates. You could put a GPS locator inside a car and determine if the car was in violation of the rule based on those GPS coordinates.

All of that is very cool. One potential problem is that we have no idea whether or not the real-world concept that corresponds to the word “park” in the ontology is the same real-world concept that corresponds to the word “park” in the law. The law is its own model of the world, and it can use the same words differently. Unless the people writing the law chose to use a definition from a specific ontology specifically, you can’t be sure. So there is a risk that you are saying things about the legal consequences of the law that are not necessarily true.

That’s not a reason not to do it, though. There is always a risk that you will encode the law wrong, and using ontological definitions doesn’t really make that risk bigger, it just requires you to be particularly aware of it, and not import ontological meanings where you are not confident that they are accurate. So you can choose to use the ontologies’ definition of the park’s boundaries, without using the ontology’s information about what a park is used for.

The good reasons not to use ontologies right now are that a) it is still surprisingly hard to do, and b) the benefits of using ontology are subject to network effects, and the network is not sufficiently large.

#### Hard to Do

There are tools out there for building ontologies and doing reasoning with them. These tools are technically impressive, but about as user friendly as the cockpit of an airliner. Which is a major improvement over the user friendliness that existed 20 years ago, don’t get me wrong. But learning how to write ontologies could be the subject of a graduate degree, still. That is a large cost to adding it to your tool stack.

#### Network Effects

A network effect is where the value of a thing increases with the number of other people using it. Social media is a great example. If there’s only one person on Twitter, it is useless.

Ontologies have the same feature. Using an ontology gets more valuable the more other people are using the same ontology, because then the things that you expressed in your legal encoding can be related to other legal encodings automatically. For example, if all of the laws were encoded using the same ontology for jurisdiction and topic, you would be able do a Google search for “marijuana laws that apply to me right now”, and Google would know.

That sort of semantic information is how Google is able to answer specific questions like “are there any flights from Tokyo to San Fransisco on Saturday.”

The idea of being able to do that for law is really neat. But as far as I can tell no one else is doing it, yet. So by using ontologies in your legal encodings you are taking on a lot of costs for future hypothetical benefit, and that benefit only arises if the ontology that everyone else ends up using is the same one.

It’s a think that I think has a lot of potential, but not a lot of present value, yet.