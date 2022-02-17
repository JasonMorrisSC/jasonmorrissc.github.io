---
title: "Using OpenFisca to Power Expert Systems in the Canadian Public Service: Lessons Learned"
date: 2022-02-16
draft: false
toc: true
keywords: ['cls2022','openfisca','expert systems']
---

## Using OpenFisca to Power Expert Systems in the Canadian Public Service: Lessons Learned

This is a draft of a paper being prepared for the Singapore Management University Centre for Computational Law's [Computational Legal Studies Conference 2022]().
 
## Abstract

The paper demonstrates an approach to encoding legislation in [OpenFisca](), and an approach to using the
OpenFisca API, which in combination can be used to obtain many of the features required for legal expert system development. Those requirements
include explanations that can be tied to legislative justifications, the ability to deal with incomplete inputs, and the ability to calculate
the relevance of inputs to a given conclusion given incomplete information.

The approach set out involves
treating sections of legislation as "variables" in the OpenFisca encoding, and then duplicating the encoded logic to represent a separate confidence conclusion
for each variable. These two encodings are queried at the same time, and the results combined into a directed graph. The graph can then
be analysed to generate information about relevance, certainty, and to generate legislatively-justified explanations.

The paper briefly contrasts this approach with standard OpenFisca code, typical imperative code,
and declarative logic
encodings. The paper concludes that the additional effort
required to obtain these benefits inside OpenFisca, and
the increased risk to software quality, make the approach
ill-advised in most cases. The paper then argues for the development of open source Rules as Code technologies aimed
specifically at expert system development.

## Introduction

### Background

Under contract with Canada's Ministry of Employment and Social Development (ESDC), I work full-time as 
"Director of Rules as Code" inside
the Benefit Delivery Modernization (BDM) Programme of Service Canada. [Service Canada]() is the subdivision of ESDC responsible for the delivery
of many services to Canadians, both in person and online, including a number of important social benefit programs. One such benefit is Old Age Security.

The BDM programme is an effort inside Service Canada to modernize and digitize the delivery of benefits services to Canadians. 
It takes an agile and user-research-focussed approach to redeveloping how services are delivered online. My role
in that programme is to find, learn, experiment with, and build open source technologies for Rules as Code to explore its potential as
a foundational technology for the future of digital public service delivery.

As part of that task, I have used an open source Rules as Code tool, [OpenFisca](https://openfisca.org), to encode provisions of the [Old Age Security Act](),
in order to test the feasibility of using that code as a legal reasoning resource for a user-facing expert system. This paper discusses the
requirements of that project, and the experience of satisfying those requirements using OpenFisca, which is not designed for powering
expert systems.

### Rules as Code

Briefly, "Rules as Code" is a proposed public sector methodology that is intended to improve
the policy development process, the legislative drafting process, service provision, and
to simplify compliance. It calls for the digitization of the semantic meaning of legislation
and regulation as early as possible in its lifespan, in an interdisciplinary way that includes
legal, subject matter, and computing experts.

It emerged from work that was undertaken in New Zealand, and has seen increasing interest,
particularly in commonwealth countries, over the course of the last few years. Recently, the
Organization for Economic Cooperation and Developemnt's Observatory for Public Sector Innovation
released a report on Rules as Code entitled "Cracking the Code."
[The executive summary of that report]() is a convenient resource for people interested in learning more. There is also an open forum for people interested in Rules as Code available at 
[talk.rulesascode.com](https://talk.rulesascode.com).

Technologically, Rules as Code can be distinguished from existing practices because it focuses
on isolating the knowledge contained in the legal rules in a way that is re-usable and application
independent. The same encoding of the rules is ideally able to answer any question to which
the rules apply. It is the re-usability of this single encoding across policy development,
legislative drafting, service delivery, and compliance tasks that makes it an attractive
tool for the public sector.

Because the primary thing being modeled is the legislation itself, there is also the opportunity
to encode that legislation in ways that makes the software capable of generating justifications
for its conclusions. Those justifications can refer to the source legal material on which
the code was based, enhancing the transparency and trustworthiness of the resulting systems.

### Expert Systems

The term "expert system" is one that has changed in usage over time. During the rule-based artificial intelligence wave of the 1980s, an
"expert system" referred to a tool that encoded the knowledge of an expert, and had two primary capabilities:
* if it had the required facts, it could calculate an answer to a query based on the encoded knowledge, and
* if it did not have the required facts, it could determine what facts were relevant, based on the same encoding.

These systems were used to build applications, typically written in declarative logic languages, that asked the user one question at a time, and provided the
user with an answer when enough information was known. Many also had the ability to justify how the system had reached
the conclusions using the same encoded knowledge. Because many expert systems encoded knowledge that was heuristic,
or used to guide the interview process, the justifications were not viewed as terribly useful to end-users, and were
used primarily as tools for debugging the applications.

That "ask questions, give answers" interface has unfortunately been conflated with the technology that enabled it. 
As such, the phrase "expert system" now commonly refers to things that have similar interfaces. It 
is frequently used to describe any system that collects relevant information from an end-user, 
particularly in a form, or an interview, and answers
a question based on the provided facts and an encoding of expert knowledge, regardless of whether
the same encoding was used to determine relevance as to calculate the answer, or whether relevance
was calculated at all.

I find this change in usage unhelpful. By this revised definition, the difference between a function and an expert system is
that a function is not typically end-user facing.

In this paper, "expert system" is used to refer not to a user-facing application, but 
to a back-end service that is capable of answering questions, justifying those answers, and
answering questions about what inputs are relevant to legal questions, all on the basis of
the same encoding.

While expert systems of the past were typically created 
using declarative logic programming approaches, the
underlying technology can be abstracted away from the capabilities of the resulting tool. From the perspective of a
user sending a legal query to a web API endpoint, the language being used by the server to calculate the response is
irrelevant. The definition of "expert system" used in this paper therefore does not require declarative logic programming. Whether
declarative logic programming is an advisable approach to building legal expert systems is addressed addressed below.

### The Eligibility Estimator Prototype Requirements

The Eligibility Estimator is a prototype web application under development in BDM. It is a web application that collects relevant
information from a user, and provides information about whether a user qualifies for four benefits implemented under
the Old Age Security Act, why or why not, and if so, in what amount. The team developing this app are using
industry standard approaches to encoding the legislative requirements in order to provide those capabilities.
My task was to parallel that approach by encoding the relevant provisions of the Old Age Security Act in a "Rules as Code" way, to examine whether that would work as a feasible alternative reasoning tool for this particular application.

The objectives of my alternative encoding were as follows:
* A single encoding that answers all the relevant questions,
* available over a simple Web API,
* capable of explaining its answers,
* capable of responding to queries with inadequate facts, and advise which additional facts are relevant,
* capable of answering questions with uncertain facts that it is not possible to collect, to be able to advise clients
  that the answer to their question is contingent on those additional pieces of information,
* capable of answering questions as to the eligibility (whether or not the user qualifies) and entitlement (how much
  the user qualifies for) with regard to the four benefits outlined in the OAS Act, and
* uses a minimal set of inputs, defined by the application developers.

While that would be sufficient to demonstrate the usability of the encoding from the perspective of the app
developers, the purpose of the experiment was to examine the viability of using Rules as Code for that purpose.
Rules as Code calls for the encoding of legislation directly, so as to allow for legally-justified explanations
for the conclusions of the system, and for re-use.

In order to test whether the resulting encoding was capable of achieving those objectives also, I
set the following additional objectives for the experiment:
* Explanations should make reference to the legislative authority, and
* all of the answers (conclusions, additional relevant questions, etc.) should be generated from the same knowledge 
  representation of the law.

Note that we might have aimed at encoding the entire OAS Act, as a demonstration that an encoding of the legislation
generally was suitable for the application's specific needs. But this would have made for a considerably larger project. As such, only portions of the OAS Act directly relevant to the eligibility and entitlement questions dealt with
by the app were included in the encoding.

## OpenFisca

In order to understand what OpenFisca is, and how it works, it's necessary to take a step back and describe the
problem that it is designed to solve, which is microsimulation.

### Microsimulation

Simulations are often done in government to predict the possible future states of the economy. Consider, for example, the need
for government to analyse how its tax revenues will change in the face of the looming retirement of a large percentage
of their population. Understanding the scale of the problem, and when it is likely to arrive, is something that
can be aided by simulating the economy.

These simulations can be broken down into two categories, macrosimulation, or microsimulation. In macrosimulation,
the things that are represented in the model are abstract macroeconomic indicators, such as inflation rates or gross
domestic product. Rules are applied for how these macroeconomic indicators relate to one another, and how they are
expected to change, and the simulation is run to see what outcomes the model will predict.

Microsimulation, by contrast, models the economy by simulating smaller parts such as individuals, families, households, and firms.
Again, rules are set out that describe how these facts are anticipated to change over time, and how the various
facts relate to one another. The simulation is then run, and the future state of the simulated population is
analysed to learn what might happen.

Microsimulation is used to analyse the effect of tax and benefit rules, and the effects of proposed changes to those
rules. Information about the population and how it is expected to change is included, as are rules about the taxes
and benefits that apply to that population. The microsimulation can then
be run with various rule encodings to see what the effect of those policies might be on individuals, or 
in aggregate.

In some jurisdictions, this sort of analysis is a legal prerequisite for the adoption of tax and benefit legislation.
Microsimulation provides a convenient way to meet those legal prerequisites. There are also some legal requirements, 
such as maximum marginal tax rates, that are difficult to calculate across
a wide variety of fact scenarios. Using a microsimulation with randomly generated populations of sufficient size
can provide comfort that a proposed legislative change does not violate those sorts of legal constraints.

Microsimulation poses some particular technological challenges:
* it requires a large number of calculations to be applied to a large number of entities, repeatedly;
* the more calculations you can perform, and the faster you can do it, the more accurate your simulation may be;
* it needs to be able to deal with how populations and rules change over time; and
* it needs to be able to compare the results of different versions of the same rules.

The target users of microsimulations are usually data scientists. However, any system that is capable of calculating
the effect of a tax or benefit policy change on tens of thousands of people is also capable of calculating the effect
with regard to one person. Code generated for microsimulation is therefore also re-usable to generate
user-facing applications that help people understand the effects of existing and proposed rules.

### Open Source Microsimulation in OpenFisca

OpenFisca is an open source platform, built in the Python programming language, for generating microsimulations of
national tax and benefit systems. It has a number of features that are designed to solve some of the technical
challenges associated with microsimulation, and to do so in an open source context so as to promote
transparency and accountability.

In order to ensure that OpenFisca encodings are capable of processing large quantities of data as quickly as
possible, OpenFisca uses vectorial computing. All variables are presumed to be an arbitrarily long list of
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

OpenFisca has also been extended by other projects. Noteworthy is the Legislation Explorer, which
creates a generic web interface allowing people to change inputs and parameters and make calculations, and the Policy Engine project, which looks to build more user-friendly interfaces
for the same thing on the basis of OpenFisca encodings.

OpenFisca has been used to power tools such as Mes Aides and LexImpact in France, Les meves adjudes in Barcelona, Rapu Ture in New Zealand.

OpenFisca organizes its code by country, and open source country packages with varying levels
of quality and coverage have been created for France, Italy, Mali, Aotearoa New Zealand, Senegal,
Tunisia, the United Kingdom, and Uruguay, and sub-national areas like Barcelona and New South
Wales. Work is underway by the developers of Policy Engine on a United States package, also.

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
* good documentation, and 
* the availability of support from the helpful and growing OpenFisca community online.

At the time of writing, I am aware of no preferable alternatives given these objectives.

Had there been viable alternatives, red tape would have been a significant factor.
Obtaining approval to use new software technologies in ESDC is a time-consuming, complicated, and unpredictable process that moves
at what used to be called a "glacial" pace, but given the realities of climate change is probably better now described as "tectonic." 

OpenFisca, as a tool that had already seen considerable use in the department, and as a library for
an already-approved interpreted programming language (i.e. Python), would have had an almost insurmountable advantage over any tools
without those features in the context of any short-term project.

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

### Age, Birth Date, Minor

As an example, let's assume that there is a law that defines a minor as someone under the age of 18. You want to build an application that
can accept a birth date, and calculate whether the person is a minor.

The first step is to set out the relevant entities. In this case, the only entity type we are concerned with is "Person."

```python
# This is the code for building a person entity

Person = build_entity(
    key = "person",
    plural = "persons",
    label = "An individual. The minimal legal entity on which a legislation might be applied.",
    doc = """
    ...
    """,
    is_person = True,
    )

```

Now that we have defined the entity, we need to define three variables: `birth_date` will hold information about what day the person was born,
`age` will hold information about their current age, and `is_minor` will hold information about whether they are legally a minor. Here is how `age` and `birth_date`
variables would be defined.

```python
class birth_date(Variable):
    value_type = date
    entity = Person
    label = "Birth date"
    definition_period = ETERNITY  # This variable cannot change over time.

class age(Variable):
    value_type = int
    entity = Person
    definition_period = DAY
    label = "Person's age (in years)"

    def formula(person, period, _parameters):
        """
        ...
        """
        birth = person("birth_date", period)
        birth_year = birth.astype("datetime64[Y]").astype(int) + 1970
        birth_month = birth.astype("datetime64[M]").astype(int) % 12 + 1
        birth_day = (birth - birth.astype("datetime64[M]") + 1).astype(int)

        is_birthday_past = (birth_month < period.start.month) + (birth_month == period.start.month) * (birth_day <= period.start.day)

        return (period.start.year - birth_year) - where(is_birthday_past, 0, 1)
```
Note that the encoding of `age` specifies that the relevant time period is "DAY", whereas the encoding of `birth_date` says that the relevant time
period is "ETERNITY".  This is how you explain to OpenFisca that a person has only one `birth_date`, and it never changes, but their `age` might
be different on one day from the day previous. Similarly, whether or not they are a minor depends on their age, so it can also change from day
to day. The definition of `birth_date` has no formula, which means that for OpenFisca to know anything about a persons' `birth_date` it will
need to be provided by the user.  On the other had `age` has a formula which can be used to calculate the person's age based on their `birth_date`.

```python
class is_minor(Variable):
    value_type = bool
    entity = Person
    definition_period = DAY
    label = "Whether the person is a minor"

    def formula(person, period, parameters):
      age = person('age',period)
      age_of_majority = parameters(period).age_of_majority
      of_age = age_of_majority <= age
      return where(of_age, False, True)
```

This example is written to demonstrate how OpenFisca code differs from standard Python.
We  use the NumPy function `where` rather than the conditional `if` because the variables
`age`, `age_of_majority`, and `of_age` are all vectors, being calculated on the basis of other
vectors. Had we used `return not of_age`, the formula would always return `False`, because
`not` does not reverse the values inside a vector, it just uses the vector to generate a
single truth value and reverses it.

In our formula for calculating whether or not the person is a minor, we make reference to a parameter, the `age_of_majority`.  That parameter
is defined elsewhere, in a YAML file, as follows.
```yaml
# The Age of Majority
description: Age of Majority (in years). 
metadata:
  unit: year
values:
  1971-07-01:
    value: 18
```
With these encodings, we can deploy our code to an API. OpenFisca greatly simplifies the task of generating an API
capable of handling these queries. Once the encoding is complete, a web server with the relevant end points can
be started by executing the `openfisca serve` command. Once the API server is running, 
any user can send a POST query to the `/calculate` end point with a query in the
following format.
```json
{
  "persons": {
    "person1": {
      "birth_date": {
        "ETERNITY": "2000-04-01"
      },
      "age": {
        "2022-02-17": null
      }
    }
  }
}
```
Facts that are known are specified, including the period for which they are known. Queries are expressed by including
facts and the date for which you would like to derive them, and setting their value to `null`.

If the user sends the above query to the OpenFisca API `/calculate` endpoint, they receive the following response.
```json
{
  "persons": {
    "person1": {
      "birth_date": {
        "ETERNITY": "2000-04-01"
      },
      "age": {
        "2022-02-17": 21
      }
    }
  }
}
```

If we sent an under-specified query, asking for age and not providing birth date, like this:
```json
{
  "persons": {
    "person1": {
      "age": {
        "2022-02-17": null
      }
    }
  }
}
```
we receive this as a response:
```json
{
  "persons": {
    "person1": {
      "age": {
        "2022-02-17": 52
      }
    }
  }
}
```

This shows an important feature of OpenFisca for expert system development. If a value is not provided for a variable (in this case 'birth_date')
and that value cannot be calculated, OpenFisca treats that value as though it's numerical representation were
zero. If the value is boolean, OpenFisca will presume it is false. If the value is a date, OpenFisca will
presume the date is January 1, 1970. If it is a number, OpenFisca will presume it is zero, and if it is a string,
OpenFisca will presume it is an empty string.

Here, OpenFisca has treated no information for the user's birth date as "January 1, 1970", and calculated
that the person is 52 years of age.

## Using OpenFisca to Power Expert Systems

OpenFisca provides users with a way to calculate the answers to multiple questions, and to expose legislative encodings over an API.
It does not explain its answers, or link the explanations to legislative source material for the encoding.
It also doesn't have anyway to distinguish between "zero," and "unknown." That in turn means that it can't even tell when it
hasn't been provided with all the relevant information, what other information might be relevant, or what facts a conclusion is
contingent on.

All of those features can be obtained in the following way.

### Using `/trace` Results

OpenFisca provides an alternative endpoint called `/trace`, which is intended for use in debugging.

Trace output includes the same answers as provided by `/calculate`, but with additional information as
to all of the variables that were calculated, and for each variable, the other variables and parameters
on which that calculation was dependent. This information is provided as a dictionary of variables, with
their dependencies and calculated values listed for each entity in the query.

The approach we use is to change our OpenFisca encoding so as to get the required information into the trace
output, and then to post-process the trace output in order to retrieve it.

### Legislative Parts, Sub-Parts, Conditions, and Conclusions are all "Variables"

In order to generate explanations that are linked to legislative source material, we need the source material
to be represented in the trace output. That requires implementing the parts of the legislation as OpenFisca
variables.

In our example, a simple depedency tree explaining how `is_minor` is calculated would look like this:
```text
is_minor
 - age
```

There are two problems with this tree. The first is that it doesn't make any reference to legislative materials.
That can be resolved by renaming `is_minor` to something like `is_minor_according_to_majority_act_s_1`. But if
we did that, we would know that the variable `age` mattered in some way to making that determination, but we
would have no idea how. We can solve that problem by adding another variable between them that explains how the elements were being used.
```text
is_minor_according_to_majority_act_s_1
  - age_is_below_age_of_majority
    - age
```
Note that in our example, there is only
one statutory explanation possible for why a person is a minor. Often, that will not be the case, and adding legislative elements will also
require the addition of additional layers of variables. For instance, if being a minor could be legally inferred either due to the majority
act or a second piece of legislation, we would need variables for `is_minor`, `is_minor_according_to_majority_act_s1`, `is_minor_according_to_other_act_s1`,
`age_is_below_age_of_majority`, and `age`, along with whatever other variables are used by the other act.

The generalized strategy is that sections of legislation, their sub-sections, their logical conditions, and their conclusions (when the conclusions are not identical to the legislative section), are all implemented as variables
in the OpenFisca encoding.

You 
may also need to create variables to represent unnamed sub-sections of a named section of law when
knowing whether and how those unnamed sub-sections applied is relevant to the user.

Consider this example from the Old Age Security Act, section 3(1)(b)(iii):

>3 (1) Subject to this Act and the regulations, a full monthly pension may be paid to ...  
> (b) every person who ...  
> (iii) has resided in Canada for the ten years immediately preceding the day on which that person’s application is approved or, if that person has not so resided, has, after attaining eighteen years of age, been present in Canada prior to those ten years for an aggregate period at least equal to three times the aggregate periods of absence from Canada during those ten years, and has resided in Canada for at least one year immediately preceding the day on which that person’s application is approved; and

Inside OAS 3(1)(b)(iii) there is a way of qualifiying if the person has resided in Canada, and there
is a way of qualifying if the person has not. Encoding only whether section 3(1)(b)(iii) was satisfied 
may not provide adequate information about why a person did or did not qualify, and what they can
do to change the result, if anything.

So in addition to encoding numbered sections of legislation, it is also sometimes necessary to encode
unnamed sub-parts of legislative clauses as separate variables.

### Encoding Confidence

Once we have reformulated our encoding to add variables for all of the legislative parts, sub-parts, conditions, and conclusions, we are left with
the problem that OpenFisca doesn't actually know what it knows, because it does not know what it has been told, and what it is simply assuming.

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
class is_minor(Variable):
    value_type = bool
    entity = Person
    definition_period = DAY
    label = "Whether the person is a minor"

    def formula(person, period, parameters):
      age = person('age',period)
      age_of_majority = parameters(period).age_of_majority
      of_age = age_of_majority <= age
      return where(of_age, False, True)

class is_minor_known(Variable):
    value_type = bool
    entity = Person
    definition_period = DAY
    label = "Whether it is known if the person is a minor"

    def formula(person, period, parameters):
      return person('age_known',period)
```

This structure gives the user the ability to say what the birth date is, and whether or not they are confident of that information. If they are not
confident in that information, whether they are a minor will be calculated in the usual way, that lack of confidence
is carried forward to other variables that depend on it.

Writing these formulas for the "known" variables takes some careful consideration.  Frequently, a variable will be calculated on a conjunction, or a disjunction, or a combination of the two. Take for example a situation where a person is entitled to a benefit if they are a minor, and a citizen. When is `entitled_to_a_benefit` "known?" It is known
as soon as any of the conjoined elements is both known and false, because at that point, the conjunction is also known to be false. It is also known if both values are
known, regardless whether they are true or false. This leads to formulas like the following, which is
taken from the encoding of the OAS Act:

```python
class allowance_age_requirement_satisfied_known(Variable):
  value_type = bool
  entity = Person
  definition_period = DAY
  label = "Whether it is known if the age requirement is satisfied for Allowance eligibility"

  def formula(person, period, parameters):
    # A conjunction is known if all the elements are known or any element in known false.
    min_false = person("allowance_age_requirement_minimum_satisfied_known", period) * not_(person("allowance_age_requirement_minimum_satisfied", period))
    cap_false = person("allowance_age_requirement_cap_satisfied_known", period) * not_(person("allowance_age_requirement_cap_satisfied", period))
    any_false = min_false + cap_false
    all_known = person("allowance_age_requirement_minimum_satisfied_known", period) * person("allowance_age_requirement_cap_satisfied_known", period)
    return any_false + 
```

Similarly disjunctions are known if any of the disjunctive elements are known true, or if all of the elements are known. The complexity of these "known" formulas
seems to grow exponentially with the number of variables referred to in the substantive variable, which acts as another motivator to break complicated variables
into simpler ones.

### Post-Processing the Results of OpenFisca Queries

Once we have made these changes to our encoding, we can send a request to the API with the relevant inputs and "known" variables. If something is presumed,
we set it to its presumed value, and set "known" to false. We query the substantive conclusion we are interested in, and the "known" equivalent for it, and
we send the request to the `/trace` endpoint, so that we will get the information from which we can build the explanation tree.

We then use the following algorithm to build a single dependency graph from the information in the response to both the substantive variable and the associated known variable.

1. Create a directed graph.
1. For each variable included in the response, add a node to the directed graph, with the value calculated.
1. For each variable referred to as a factor in calculating a variable in the response, create an edge from the conclusion to the factor.
1. For each variable in the graph, find the corresponding "known" variable for each node in the graph, and annotate the node with the value of the "known" variable.

The result is a graph of variables with names, calculated values, and true-or-false "known" values, linked by
their dependency relationships.

This graph can now be used to generate explanations, determine relevant inputs, and calculate whether answers are contingently true, in the following ways.

#### Generate An Explanation

To generate an explanation, generate a tree from the graph using the variable you are interested in as the root node. For each element in the tree, 
include the substantive explanation and whether it is known, and display it to the user.

Because OpenFisca's `\trace` includes information about which
were the query variables (variables that were set to `null` in the request), generating explanations can be done automatically for all query variables that are not "known" variables.

#### Determine Relevant Inputs

To determine what inputs are relevant, first, generate the explanation for the variable you are interested in. If the root variable is "known", there are no other relevant inputs.
If the root variable is not "known", find all leaf nodes that do not have a "known" ancestor. Those are the relevant inputs. Note that this method assumes that only
leaf nodes are being collected from the user. In a situation where you are happy to ask, for example, either the person's age, or their birth date, then the "relevant" inputs
are all the nodes with no known ancestor.

This provides the application with a list of relevant nodes, but doesn't advise the application on how to prioritize
displaying them to the user.

#### Answer Contingent Questions

To answer a contingent questions subject to assumptions, first, determine the relevant inputs for the target query. Then get the list of variables that your application can collect from the user.
If none of the relevant inputs are inputs that your application can collect, the
answer provided in the explanation is contingent on the relevant inputs' assumed values being correct.


### Old Age Security Act Example

The approach described above was used to encode portions of the Old Age Security Act. That encoding
is available at [https://github.com/JasonMorrisSC/openfisca-canada](https://github.com/JasonMorrisSC/openfisca-canada). At the time of writing, a demo server running that code is available at [http://offnet.canadacentral.cloudapp.azure.com](http://offnet.canadacentral.cloudapp.azure.com).

Below is an excerpt from that code, showing the formula for calculating whether the
age requirement for old age security is satisfied:

```python
class oas_eligible_age_requirement_satisfied(Variable):
  value_type = bool
  entity = Person
  definition_period = DAY
  label = "Whether the age requirement for OAS is satisfied"

  def formula(person, period, parameters):
    satisfied = person('oas_eligible__age_above_eligibility', period)
    return satisfied

class oas_eligible_age_requirement_satisfied_known(Variable):
  value_type = bool
  entity = Person
  definition_period = DAY
  label = "Whether we know if the age requirement for OAS is satisfied"

  def formula(person, period, parameters):
    known = person('oas_eligible__age_above_eligibility_known', period)
    return known
```

Note the verbosity of the above code, which for practical purposes, expresses only the concept
"a person satisfies the age requirement if their age is at or above the minimum." The boilerplate
code required is significant.

Code that uses the `/trace` endpoint of the OpenFisca serverand then performs the graph analysis
described above is available as a [Google Colab notebook](https://colab.research.google.com/drive/1MTSzJN-_vGt9py2rd4t6kWZZbyS-0vsV#scrollTo=5znmuALOCTco) and as a [GitHub Gist](https://gist.github.com/JasonMorrisSC/f044233b2641fd2dca991f6edefe89ef). Running that code inside Google Colab will
start a temporary server which acts as the middle layer between OpenFisca and a user-facing
application. The code announces an ngrok.io address over which it can be accessed. Note that this code is
also written to mimic - to the degree possible - the output being provided by the existing 
implementation of the Eligibility Estimator. The portion of the code that performs the graph
analysis described above is included here:

```python
# Post-Process the Results

  dependency_graph = nx.DiGraph()
  for (K,V) in response['trace'].items():
      if "_known<" not in K: # Exlclude _known variables, as they will be used to annotate
          dependency_graph.add_node(K.split('<')[0], value=V['value'])
          for target in V['dependencies']:
              dependency_graph.add_edge(K.split('<')[0],target.split('<')[0])
  for (K,V) in response['trace'].items():
      if "_known<" in K:
          related_node = K.split('_known')[0]
          dependency_graph.nodes[related_node]['known'] = V['value']

  incorrect_nodes = []
  for N in dependency_graph:
      # Not sure why this test is necessary, but duplicate nodes
      # with no value show up for root elements in the graph
      if 'value' in dependency_graph.nodes[N]:
        values = dependency_graph.nodes[N]['value']
        if 'known' in dependency_graph.nodes[N]:
          knowns = dependency_graph.nodes[N]['known']
        else: # Some output variables do not have associated "known" variables.
          knowns = [True] * len(values)
          dependency_graph.nodes[N]['known'] = knowns
        display = []
        for (value, known) in zip(values,knowns):
            display.append(value if known else "unknown, potentially " + str(value))
        dependency_graph.nodes[N]['display'] = display
      else:
        # Checking to see if this solves problems later for
        # incorrectly duplicated nodes.
        incorrect_nodes.append(N)

  for i in incorrect_nodes:
    dependency_graph.remove_node(i)
  
  # print(str(nx.readwrite.json_graph.adjacency_data(dependency_graph)))

  def generate_explanation(goal,entity):
      expl = {}
      if len(dependency_graph.adj[goal]):
        expl['reasons'] = []
      expl[goal] = dependency_graph.nodes[goal]['display'][entity]
      for neighbour in dependency_graph.adj[goal]:
          expl['reasons'].append(generate_explanation(neighbour,entity))
      return expl

  output = {}
  for benefit in ['OAS','GIS','Allowance','AFS']:
    output[benefit] = {}


  # This loop adds information to the output from the graph
  # It makes a number of assumptions, such as there is only ever one entity being
  # asked about.
  for goal in list(set([r.split('<')[0] for r in response['requestedCalculations']])):
    if "_known" not in goal:
        i = 0
        while i < len(dependency_graph.nodes[goal]['value']):
            relevant = []

            for node in descendants(dependency_graph,goal):
                if not descendants(dependency_graph,node): # Only leaf nodes
                    if not dependency_graph.nodes[node]['known'][i]:
                      known_parent = False
                      for path in all_simple_paths(dependency_graph,goal,node):
                        for parent in path:
                            if dependency_graph.nodes[parent]['known'][i] == True:
                                known_parent = True
                                break
                        if not known_parent:
                            relevant.append(node)
            
            # dependency_graph.nodes[goal]['relevant'] = list(set(relevant))
            
            

            unaskable = ['eligible_under_social_agreement']
            relevant_and_askable = False
            values = dependency_graph.nodes[goal]['value']
            conditionals = [False] * len(values)
            dependency_graph.nodes[goal]['conditional'] = conditionals
            if relevant:
              for q in relevant:
                if q not in unaskable:
                    relevant_and_askable = True
              if not relevant_and_askable and dependency_graph.nodes[goal]['known'][i] == False:
                dependency_graph.nodes[goal]['conditional'][i] = True
                
            explanation = generate_explanation(goal,i)
```

The explanations generated are not used in the front end directly,
but are used to generate short explanations that are displayed to the user, in an effort to duplicate
the behaviour of the existing implementation.

A thin example of a front-end application accessing that middle layer is also available as a [Google
Colab Notebook](https://colab.research.google.com/drive/1FAwqjLRq2Ax7Y7-QextVNDeJkD01OV_W#scrollTo=L1JPR0sur8bn) and [GitHub Gist](https://gist.github.com/JasonMorrisSC/11883ddbb36c0b0b9981a903c644179e). Providing that code with the announced ngrok.io
address for the middleware will allow you to make queries against middle layer, and display
the results received. A video illustrating how a conversation
might proceed between the front end of the application and the OpenFisca reasoner, is available on
YouTube, and included here.

TODO: YouTube Video

If you provide only the age of 65, the server responds, with regard to OAS Eligibility, 
"More Information Required", and provides a list of relevant inputs including whether the person is
eligible under a social agreement with a different country, and where the person lives.

If you then add information about the person's place of residence being Canada, and rerun the query,
the answer remains
unknown, but agreements with different countries are no longer relevant.

If you provide a legal
status of "Canadian Citizen", income of 10000, and duration of residence in Canada of 20, the
response is that the person is eligible.

If you then change the place of residence to Greece,
the person is reported as "conditionally eligible", depending on the one remaining relevant input of
whether they qualify under the agreement with Greece.

## Analysis of Using OpenFisca for Expert System Development

Dealing with OpenFisca's vectorization of calculations is awkward. While being familiar with Python is
an advantage, it is not as strong an advantage as you would expect unless you are also familiar with
the mathematical library NumPy.
Standard Python keywords like `and`, `or`, and `if` do not work as you would naively assume
when dealing with vectors, which makes the task of writing encodings in OpenFisca challenging.
The need to always annotate input and output data and parameters with dates is also a frustrating
but understandable annoyance. I anticipate these things would fade into the background with time spent
working with OpenFisca code. Those are also problems that exist before applying the approach described
above.

Overall, my impression of having used OpenFisca to power a legal expert system was disappointing.
While the approach described above satisfied the requirements of
the experiment, but my impression was that the above approach resulted in code that
was long, repetitive, riskier and less useful than code written in other approaches. Expert
systems is not what OpenFisca is for, and putting it to that use causes unnecessary friction.

### Rules as Code v Application Development

Recall that application development and Rules as Code
are not the same thing. Analyzing whether or not OpenFisca, or any particular approach to the
use of OpenFisca, is an appropriate tool for Rules as Code generally is a very different question
from whether it is a good approach to expert systems.

In fact, analyzing Rules as Code in the context of any single application is counter-productive.
Encoding an entire piece of legislation so that it can be used both by your application
and arbitrary others is, in economic terms, a public good. The benefits received by the other
users of your encoded law is a positive externality that imposes costs on the current
development project that are impossible to recoup. And the work involved in writing code that
can do many things is necessarily higher than the work involved in writing code that can do
only the thing you currently need.

So if you are approaching it from the perspective of a person who needs to get a single product
out the door as effectively and efficiently as possible, Rules as Code is always going to appear
to make things harder, not easier.

That is not the right context from which to approach Rules as Code generally.
Rules as Code is public administration infrastructure.
Tools and techniques for Rules as Code need to be evaluated in that larger context.

Therefore, the question that this
paper seeks to address is not whether or not we should "do" Rules as Code, or whether we
should use OpenFisca to do it.

Rather, the question this paper seeks to address is assuming that we have
decided to use a Rules as Code approach to expert system development; and that we
want the resulting system to be explainable, justified by reference to
the source legislation, capable of understanding incomplete facts, and advising as to relevance;
how does OpenFisca compare to the alternatives.

### OpenFisca Exists

The main advantage that OpenFisca provides is that it is real. The alternatives, at least with regard
to open source technologies,
are not yet deployable. They are works in progress, or they do not have the documentation and tooling and community
that has built around OpenFisca, or they are hampered in their growth because they require the
adoption of a declarative or functional programming paradigm that is unfamiliar to most software
developers.

Closed source technologies are undesireable in the Rules as Code realm because of the
requirements for transparency and accountability arising in admininstrative law, and the move
of public sectors away from proprietary software solutions.

But good open source alternatives to OpenFisca for expert system development 
can nevertheless be anticipated. Work on tools like L4, Blawx,
and Catala continues, and is very promising. 
An apples-to-apples comparison of OpenFisca and some
of these alternatives applied to the same computational law
task would be worthwhile, but remains future work.

### OpenFisca Requires Verbose Code

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
the application would need to be astronomically complicated, or the encoding of the law 
*would need to already exist*. The latter is more realistic, and that's the approach I think we should pursue.

With regard to the approach described above there is no way to avoid the conclusion that using OpenFisca
this way involves writing an enormous amount of code, much of which is boilerplate, and repetitive. In particular, the need to duplicate the logic of the encoding in order
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

### OpenFisca Is Efficient When Dealing with Large Data Sets, and Variables Over Time

It must be mentioned that OpenFisca excels at solving the problems that it was specifically designed
to solve. If you need to deal with variables and legislative parameters that vary over time,
and if you need to be able to process the same conclusions with regard to a large number
of entities at the same time, OpenFisca provides excellent support for those capabilities.

I understand that OpenFisca also has capabilities with regard to generating representative
population data, which it was not necessary to explore in my work, but I anticipate those
are equally effective.

### OpenFisca Lacks the Reasoning Capabilities of Declarative Methods

OpenFisca is based on Python, which is imperative. Variables are calculated using formulas,
which are imperative functions. Imperative functions go in only one direction, from inputs to outputs, and cannot be automatically reversed.

Declarative logic programming, by contrast, allows you to specify relationships, and then use them
in both directions.

By way of illustration, an equivalent declarative logic programming encoding of the OAS rules might allow for asking
abstract reasoning questions, like "are there any circumstances in which a person qualifies for GIS and
lives outside of Canada?" The logic required to answer that question starts at the conclusion - whether the person qualifies for GIS - and works backward to the inputs that can lead to that conclusion.

With the OpenFisca approach described above, the only way to get an answer to that question would be to
randomly generate a large population (which OpenFisca can help with), run the queries against all the
entities in that population, and test the results to see whether or not the described situation arises.
But even then, you don't know that it is impossible, you just know you didn't find any examples of it.

A declarative logic encoding might be capable of saying categorically that such a scenario is impossible,
or determining that it is possible, and providing an example, without having to generate any data or
analyse the output of any queries.

So while OpenFisca code can be written in such a way as to get some of the features of declarative code
alternatives, and the above method demonstrates that possibility, that method still does not get you
all the capabilities of a declarative encoding of the same rules, which encoding would likely be
dramatically shorter.

### This Approach Is Error Prone

In addition to the cost of additional lines of code, there is a negative consequence for 
software reliability. Every line of code is an opportunity for another bug. Given the complexity
of the formula used for calculating "known" values in the above method, we are caught between
a rock and a hard place. Either we simplify the variables to the point where the formulas for
whether or not they are known are simple, and we multiply the number of variables we are encoding,
or we leave ourselves with more complicated variable definitions and significantly increase the risk
of errors in how the "known" variables are being calculated.

## Conclusion

Open Fisca clearly belongs in Rules as Code toolbox. If you need microsimulation,
it is an excellent option. If you need microsimulation, and you don't need things like explanations, legal
justification, relevance, and dealing with incomplete facts, it is also a reasonable way to build simple web apps.

My experience also suggests that if you do need those additional features, or if you are trying to build a generalized encoding
for people who might, OpenFisca is suboptimal. It can be made to behave sort of like
an expert system, but the effort and risk involved is high.

This paper does not address the alternative
of attempting to make declarative expert system code behave like a microsimulator. My instinct
is that these would be result in relatively inefficient code if done with
declarative logic approaches. Functional approaches seem well-suited to the microsimulation task,
but I have no direct experience with them, and it is not clear whether they 
share all the advantages of logical code.

What seems obvious to me from the experience of having used both OpenFisca and declarative methods to generate explainable legal expert systems is that
microsimulation and declarative logic tools are more powerful in combination than either is alone.

With both, you could use the declarative encoding with subject matter experts to verify it matches their expectations.
That is arguably easier to do with declarative code, because declarative code matches the structure of the source rules much better than imperative approaches. Then,
the declarative encoding could be used to generate tests for microsimulation projects. If the microsimulation
code passes all the of the tests generated by the declarative code, then we can have confidence it is consistent with the declarative code,
which in turn is consistent with the experts' expectations. If a test fails, the declarative code can be used to
generate an explanation for why the result that was generated was not correct, and why the result that was expected is
correct.

This multi-layered approach would be a boon to legal software quality assurance, allowing you do add legal verifiability
to the list of features provided by whatever other technology you are using. Microsimulations would
remain as efficient as you like, but we could also have greater confidence that they were legally accurate.

Ultimately, the lesson from this experiment is to choose tools on the basis of whether the problem
you are trying to solve is the problem they were designed to address. We ought not cram microsimulator-shaped pegs into expert-system-shaped holes. OpenFisca should be used for what it is
good at, which is microsimulation. We should continue and wherever possible invest in the 
work on developing and improving open source tools for the problem of expert systems.

## Acknowledgements

I would like to thank the maintainers of OpenFisca for their valuable and important tool.
I would like to thank the members of the OpenFisca community who have and continue to assist me in figuring out how to
get OpenFisca to do things it wasn't designed to do. I would also like to thank the members of the Eligibility Estimator
team for their help in understanding their use case and their needs, and for having done the work of encoding the status
quo version before I even got started. I would also like to thank the organizers of the Computational Legal Studies
Conference for hosting the conference, supporting the academic field of Computational Law, and creating forums for conversations
around topics like Rules as Code.
