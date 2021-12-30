---
toc: true
title: What is Justified Stable Model Constraint Query-Driven Answer Set Programming?
description: >-
  I spent some time today trying to understand where s(CASP) fits into the world
  of programming languages more generally, and here’s what I…
date: '2021-02-18T09:27:29.547Z'
categories: []
keywords: [scasp]
slug: >-
  /@roundtablelaw/what-is-justified-stable-model-constraint-query-driven-answer-set-programming-c895955bf68c
---

I spent some time today trying to understand where s(CASP) fits into the world of programming languages more generally, and here’s what I think is going on.

Remember, I’m not an expert. This is my amateur understanding after playing with it for a few days.

### What is the “Usual” type of programming language?

The usual paradigm for programming languages is called imperative programming, in which you basically tell the computer what to do, and in what order.

### What other paradigms are there?

Lots. Object-oriented programming, functional programming, logic programming. But we’re going to start from logic programming, and try to go from there to justified stable model constraint query-driven answer set programming, so that you have an idea what constrained answer set programming is.

### What’s Logic Programming?

Logic programming is when you write your code by making statements that have parallels in formal logic. There are different kinds of logic, but the simplest one is propositional logic. Propositional logic lets you say things like this

if socrates is human, then socrates is mortal.  
socrates is human  
therefore, we know that socrates is mortal.

in a logical language that looks like this:
```
H -> M  
H  
\----  
M
```
The logical language isn’t worried about the semantic content of the propositions, just about the “shape” of the argument, and whether you can get from the things above the line (premises), to the thing below the line (conclusion) only by using a small number of pre-defined rules for how those shapes can be manipulated.

In Prolog, a logic programming language, you might encode that like this:
```prolog
mortal :- human.  
human.  
?- mortal.
```
The top line is a “rule”, the second line is a “fact”, and the bottom line is a “query”. The answer to the query will be “yes”, because Prolog can prove “mortal” is true by using the rule and the fact together.

Predicate logic, or first-order logic, is a little more sophisticated, and allows you to say things like
```
every person has bob as a friend
```
in a logical language that looks like this:
```
A(X)(person(X)->friend(X,bob))
```
and which in Prolog code might look like this:
```prolog
friend(X,bob) :- person(X).
```
The way that logic programming works on a closed-world assumption. It assumes that if you haven’t told it that something is true, and it can’t derive something is true from what it already knows, then that thing is false. And the “things” involved need to be specific.

So there are some things that you can say in formal logic, like

every person has a friend

which can be stated like this
```
A(X)(person(X)->E(Y)(friend(X,Y)))
```
but there isn’t really a way to say that in Prolog. If you try, by typing something like
```prolog
friend(X,Y) :- person(X).  
person(jason).  
?- friend(jason,Y).
```
It will try to find a specific value for ‘Y’ that makes the statement `friend(jason,Y)` true. And it cannot, so it will return the answer “no.”

So logic programming is cool, but it doesn’t let you make abstract statements.

### What is Answer Set Programming?

Answer set programming takes a different approach, and allows you set out rules and facts and constraints, and uses them to create all the different possible valid combinations. Rules and facts are pretty much the same, but constraints are sets of things that cannot all be true at the same time.

Typically, your “query” is actually included as one of the things that you want to be true in the model, and then you ask it if if can find any models that match the description. This is called “satisfiability” checking.

So if we had this idea:
```
pizza is either yummy, or not yummy.  
pasta is either yummy, or not yummy.  
pasticio is a yummy pasta.  
pepperoni is a pizza.  
is it possible for there to be a pizza and a pasta that are both not yummy?
```
We could encode that, and the answer set program would figure out the following possibilities:
```
yummy(pepperoni), yummy(pasticio); and  
yummy(pasticio), not yummy(pepperoni).
```
And then it would search those possible answers for the thing you were asking about, and answer “no.” Because there is no possible combination of the rules and the specific examples that leads to both a not-yummy pizza and a not-yummy pasta.

Again, like logic programming, this tool is requiring that the statements be “grounded” in specific examples. It isn’t able to just imagine pastas or pizzas that you didn’t tell it about. A fully-grounded predicate is essentially the same thing as a proposition, so Answer Set Programming is based on propositional logic.

So in answer set programming you can say
```
a thing is always blue.  
a thing is always not blue.  
the pen is a thing.
```
And then ask it whether there is any models generated by that set of rules at all. Obviously, for the example above the answer is “no”. But in more complicated sets of facts and rules it can be very difficult to tell.

If you included your query in the model, getting no models back means that there is no way for your query to be true. If you didn’t, getting no models back means that your rules are impossible to satisfy, and contain some sort of contradiction. If you get an answer with your query in the model, those are the ways that your query can be true. If you get an answer without one, those are all the ways in which your model could be true.

### What is Query-Driven Answer Set Programming?

Generating all the possible combinations of facts and rules and then searching that entire database for the thing you are looking for is slow, particularly for complicated rule sets. Query-driven answer set programming speeds it up somewhat by not just generating every possible combination, but only checking the combinations of answers that can possibly result in what you are looking for.

It’s basically just a little smarter, and faster than traditional answer set programming.

Query-driven answer set programming also has the advantage that it knows what you are interested in, because the “query” is separate from the rest of the code, so it can tell you only those parts of the model that it is necessary to tell you about in order for you to understand the possibilities.

For example, if you have a number of mutually-exclusive categories:
```
I am a cat lovers or a dog lover but not both.  
My pet is a dog or a cat but not both.
```
And you ask the system whether it is possible that your pet is a dog, by adding “my pet is a dog” to the database, the traditional answer-set programming language will tell you
```
dog, not cat, cat lover, not dog lover; or  
dog, not cat, dog lover, not cat lover;
```
That’s more information than you were asking for. A query-driven system will just answer:
```
dog, not cat
```
because those are the only things relevant to the question you asked. That can help you focus on what was important, and make the results easier to read.

Again, though, these systems are “grounded”. They require you to be talking about specific propositions, or predicates with grounded terms, not variable terms.

If you want to be able to say something like
```
a person is either a cat lover or a dog lover but not both  
a pet is either a cat or a dog but not both  
I am a person, and I have a dog.
```
and then ask it for models, it generates the following possible models
```
person(you), has(you,your_dog), pet(your_dog), dog(your_dog), not_cat(your_dog)
```
But you cannot ask it whether or not it is possible for there to be a person who is both a cat lover and a dog lover. You can only ask it if there are any examples of things in the database that could be both cat lovers and dog lovers.

One very useful feature of query-driven answer set programming is that you can say what might be true, and ask it to find models in which the thing you are interested in is actually true.

So for instance, you can say
```
If I have a dog, I am a dog lover.  
I might have a dog.  
Am I a dog lover?
```
and the tool can reply something like:
```
assuming have_dog(you), dog_lover(you)
```
That ability to derive the facts that would lead to a conclusion is called “abductive” reasoning.

### What is stable model query-driven answer set programming?

Stable model query-driven answer set programming is a version of query-driven answer set programming eliminates the need to be specific. You no longer need to have specific examples of the things you are interested in to get answers about them.

In stable model answer set programming, the idea
```
if something is a dog lover, it is not a cat_lover, and vice versa.
```
… can be expressed as…
```
-cat_lover(X) :- dog_lover(X).  
-dog_lover(X) :- cat_lover(X).
```
And then you can ask whether anything can be a dog lover and a cat lover at the same time, by using the query
```
?- cat_lover(X), dog_lover(X).
```
And you will get the answer “no models.”

If you ask whether something can be a cat lover:
```
?- cat_lover(X).
```
it will say “yes, anything that is not a dog lover”
```
cat_lover(X), -dog_lover(X).
```
where the capital letter X stands for “anything”.

Answer Set Programming uses propositional logic, not predicate logic. Stable model answer set programming allows you to use predicates, which are usually written using parentheses with a lower-case letter starting the name of the predicate, and capital letters used to represent variables inside them. So the first version of stable model answer set programming, instead of calling it SMASP or SASP, was called s(ASP).

I _just_ got it.

Stable model query-driven answer set programming also gives you the ability to include predicates as the arguments of other predicates, which is a higher-order logic feature.

### What is stable model query-driven constraint answer set programming?

Wait, don’t we already have constraints? Yes. We do.

But “constraint programming” is a style of programming that is designed to let you deal with reasoning where the values of your predicates might have an infinite number of values, like when you are dealing with math. Every programming language knows that 1+2 = 3. But a constraint programming language knows that X>4 and X<8 means X is more than four and less than eight, and can reason about the value without knowing what it is precisely.

Constraint answer set programming adds the ability to deal with constraints to the other features of the language. So now you can say
```
a person with an age over 18 can drive a car
```
by encoding
```
can_drive(X) :- age(X,Y), Y .>=. 18.
```
then, because you can ask questions about inspecific things, you can ask
```
who can drive a car?
```
by writing the query
```
?- can_drive(X).
```
and you will get an answer that looks sort of like this:
```
can_drive(X), age(X,Y | Y > 18).
```
Which says that “something can drive a car if that thing has an age, and the age is over 18.”

### What is justified stable model query-driven constraint answer set programming?

Just take everything we have above, and add the ability to generate an explanation for any given model, including what parts of that explanation were assumed to be true.

So let’s say we have these rules:
```
a person can drive a car if they are over the age of 16  
a person can write a will if they are over the age of 18
```
And we are interested in knowing whether or not there are any people who can drive cars, but who can’t write wills…
```prolog
#abducible age(X,Y).  
can_drive(X) :- age(X,Y), Y .>. 16.  
can_make_will(X) :- age(X,Y), Y .>. 18.  
?- can_drive(X), not can_make_will(X).
```
If you run that code, one of the models returned is:
```
can_drive(A),  age(A,B | {B #> 16}),  not can_make_will(A),  age(A,C | {C #=< 18}),  not age(A,D | {D #> 18})
```
Which says “yes, it is possible when the person can drive, the person’s age is over 16, the person cannot write a will, the person’s age is less than or equal to 18, and the person’s age is not over 18.”

But that’s not terribly convenient to read. So we will add some information about how to describe these predicates to the user:
```prolog
#pred can_drive(Y) :: '@(Y) can drive a car'.  
#pred can_make_will(X) :: '@(X) can make a will'.  
#pred age(X,Y) :: '@(X) is @(Y) years of age'.
```
Then we run the code again asking for a human-language explanation for the models. This time we get the following:
```
QUERY:I would like to know if  
     A can drive a car and  
     there is no evidence that A can make a will.

ANSWER: 2 (in 3.321 ms)

JUSTIFICATION_TREE:  
A can drive a car, because  
    A is B greater than 16 years of age, because  
        it is assumed that A is B greater than 16 years of age, and  
        ...  
there is no evidence that A can make a will, because  
    A is C less or equal 18 years of age, because  
        it is assumed that A is C less or equal 18 years of age, and  
        ...  
    there is no evidence that A is D greater than 18 years of age, because  
        it is assumed that there is no evidence that A is D greater than 18 years of age.  
The global constraints hold.
```
It made three assumptions: the age of the person is not above 18, the age of the person is less than or equal to 18, and the age of the person is not below 16. It doesn’t know about any specific people, or any specific ages, but it can understand how the rules interact, and it can explain the interactions.

So it is a form of “explainable artificial intelligence.”

### Wrapping it All Up

So, justified stable model query-driven constraint answer set programming is a programming paradigm in which:

*   You write facts (things that are definitely true), rules (ways of getting new information), and constraints (things that are definitely not true), and then pose queries (is it possible for this to be true?).
*   Only valid answers are found (“query driven”). In the answers, only relevant information is included (“query driven”). Instead of getting the values that could make the query true, you get all of the possible ways in which any values could make the query true, and all the relevant things that are true in each case (“answer sets”). Which means two of the answers may have the same values for the variables in your query, but represent different “reasons” for why those values work. This also allows you to use the same rules to do both deduction (if these facts are true what can I conclude?), and abduction (if this conclusion is true, what facts might cause it to be true?).
*   You can use predicate logic, and higher-order logical features, and you can write rules and ask queries that are entirely abstract, without reference to specific values (the “stable model”).
*   If you want to reason about unspecific values over an infinite domain like numbers, you can do that using constraint programming features (“constraint”).
*   It can explain its answers in a natural language way (“justified”).