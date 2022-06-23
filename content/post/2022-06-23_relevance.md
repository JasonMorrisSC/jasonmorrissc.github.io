---
title: "Relevance in Blawx with s(CASP)"
date: 2022-06-23T15:42:18-06:00
draft: false
toc: true
keywords: ["blawx"]
---

This post describes a method that I have implemented in v1.3.15-alpha of Blawx for
using abductive reasoning features of the s(CASP) programming langauge to power
relevance decisions in expert systems.  I'll try to give you a brief introduction
to what problem we're trying to solve, how it has been solved before, how I have
solved it in Blawx, and how those solutions differ.

## The Problem

An expert system is a sort of application that takes a knowledge representation,
in most cases a declarative logic description of a set of rules, and a question that
it has been asked to answer, and then attempts to answer the question. When it requires
additional information it poses that information to the user, and continues.

"Relevance" in the context of an expert system is the question of drawing a distinction
between questions worth asking, and questions not worth asking, because they cannot help
you find an answer to a question.

For example, let's say there is a rule that you must be over the age of 18 and a citizen to vote.
Let's also say that we know you are 16. It is not relevant whether or not you are a citizen,
anymore. Collecting that information and adding it to what the system knows about you cannot
possibly have any effect on what the answer is.

Note that for this discussion, I'm presuming that what we want is at least one definitive
answer, and we do not care how many explanations we can provide for that answer. If we want
all the possible answers, and all the possible justifications for all of those answers, and we
want to be able to explain both positive and negative results with all possible justifications,
relevance can be defined with the rules alone, in the absence of any facts.

Here, what we are interested in is how can an expert system know not to ask about citizenship
if the age is already disqualifying, for example, and just answer "no".

## How It Has Been Solved Before

The earliest expert systems used an approach to relevance that was very straightforward.
A logical encoding had a conclusion, and a list of conditions that needed to be true for
that conclusion to hold. The software kept track of questions it had asked the user, and
answers it had received. The conditions for the rules are checked in whatever search order
the software uses, and if the software comes across an input that has not yet been collected
from the user, and cannot be derived on the basis of any inputs already collected, the
software pauses, poses that question to the user, adds the information the user provides,
and continues.

Other, more sophisticated approaches also exist. For example, if you build an expert system
using Oracle Policy Modelling, and you run that expert system in the debugger, the debugger
will display a list of questions that it knows are currently relevant to the interview,
on the basis of the information already provided. If those questions can be posed to the
user in a screen provided by the developer, that screen is scheduled to be displayed to the user
where it appears in the sequence of screens specified by the developer.
If it cannot, the system automatically generates a generic interview screen that will collect that
information, and that screen is scheduled to be displayed at the end of
the interview.

Here, the issue of how and when the question is posed to the user is an interface detail.
The "relevance" question in OPM is how does it know which questions should be displayed
and scheduled? In OPM, the rules are set out in a declarative way, but in a unified way.
Unlike in logic languages, where you can have two different rules for concluding the same
thing, in OPM there can be only one rule for a given conclusion. As a result, OPM can analyse
the tree of code, knowing that all of the relevant factors are present in that tree,
and consider all of the factors that can be determinative of the result given the current
information, and consider which of those factors have not yet been collected from the user.

The old-school expert system method uses a logical encoding, and sort of follows the reasoner
as it traverses its search, stopping whenever it needs some additional information. It learns
about the relevance of questions one at a time, and the order in which it learns them is
dependent on the search strategy.  This backward chaining method is also very similar to
the approach used in Docassemble, which is a popular tool for building legal expert systems
for generating legal documents.

The code analysis approach used by Oracle Policy Modelling uses a more functional style of
encoding, and finds all the relevant inputs at the same time. This has the advantage that
you can choose how to schedule those questions on the basis of human interface factors.

Some time ago I built a similar system in OpenFisca, a rules as code tool for microsimulations.
However, unlike the meta-programming methods used in old expert systems and OPM, that
method required a significant amount of additional code to make the original encoding
capable of reporting relevant inputs, and dealing with unknown values before they are
provided by the user.

## How I Solved It In Blawx

Blawx uses s(CASP), a stable-model goal directed answer set programming language with
constraints. The way that we calculate relevance in Blawx works like this:

### The Interview Loop

Much like the approach taken in tools like OPM, the relevance of additional questions is
constantly being recalculated on the basis of additional information from the user. The
basic loop of the interview implemented in BlawxBot is this:

1. Contact the reasoner with the information currently known to get answers and relevant inputs.
2. If there is an answer, display it. Otherwise, collect a relevant input.
3. Repeat.

Note that a relevant input includes "there are no more answers for this type of question".

The specific way that BlawxBot chooses which of the relevant inputs to collect is not
significant for our purposes, here. What is useful to understand is how BlawxBot describes
the known information, and how Blawx describes the relevant information.

### The Blawx Ontology

While Blawx is based on s(CASP), which allows for a generic FOL method of describing real
world objects and their relationships to one another, Blawx places quite strict limitations
on how the user can describe data. Users are restricted to creating "Categories", which
are represented as a unary predicate, and "Attributes" which are associated with categories,
and a data type for the second parameter. The permitted data types are currently boolean,
number, date, duration, and objects in a particular category. Attributes are represented
in the resulting s(CASP) code as a binary predicate. "Objects" are then placed into categories,
and represented as an atom applied to a unary category predicate. "Values" are added to an
object's attributes, which is represented by a binary attribute predicate where the first
parameter is the atom representing the object, and the second parameter is the value.

The information about what categories and attributes have been declared in the code is
available to the expert system by contacting the `/onto` endpoint for the relevant test.
This information is then re-used throughout the interview to generate descriptions of what
facts are known, and to interpret the information about what inputs are relevant.

### How You Describe What you Know To Blawx

When the interview sends information to Blawx about what information has already been collected,
it does so by describing the objects that have been created in each category, and the values that
have been applied to those objects' attributes, as might be expected.

Additionally, the interview advises the `/interview` endpoint whether the user has indicated
that there are no additional objects in a given category, and whether for a given attribute and
object, the user has indicated that there are no more values.

### How Blawx Describes What Is Relevant

The feedback from the `/interview` endpoint includes the answers that are already known for
the question, if any, and also information about what categories and attributes are relevant.

The simplest way that BlawxBot uses for determining whether any questions are relevant is
by asking whether any answers have been found. If the answer is yes, we have at least one
good answer with at least one good explanation, and nothing else is relevant. However, this
is an implementation detail of BlawxBot. The interview will also provide information on what
additional questions might be relevant to find additional positive answers to the query, and
additional explanations for those answers.

The list of relevant categories is a simple list of category names.
If a category is included, it means that creating another object
in that category could lead to a positive answer for the question.

If an attribute in a category is relevant, but the object's membership in that category
is not logically relevant to the conclusion, the category will not be included. The list
of relevant categories is a list of categories that are logically relevant, not relevant
in the context of how the interview chooses to collect the information. This is designed
to separate the interface from the reasoning to the greatest degree possible.

The list of relevant attributes is a list of binary terms that may be unground, partially ground,
or fully ground. If an unground term is included, that indicates that the attribute is
relevant with regard to objects in that category that have not yet been created. A fully-ground
term indicates that whether that object has that value for that attribute is relevant. A number
of fully-ground terms, and the absence of any partially-ground terms for the same attribute
and object suggest that there are a limited number of values that can possibly be relevant.
A partially-grounded term with the first parameter grounded indicates that the attribute's
value is relevant for the provided object. A partially-grounded term with the second parameter
grounded indicates that it is relevant whether any new objects have that particular value.
Again, the presence of partially-grounded terms with the second parameter grounded, and
the absence of an unground version of the same predicate, suggests that there are a limited
number of potentially valid values for new objects.

### How the Interview Endpoint Calculates Relevance

Now that we understand what information is sent to the `/interview` endpoint, and what
information is returned, we can look at how, exactly, Blawx is using s(CASP)'s advanced
reasoning features to obtain that output.

#### Step 1: The Answer Query

First, the interview endpoint accepts the information provided by the interview, and converts
it into a series of s(CASP) statements about known objects and values. Then the query is
executed for the test question, and any answers are obtained.

#### Step 2: The Relevance Query

Second, the interview accepts the information provided by the interview, and converts it into
a series of s(CASP) statements about known objects and values, as well as a set of s(CASP)
statements about unknown values. Here, we use the abducible reasoning features available in
s(CASP), essentially telling the reasoner to impose the rule of the excluded middle on
certain categories of statements, only considering models in which those statements are
either known false or known true.

For example, if the interview specifies that there are two people, adam and bernice, but that
the user has not indicated that the list of users is known to be closed, Blawx's interview
endpoint will generate the following code:

```
person(adam).
person(bernice).
#abducible person(X).
```

Likewise, the interview endpoint generates abducibility statements for attributes where
all the values have not been provided.  For example, if we know bob's age is 40, and there
are no other values for bob, and we do not know bernice's age, the interview endpoint
will generate the following code:

```
age(bob,40).
#abducible age(bernice,X).
-age(X,Y) :- not age(X,Y), X \= bob.
age(X,Y) :- not -age(X,Y), X \= bob.
```

Here, the first line states that bob is 40. No other possible ages will be considered.
The second line indicates that ages will be presumed for bernice, as required.
The third and fourth lines indicate that ages will be presumed for objects other than
bob as required. The interview endpoint generates the list of objects to exclude
by finding the category for the attribute, finding all of the objects in that category,
and adding a disunity for each of those objects for which that attribute is closed.

You will note that in this scenario, the third instruction is redundant to the second.
This is by design, as in the future we anticipate allowing the user to specify that the
list of persons is closed, making the third instruction unnecessary, but that Bernice's
age is unknown, necessitating the second instruction.

The same query is then run against the code with these additional abducibility statements
included.

#### Step 3: Analysing Results of the Relevance Query

s(CASP) will return a number of models, each of which is a minimal answer set for
a different wa of reaching the target conclusion based on the known information.

These answer sets are searched for the existence of statements that have been justified
by s(CASP)'s abduciblity system. That is to say, statements that s(CASP) has presumed
in order to obtain that model. Because s(CASP)'s answer sets are minimal, which is possible
because of the goal-directed execution used in s(CASP), while the truth or falsehood of
all the possible combinations of values will have been considered, the answer sets will
contain only those statements that were material to the conclusion in that answer set.

The collection of assumed statements are then used to generate the list of relevant
conclusions (using only the functor of the assumed term), and the list of relevant
attributes (including the entire assumed term).

## How the Solution Is Different

Unlike old-school expert systems,
which rely on asking question in the order that the inputs become relevant to the
search, Blawx's relevance method will never include a question as relevant merely
because it is relevant to the current point in the search tree, despite the fact
that elsewhere in the search tree the known information would be sufficient.

Unlike old-school expert systems, and similar to the approach in OPM, Blawx provides
information about all the currently relevant inputs, not just one, and allows the
interface to do with that information as they like.

Unlike Oracle Policy Modelling, it does not
require the user to write their code in a functional style. Blawx's code is
written in a way that is structurally isomorphic to the law, which simplifies coding,
review, and maintenance, even when the rules are defeasible. The style used in OPM requires that multiple rules
which overrule one another be reformulated into a single function.

Unlike many systems for building expert systems, by default the method used in Blawx
will include the goal question as one of the statements that can be presumed, and are relevant
to pose to the user. Old-school expert systems sometimes behaved in this way, asking the goal
question first and seeking sub-goals only if the user reported that the answer to the goal
question was unknown. Here, because Blawx includes the goal question as relevant, but does
not impose an order, the goal question might appear at an arbitrary place in the interface
unless it is handled differently.

Unlike OPM, which deems the existence of an object relevant, but does not schedule questions
about that objects properties and relationships until after the object exists, Blawx will provide,
even at the start of the interview, information about which parts of the ontology will
ever be relevant.

Unlike the approach that was previously used in OpenFisca, and similar to the metaprogramming
approaches in OPM and expert systems, Blawx's method does not require any
additional code from the user to calculate relevance than was provided to calculate answers.

Because s(CASP) uses goal-directed answer set programming, there are also queries that it is
possible to calculate relevance for in Blawx that it is not possible to calculate relevance for
in other logic-based programming languages.

## Future Work

The current implementation does not have a method of distinguishing between abducibility that
was created by the interview endpoint, and abducibility that was inherent to the code or the test
that is being implemented. There is also a risk that for sufficiently complicated rulesets, these relevance
queries are going to be prohibitively slow. I haven't stress-tested it, yet. Mitigating those
problems is left for future work.

There are also issues remaining with the BlawxBot demonstration expert system. For instance, it does not yet use the relevance information provided
to the fullest possible extent, because it does not yet detect when the valid values 
of an attribute for a specific object are enumerated by the reasoner, and use that information
to customize the user interface.

## Conclusion

The method of determining the relevance of additional inputs in Blawx is probably a world's first.
I don't think anyone has tried using goal-directed answer set programming to solve this problem
before. This is also the only solution that I'm aware of that provides all relevant questions
simultaneously, and does not require the knowledge engineer to violate structural isomorphism
between the law and its encoding.

Blawx is also the only expert system tool of which I am aware that is capable of calculating
the relevance of all inputs at the same time, and is open source. All other tools with these
sorts of capabilities are commercial, and tend to be extremely expensive to license.