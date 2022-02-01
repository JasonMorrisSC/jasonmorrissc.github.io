---
title: "Using OpenFisca to Power Expert Systems in the Canadian Public Service: Lessons Learned"
date: 2022-01-25
draft: true
toc: true
keywords: ['cls2022','openfisca','expert systems']
---

This is a draft of a paper being prepared for the Singapore Management University Centre for Computational Law's Computational Legal Studies Conference 2022.
As an accepted paper at CLS2022, it is also under consideration for publication in the MIT Computational Law Review.  Whether or not to publish it prior to that
event or prior to publication in the MIT CLR will depend on the opinions of those two organizations.
 
## Abstract

The paper demonstrates an approach to encoding legislation in OpenFisca, and an approach to manipulating the responses to queries to the
OpenFisca API, which in combination can be used to obtain many of the features required for legal expert system development. Those requirements
include explanations that can be tied to legislative justifications, the ability to deal with incomplete inputs, and the ability to calculate
the relevance of inputs to a given conclusion given incomplete information.

The approach set out involves
treating sections of legislation as "variables" in the OpenFisca encoding, and then duplicating the encoded logic to represent a separate confidence conclusion
for each variable. These two encodings are queried at the same time, and the results combined into a directed graph. The graph can then
be analysed to generate information about relevance, certainty, and to generate legislatively-justified explanations.

The paper briefly analyses this approach in contrast with others, including standard OpenFisca code, simple decision-trees, and declarative logic
encodings. The paper concludes that while it is possible to encode legislation in OpenFisca in such a way as to power expert systems with these sorts of requirements,
it is unlikely to be a practical approach in most circumstances. The paper then briefly discusses what alternative tools are required in the Rules
as Code space to simplify these sorts of tasks.

## Introduction

### Background

Lexpedite Legal Technologies Ltd. is contracted with Canada's Ministry of Employment and Social Development. Under that contract, the author works full-time as 
"Director of Rules as Code" inside
the Benefit Delivery Modernization Programme of Service Canada. Service Canada is the subdivision of ESDC Canada responsible for the delivery
of many services to Canadians, both in person and online, including a number of important social benefit programs. One such benefit is Old Age Security.

The Benefits Delivery Modernization programme is an effort inside Service Canada to modernize and digitize the delivery of benefits services to Canadians. 
It takes an agile and user-research-focussed approach to redeveloping how services are delivered online. The author's role
in that programme is to find, learn, experiment with, and build open source technologies for Rules as Code to explore its potential as
a foundational technology for the future of digital public service delivery.

As part of that task, the author has used an open source Rules as Code tool, OpenFisca, to encode provisions of the Old Age Security Act,
in order to test the feasibility of using that code as a legal reasoning resource for a user-facing expert system. This paper discusses the
requirements of that project, and the experience of satisfying those requirements using OpenFisca, which is not designed for powering
expert systems.

### "Rules as Code"

### Expert Systems

The term "expert system" is one that has changed in usage over time. During the rule-based artificial intelligence wave of the 1980s, an
"expert system" referred to a tool that encoded the knowledge of an expert, and had two primary capabilities:
* if it had the required facts, it could calculate an answer to a query based on the encoded knowledge, and
* if it did not have the required facts, it could determine what facts were relevant, based on the same encoding.

These systems were used to build applications, written in declarative logic languages, that asked the user one question at a time, and provided the
user with an answer when enough information was known. Many also had the ability to justify how the systems had reached
their conclusions using the same encoded knowledge, but because many expert systems encoded knowledge that was heuristic,
or used to guide the interview process, the justifications were not viewed as terribly useful to end-users, and were
used primarily as tools for debugging the applications.

That "ask questions, give answers" interface has unfortunately been conflated with the technology, 
and so the phrase "expert system" now refers mainly to things that have similar interfaces. It 
is frequently used to describe any system that collects relevant information from an end-user, 
particularly in a form, or an interview, and answers
a question based on the provided facts and an encoding of expert knowledge, regardless of:
* how (or even whether!) the system determined that the facts requested were relevant, or
* whether the question was answered using the same encoding.

I find this change in usage unhelpful. By this revised definition, the difference between a function and an expert system is
that a function is not typically end-user facing.

In this paper, "expert system" is used in the former sense. While expert systems of the past were typically created 
using declarative logic programming approaches, the
underlying technology can be abstracted away from the capabilities of the resulting tool. From the perspective of a
user sending a legal query to a web API endpoint, the language being used by the server to calculate the response is
irrelevant. The definition of "expert system" used in this paper therefore does not require the use of declarative logic programming. Whether
declarative logic programming is an advisable approach to building expert systems is something we will return to.

### The Eligibility Estimator Prototype Requirements

The Eligibility Estimator is a prototype web application under development in BDM. It is a web application that collects relevant
information from a user, and provides information about whether a user qualifies for four benefits implemented under
the Old Age Security Act, why or why not, and if so, in what amount. The team developing this app were using a typical
approach to encoding the legislative requirements, and meeting these requirements. My task was to parallel that approach
by encoding the relevant provisions of the Old Age Security Act in a "Rules as Code" way.

The objectives were as follows:
* A single encoding that answers all the relevant questions,
* available over a simple Web API,
* capable of explaining its answers,
* capable of responding to queries with inadequate facts, and advise which additional facts are relevant,
* capable of answering questions with uncertain facts that it is not possible to collect, to be able to advise clients
  that the answer to their question is contingent on those additional pieces of information,
* capable of answering questions as to the eligibility (whether or not the user qualifies) and entitlement (how much
  the user qualifies for) with regard to the four benefits outlined in the OAS Act, and
* uses a minimal set of inputs, defined by the application developers.

In addition to those requirements, in order to explore the possibilities of using OpenFisca for encoding legal expert
systems, I also sought to accomplish two additional objectives:
* Explanations should make reference to the legislative authority, and
* all of the answers should be generated from the same knowledge encoding.

Note that even with these additional objectives the project differs from a "true" Rules as Code encoding, because it
required only encoding those elements of the OAS Act that are relevant to the questions being answered by the 
Estimator. An encoding of the entire Old Age Security Act would have been an unwieldy project to attempt in the
time allowed.

## OpenFisca

In order to understand what OpenFisca is, and how it works, it's necessary to take a step back and describe the
problem that it is designed to solve, which is microsimulation.

### Microsimulation

Simulations are often done in government to predict the possible future states of the economy. Consider the need
for government to analyse how its tax revenues will change in the face of the looming retirement of a large percentage
of their population. Understanding the scale of the problem, and when it is likely to arrive, is something that
can be aided by simulating the economy.

These simulations can be broken down into two categories, macrosimulation, or microsimulation. In macrosimulation,
the things that are represented in the model are abstract macroeconomic indicators, such as inflation rates or gross
domestic product. Rules are applied for how these macroeconomic indicators relate to one another, and how they are
expected to change, and the simulation is run to see where those indicators end up, and when.

Microsimulation, by contrast, models the economy by simulating smaller parts such as individuals, families, households, and firms.
Again, rules are set out that describe how these facts are anticipated to change over time, and how the various
facts relate to one another. The simulation is then run, and the future state of the simulated population is
analysed to learn what might happen.

Microsimulation is used to analyse the effect of tax and benefit rules, and the effects of proposed changes to those
rules. Information about the population and how it is expected to change is included, as are rules about the taxes
and benefits that apply to that population, and how those rules are proposed to change. The microsimulation can then
be run to see what the aggregate effect of those policies might be on the population.

In some jurisdictions, this sort of analysis is a legal prerequisite for the adoption of tax and benefit legislation.
Microsimulation provides a convenient way to meet those legal prerequisites. There are also some legal requirements, 
such as maximum marginal tax rates, that are difficult to calculate across
a wide variety of fact scenarios. Using a microsimulation with randomly generated populations of sufficient size
can provide comfort that a proposed legislative change does not violate those sorts of legal constraints.

Some of the particular technological challenges associated with microsimulation are:
* it requires a large number of calculations to be applied to a large number of entities, repeatedly;
* the more calculations you can perform, and the faster you can do it, the more accurate your simulation may be;
* it needs to be able to deal with how populations and rules change over time; and
* it needs to be able to compare the results of different versions of the same rules.

The target users of microsimulations are usually data scientists. However, any system that is capable of calculating
the effect of a tax or benefit policy change on tens of thousands of people is also capable of calculating the effect
with regard to one person. Code generated for microsimulation is therefore also re-usable to generate
user-facing applications that help people understand the effects of existing and proposed laws.

### Open Source Microsimulation in OpenFisca

OpenFisca is an open source platform, built in the Python programming language, for generating microsimulations of
national tax and benefit systems. It has a number of features that are designed to solve some of the technical
challenges associated with microsimulation, and to do so in an open source context so as to promote
transparency and accountability.

In order to ensure that OpenFisca encodings are capable of processing large quantities of data as quickly as
possible, OpenFisca is uses vectorial computing. All variables are presumed to be an arbitrarily long list of
values, and operations are performed against these vectors, not against the values individually. All values in
OpenFisca are also presumed to change over the additional dimension of time. Rules are implemented by describing
the periods of time to which they apply, and all values are calculated as per a given point in time or period of
time. This makes it easier to specify how rules have changed over time, and to ask questions about dates in
the past and dates in the future.

For the most part, vector computation and temporal logic are hidden from the user, making it easier to describe
how values are calculated as if you were calculating for a specific entity at a specific point in time. OpenFisca then does the work
of extrapolating how to calculate that same value at any given point in time, and for a large number of entities,
automatically.

OpenFisca also has features that allow the developer to parameterize rules, and to specify changes to those parameters
separately from the rules themselves. This is useful for expressing, for example, how tax rates and brackets have
changed over time. Beyond that, OpenFisca also allows the user to describe alternative versions of the rules that
go deeper than just parameters, called "reforms". A query can then be run with or without any proposed reform
included in the rules, to see how the proposal changes the results.

OpenFisca comes with a number of tools to facilitate the development of these encodings, including testing tools,
and a system of automatically deploying an OpenFisca encoding as a RESTful Web API. The API provides two primary
endpoints, `/calculate` and `/trace`, where the latter is used for debugging the results of a calculation. There
is also a legislation explorer that can be used to automatically generate a web interface for making requests
against an OpenFisca API, as well as tools for automatically generating Swagger documentation for the encoded
systems.

OpenFisca has also been extended by other projects. Noteworthy is the Policy Engine project, which seeks
to simplify the task of building web interfaces that allow end users to explore the effects of tax and benefit
policies encoded in OpenFisca.

TODO: Where has OpenFisca been used, links to examples.

### Why Use OpenFisca?

OpenFisca was selected for use inside BDM in a number of projects that pre-date my involvement.
ESDC and the Canada School of Public Service have collaborated over the last few years on Rules as Code
discovery projects that used OpenFisca with departments including Fisheries and Oceans, and Labour. That important
work continues, and we continue to contribute to it as part of my role within BDM.

The motivations of selecting OpenFisca for the Eligibility Estimator demonstration include:
* a desire to build on that existing knowledge and experience,
* a desire to use a mature tool that has been successfully deployed elsewhere in similar circumstances,
* the desire to use open source technologies in accordance with the direction of the Government of Canada,
* the desire to use open source technologies because of the implications for algorithmic accountability in Rules as Code in particular,
* the need for a tool specific to the encoding of benefits legislation, 
* my familiarity with the Python programming language and ecosystem,
* good documentation, and 
* the availability of support from the helpful and growing OpenFisca community online.

At the time of starting the project, and at the time of writing, I am aware of no preferable alternatives given these objectives.

That said, had there been viable alternatives red tape would have been a relevant factor.
Obtaining approval to use new software technologies in ESDC is a time-consuming, complicated, and unpredictable process that moves
at what used to be called a "glacial" pace, but given the realities of climate change is probably better now described as "tectonic."
Authorizations in this process are based on security reviews that may not apply to revised versions of the software,
or even security patches, making the approved software unsafe or out of date before it is authorized for use.

OpenFisca, as a tool that had already seen considerable use in the department, and as a library for
an already-approved interpreted programming language (i.e. Python), would have had an almost insurmountable advantage over any tools
without those features.

## Typical OpenFisca Code

In this section I will briefly introduce a toy example of how a simple rule might be encoded in OpenFisca, and then give an explanation
of why this style of encoding is not, in and of itself, sufficient for powering expert systems.

### Entities, Variables, and Parameters

The basic structure of an OpenFisca encoding is divided between entities, variables, and parameters.

An entity is a type of object that will be simulated, such as an individual, a household, a family, or a firm. Typically, the number of entities
required is relatively small. Many encodings can be done creating only an "individual" entity.

A variable is a thing that you can either specify or derive about an entity. OpenFisca doesn't distinguish between the two. If you specify
a value, OpenFisca will use it. If you do not, and you have described how the variable can be calculated, it will attempt to calculate the value
for you if required.  A variable has zero or more formulas, each of which is used to calculate that variable for a given period of time. If you
are not concerned with time, you can define one variable that defines the variable at all points in time.

A parameter is a scalar value set out in the relevant rules that might change over time without there being changes to the rest of the rules.
It may be a tax rate, an entitlement amount, or the age of majority.

### Age, Birth date, Minor

As an example, let's assume that there is a law that defines a minor as someone under the age of 18. You want to build an application that
can accept a birth date, and calculate whether the person is a minor.

The first step is to set out the relevant entities. In this case, the only entity type we are concerned with is "Individual."

```python
# This is the code for building a person entity
```

Now that we have defined the entity, we need to define three variables: `birth_date` will hold information about what day the person was born,
`age` will hold information about their current age, and `is_minor` will hold information about whether they are legally a minor. Here is how those
variables would be defined.

```python
age
```
Note that the encoding of `age` specifies that the relevant time period is "DAY", whereas the encoding of `birth_date` says that the relevant time
period is "ETERNITY".  This is how you explain to OpenFisca that a person has only one `birth_date`, and it never changes, but their `age` might
be different on one day from the day previous. Similarly, whether or not they are a minor depends on their age, so it can also change from day
to day. The definition of `birth_date` has no formula, which means that for OpenFisca to know anything about a persons' `birth_date` it will
need to be provided by the user.  On the other had `age` has a formula which can be used to calculate the person's age based on their `birth_date`.

```python
minor
```
In our formula for calculating whether or not the person is a minor, we make reference to a parameter, the `age_of_majority`.  That parameter
is defined elsewhere, in a YAML file, as follows.
```python
age_of_majority
```
With these encodings, we can deploy our code to an API, and send queries that look like this:
```json
query
```
and receive a response from the server that looks like this:
```json
response
```
Note that this code does not explain how the answer was obtained, it just says what the answer is. So by default OpenFisca code doesn't have any
way to explain its answers. If we sent an under-specified query, with no age and no birth date, like this:
```json
no info
```
we receive this as a response:
```json
no answer
```
So we can see that OpenFisca by default is also not able to tell the user what additional information would be relevant to the question that was asked
when the information provided is incomplete. It also doesn't have any built-in method to provide answers that are contingent on unspecified information.
Instead, if information is not provided, OpenFisca treats it as though a value of 0 was provided for each datatype. For dates, the 0 becomes
January 1, 1970. For boolean values, 0 becomes false.

There is a mechanism to get more information from the server about how it reached its conclusions, by targeting the `/trace` endpoint instead of the
`/calculate` endpoint. The same query from above sent to `trace` returns information like this:
```json
trace result
```
There is a lot of information there, but it is not in a format that is particularly helpful to understand what you are trying to do.

## Using OpenFisca to Power Expert Systems

Out of the box, OpenFisca provides us with a way to calculate the answers to multiple questions, and expose that encoding over an API.
It does not explain its answers, or linking the explanations to legislative source material for the encoding.
It also doesn't have anyway to distinguish between "zero," and "unknown." That in turn means that it can't even tell when it
hasn't been provided with all the relevant information, what other information might be relevant, or what facts a conclusion is
contingent on.

What follows is a description of the approach we used to overcome these limitations.

### Legislative Parts, Sub-Parts, Conditions, and Conclusions are all "Variables"

The first thing to note is that in trace results, OpenFisca provides information about variables, and the variables and parameters that were
used to calculate them. That information can be reformatted as a tree, which will provide a version of an explanation for the conclusion.
But for that explanation to be correlated to the source material, the elements of the legislation need to be implemented as OpenFisca variables.

In our example, the dependency graph is going to look like this:
```text
is_minor
 - age
```
In our `is_minor` example, we could rename the variable `is_minor_according_to_majority_act_s_1`, which would be helpful, but we would still not know
the variable `age` is being used. We can solve that problem by adding another variable between that explains how the elements were being used.
```text
is_minor_according_to_majority_act_s_1
  - age_is_below_age_of_majority
    - age
```
By renaming one variable and adding another, we can now use the response from the trace, with some post-processing, to generate a tree that
will provide an explanation as to how the decision was reached, and reference the relevant legislation. Note that in our example, there is only
one statutory explanation possible for why a person is a minor. Often, that will not be the case, and adding legislative elements will also
require the addition of additional layers of variables. For instance, if being a minor could be legally inferred either due to the majority
act or a second piece of legislation, we would need variables for `is_minor`, `is_minor_according_to_majority_act_s1`, `is_minor_according_to_other_act_s1`,
`age_is_below_age_of_majority`, and `age`, along with whatever other variables are used by the other act.

Note also that this does not require you only to provide variables for numbered sections of legislation and input variables, and legal conclusions. You 
may also need to create variables to represent sub-sections of a numbered section of law, where those subsections are going to be relevant to the
explanation presented to the user. If it makes a difference to the user whether they qualified as a Canadian resident, or as a foreign resident, and
the requirements for each are alternatives described in a single paragraph of text, you will need to divide that out in to sub-variables as well.

### Encoding Confidence

Once we have reformulated our encoding to add variables for all of the legislative parts, sub-parts, conditions, and conclusions, we are left with
the problem that OpenFisca doesn't actually know what it knows, because it does not know what it has been told, and what it is simply assuming is "zero".

The solution to that problem is to take our list of variables, and duplicate each variable with another variable with the word "_known" at the end of
its name. This "known" variable will track information about what the system has been told, and what the system has confidently derived.

In our `is_minor` example, we would now have the following list of variables:
* is_minor_according_to_majority_act_s1
* is_minor_according_to_majority_act_s1_known
* age_is_below_age_of_majority
* age_is_below_age_of_majority_known
* age
* age_known
* birth_date
* birth_date_known

Each "known" variable is then given a formula for determining when we can be confident about the answer that OpenFisca has generated.  In this case,
the formula is not complicated. Whether the person is a minor according to the majority act is known if the only factor it depends on is also known.
Likewise for whether age is below the age of majority. That is known if we know the age, and we can presume that we know the age of majority because
it is a parameter. The person's age is known if their birth date is known, and their birth date is known only if the user says so, so it does not
have a formula.
```python
#Example of formulas
```

This structure gives the user the ability to say what the birth date is, and whether or not they are confident of that information. If they are not
confident in that information, whether they are a minor will be calculated in the usual way, but whether OpenFisca is confident in that conclusion
will remain "false", because it is presumed to be "zero".

Writing these formulas for the "known" variables takes some careful consideration.  Frequently, a variable will be calculated on a conjunction, or a disjunction.
Or a combination. Take an example where a person is entitled to a benefit if they are a minor, and a citizen. When is `entitled_to_a_benefit` "known?" It is known
as soon as any of the conjunctions is known, and false, because at that point, the conjunction is also known to be false. It is also known if both values are
known, regardless of what the values are. This leads to formulas like the following in "known" variables.
```python
known if either_false or both_known
```

Similarly disjunctions are known if any of the disjunctive elements are known true, or if all of the elements are known. The complexity of these "known" formulas
seems to grow exponentially with the number of variables referred to in the substantive variable, which acts as another motivator to break complicated variables
into simpler ones so as to simplify the encoding of the "known" versions.

### Post-Processing the Results of OpenFisca Queries

Once we have made these changes to our encoding, we can send a request to the API with the relevant inputs and "known" variables. If something is presumed,
we set it to its presumed value, and set "known" to false. We query the substantive conclusion we are interested in, and the "known" equivalent for it, and
we send the request to the `/trace` endpoint, so that we will get the information from which we can build the explanation tree.

We then follow this algorithm to build a single dependency graph from the information in the response to both the substantive variable and the associated known variable.

```pseudocode
Create a directed graph.
For each variable included in the response, add a node to the directed graph, with the value calculated.
For each variable referred to as a factor in calculating a variable in the response, create an edge from the conclusion to the factor.
For each variable in the graph, find the corresponding "known" variable, and add the value of the known variable to the node for the substantive variable.
```

This graph can now be used to generate explanations, determine relevant inputs, and calculate whether answers are contingently true.

#### Generate An Explanation

To generate an explanation, generate a tree from the graph using the variable you are interested in as the root node. For each element in the tree, 
include the substantive explanation and whether it is known, and display it to the user. Because OpenFisca includes information about which
were the query variables in its `\trace` output, this can be done automatically for all variables that are not "known" variables.

#### Determine Relevant Inputs

To determine what inputs are relevant, first, generate the explanation for the variable you are interested in. If the root variable is "known", there are no other relevant inputs.
If the root variable is not "known", find all leaf nodes that do not have a "known" ancestor. Those are the relevant inputs. Note that this method assumes that only
leaf nodes are being collected from the user. In a situation where you are happy to ask, for example, either the person's age, or their birth date, then the relevant inputs
are all the nodes with no known ancestor.

This provides the application with a list of relevant nodes, but doesn't advise the application on how to prioritize them.

#### Answer Contingent Questions

To answer a contingent questions subject to assumptions, first, determine the relevant inputs for the target query. Then get the list of variables that your application can collect from the user.
If none of the relevant inputs are inputs that your application can collect, the
answer provided in the explanation is contingent on the relevant inputs' assumed values being correct.



## Analysis

Dealing with OpenFisca's vectorization of everything is awkward. While being familiar with Python is
an advantage, it is not as strong an advantage as you would expect unless you are also familiar with
NumPy. Standard Python keywords like `and`, `or`, and `if` do not work as you would naively assume
when dealing with vectors, which makes the task of writing encodings in OpenFisca challenging.

The need to always annotate input and output data and parameters with dates is also a frustrating
but understandable annoyance. I anticipate these things would fade into the background with time.

Having gone through the experience of figuring out how to get OpenFisca to behave like an expert
system reasoning tool, and implementing that approach in a limited context, the prevailing impression
I was left with was that I was writing so much code, and getting so little for it.  I suspect that
this is a result of forcing OpenFisca to occupy a role it wasn't designed for.

### Rules as Code v Application Development

Recall that application development and Rules as Code
are not the same thing. Analyzing whether or not OpenFisca, or any particular approach to the
use of OpenFisca, is an appropriate tool for Rules as Code generally is a very different question
from deciding whether or not it is a good idea with regard to a given application.

Encoding an entire piece of legislation so that it can be used both by your application
and arbitrary others is, in economic terms, a Public Good. The benefits received by the other
users of your encoded law is a positive externality that imposes costs on the current
development project that are possible to recoup. Rules as Code is therefore almost
never going to be a good idea when viewed from the context of the development of a single application.

That is not the right context. Rules as Code is digital justice and public administration infrastructure.
Tools and techniques for Rules as Code need to be evaluated in that context. The question that this
paper seeks to answer here is not whether or not we should "do" Rules as Code. Rather, if we have
decided to do it, and we have decided that we want it to be explainable, justified by reference to
the source legislation, capable of understanding incomplete facts, and advising as to relevance;
in *that* context, what are the advantages and disadvantages of the above approach to using
OpenFisca as compared other alternatives, available or anticipated?

### Declarative Alternatives We Need Don't Yet Exist

I will talk below about the comparison between the experience of using OpenFisca and using declarative
logic programming for the same task. It's important to say up front that in this context, declarative
logic solutions for rules as code are "anticipated", and not "available."

I base the arguments below on my experiences using declarative logic encodings for Rules as Code as
part of my work at SMU Centre for Computational Law, in the development of Blawx, and during my LLM
thesis work at the University of Alberta. In future papers I would like to do an apples-to-apples
comparison between imperative and declarative approaches, to give greater veracity to the arguments
made below.

That said, even if I am to be believed about the benefits of declarative encodings, there are currently 
no open source tools based on declarative logic that have high quality documentation, 
allow for easy deployment of code to web APIs, and have all of the explainability and reasoning features described
above. There are a few commercial options that are not appropriate for adoption in Rules as Code because
they are proprietary, most notably Oracle Intelligent Advisor. There are a few open source alternatives, all 
of which are missing at least some of the factors above.

As part of my work with the Canada School of Public Service I'm actively working on a revised version of 
Blawx with many of the needed features. Work on L4 at Singapore Management University Centre for Computational
Law continues, also, and there are likely others of which I am not aware.
But even the perfect declarative logic Rules as Code tool emerged today, it would be a matter of years before
it would have the level of community support and real-world use that OpenFisca enjoys now.

So my comments below should not be taken as reasons not to use OpenFisca, but as reasons to expand our
toolkit of Rules as Code technologies.

### Code Length

Comparing the encoding of the knowledge in an entire law with the encoding of the knowledge in a law
required for answering a specific type of question is an "apples and oranges" affair.  That said,
if you compare the code length of all of the available approaches, both Rules as Code and status quo,
the shortest code length is in the status quo. I would estimate the difference to be in the range of
a factor of 10. It's hard to overstate the advantages for development time when you know in advance
what question you need to answer, what inputs you will have, and what outputs you care to provide.

I mention this not to applaud the status quo. The status quo remains extremely risky in terms of 
legal compliance, duplicative of effort, and inconsistent in results.
I mention it to make clear to Rules as Code advocates how steep the uphill battle
is to convince people to adopt Rules as Code as an approach to application development.
Application development is a use-case for encoded legislation, but it is not a reason to write those
encodings. For Rules as Code to be a good idea inside the context of an application development process,
the application would need to be astronomically complicated, or the encoding of the law would need to
already exist. The latter is more realistic, and that's the approach I think we should pursue.

With regard to the approach described above there is no way to avoid the conclusion that using OpenFisca
that way involves writing an enormous amount of code. In particular, the need to duplicate the logic of the encoding in order
to calculate certainty results in a more-than doubling of the lines of code required. It is theoretically
possible that some of that work could be automated, by generating the confidence code from the code
for the other variables, but the difference is still stark.

Let me provide an anecdotal example. I used a declarative logic programming language called s(CASP)
to encode a section of Singapore legislation last year with SMU Centre for Computational Law.
The ratio of lines of code to words in the legislative text was 0.5. Every two words of legislation resulted in a line of code. When I did
the same calculation for this work in OpenFisca, the ratio was 2. And at the time I did the calculation the code was unfinished, so the real ratio
for the OAS code was ultimately going to be higher.

That's more than a four-fold increase in lines of code using the approach
described above as opposed to a declarative encoding.
Even if you could automatically generate the confidence code, it is still more than
a two-fold increase.

The singular of data is not anecdote. But if doing these encodings in OpenFisca actually is harder, that is only
the "I" part of "ROI". We also need to measure the return. Unfortunately, the only place I can find
where using the OpenFisca method gives a greater return is in processing speed. Meanwhile, several
capabilities available in a declarative environment are missing.

### Imperative vs. Declarative Logic

One of the things that you use by using an imperative programming language instead of a declarative
language is the ability to reverse the reasoning. 

Imperative functions for calculating
variables go in only one direction, from inputs to outputs, and cannot be automatically reversed.

Declarative logic programming, by contrast, allows you to specify relationships, and then use them
in both directions.

An equivalent declarative logic programming encoding of the OAS rules might allow for asking
abstract reasoning questions, like "are there any circumstances in which a person qualifies for GIS and
lives outside of Canada?" That is going from what in the OpenFisca encoding is a target conclusion,
whether the person qualifies for GIS, and reversing the logic to see if it is possible that they live
outside Canada. And it can be answered without providing specific facts.

With the approach described above, the only way to get an answer to that question would be to
randomly generate a large population, and test it to see whether or not that situation arises.
And even then, you don't know that it is impossible, you just know you didn't find any examples of it.

So while OpenFisca code can be written in such a way as to get some of the features of declarative code
alternatives, it is still not capable of everything that the likely much shorter declarative
encoding would have been able to do.

## Conclusion: OpenFisca Alone Is Not Enough

The question is not whether to use OpenFisca for Rules as Code. It clearly belongs in the quiver. If you need microsimulation,
it is an excellent option. If you need microsimulation, and you don't need things like explanations, legal
justification, relevance, and dealing with incomplete facts, it is also a reasonable way to build simple web apps.

But my experience using it suggests that if you do need those extra things, or if you are trying to build a generalized encoding
for people who might, OpenFisca is suboptimal. It can be forced to behave sort of like
an expert system engine, but the effort and risk involved is needlessly high.

If you don't need microsimulation or temporal reasoning features at all, OpenFisca is probably not the best alternative.
Using it is slightly more difficult because of the temporal and vectorial features.

Making OpenFisca behave like an expert system is hard, and risky. This paper does not address the alternative
of attempting to make declarative code behave like a microsimulator, but my instinct is that it would be similarly
difficult, if it is even feasible. Much of what happens in writing a microsimulator is the programmer using their
knowledge of the rules to find more efficient ways of reaching the same conclusion, and declarative logic is not renowned
for its computational efficiency.

What seems obvious to me from the experience of having used both OpenFisca and declarative methods is that
microsimulation and declarative logic reasoning are things that are more powerful in combination than either is on its own.

With both, you could use the declarative encoding with subject matter experts to verify it matches their expectations.
That is arguably easier to do with declarative code, because declarative code matches the structure of the source rules much better. Then,
the declarative encoding could be used to generate tests for microsimulation projects. If the microsimulation
code passes all the of the tests generated by the declarative code, then it is consistent with the declarative code,
which in turn is consistent with the experts' expectations. If a test fails, the declarative code can be used to
generate an explanation for why the result that was generated was not correct, and why the result that was expected is
correct. That would be a boon to quality assurance in developing legal applications generally.
In terms of microsimulation code, it allows us to show legal consistency without making any sacrifice of efficiency.

Rather than use OpenFisca to power expert systems, people in the public sector should be investing in declarative logic tools
that are designed for that task, and that can be used to improve the quality of all the legislative encodings we create now.

## Acknowledgements

I would like to thank the maintainers of OpenFisca for their valuable and important tool.
I would like to thank the members of the OpenFisca community who have and continue to assist me in figuring out how to
get OpenFisca to do things it wasn't designed to do. I would also like to thank the members of the Eligibility Estimator
team for their help in understanding their use case and their needs, and for having done the work of encoding the status
quo version before I even got started. I would also like to thank the organizers of the Computational Legal Studies
Conference for hosting the conference, supporting the academic field of Computational Law, and creating forums for conversations
around topics like Rules as Code.
