---
toc: true
title: Using OpenFisca as an Expert System Engine
description: >-
  At Service Canada we are working on a new tool to help Canadians to plan for
  retirement. And we are experimenting with implementing that…
date: '2021-12-08T19:29:44.580Z'
categories: []
keywords: [openfisca]
slug: /@roundtablelaw/using-openfisca-as-an-expert-system-engine-e291ddc50815
---

At Service Canada we are working on a new tool to help Canadians to plan for retirement. And we are experimenting with implementing that system using a Rules as Code approach. Specifically, we are looking at implementing the rules in [OpenFisca](https://openfisca.org).

OpenFisca is a leading Rules as Code solution, but it is primarily aimed at microsimulation. Microsimulation is where you create a data model that represents small elements of a population, like people and households, and then encode the effects of different tax and benefit regimes. You can then load data that is representative of your actual population, and look at the effects on those individuals or groups in aggregate. Or, you can load a smaller number of specific examples and see how those particular fact scenarios are affected by the rules.

In either case, the expected workflow is that the information is provided at the beginning, and OpenFisca does a calculation, and returns the result.

An expert system, if you are using the term as it was used in the 80s and 90s, is a system that has a thing that is it trying to figure out, and can figure out what questions to ask based on the rules and the information that has already been provided. That process operates differently. It anticipates that the data will be given to the reasoner one piece at a time, and after each piece of data is given, the reasoner will advise whether it knows the answer to the question, and if not, what other questions are relevance.

### The Plan

In order to give OpenFisca something like that capability, my plan is to give OpenFisca the concept of uncertainty. By default, OpenFisca treats variables that it hasn’t been given as “false-ish”. Booleans will be false, integers will be zero, and strings will be blank, for example. That means that OpenFisca can’t really tell the difference between having been told that an input variable’s true value is zero, and not having been given a value.

In order to give OpenFisca the concept of uncertainty, I’m going to take the existing list of variables, and duplicate them, creating a separate boolean variable for whether or not the first variable is “known.” For input variables, the user will specify that the variable is known in the data submitted to OpenFisca.

Then, the user will be able to ask for the result they are interested in, and they will also be able to ask whether that result is known. If they use the `/trace` endpoint for the OpenFisca server, they will receive data that can be turned into a dependency tree, showing how each part of the calculation depended on other parts.

Then, if we break out the conclusions of all the logical operations into their own variables, so the conclusion of each “and”, “or”, or comparison becomes its own variable that may or may not be known, the dependency tree for whether or not the conclusion is “known” should have all the information we need to understand what questions are relevant.

If OpenFisca says that the root of that tree is “known”, then the question has been answered, and there are no more questions that need to be asked. Otherwise, all of the unknown leaf nodes that have only unknown nodes above them in the tree are still potentially relevant.

The resulting dependency trees will also be sufficient information from which to generate sophisticated user-facing explanations for the conclusions that are reached. I have a suspicion that they will actually contain considerably more information than a typical logical trace will provide, because it should be possible to determine if a conclusion might have been reached in more than one way based on the information provided.

### Some Caveats

#### Structural Isomorpshim

I’m a big fan of what I call “structural isomorphism.” The smallest possible parts of your encoding should have as close as possible to a one-to-one relationship with the smallest possible parts of the law.

For this experiment, I’m ignoring structural isomorphism, at least to the extent that the sections of code should be small. This approach requires a lot of code. I don’t think it breaks the one-to-one relationship, but I’ll find that out later and let you know. It certainly breaks the requirement that the pieces of code be as small as possible.

#### Multiple Queries

An acknowledged downside of this approach is that in order to get the information about whether the conclusion is known, or not, the user needs to send two queries to the OpenFisca API. One for the answer, and one for whether or not the answer is known. That’s less than elegant. There’s probably a way to solve that problem by having OpenFisca query itself, and then include the results in the response.

But we’re not there, yet.

#### Post-Processing

Another acknowledged downside of this approach is that it requires the client accessing the OpenFisca Web API to do post-processing on the results that it receives from the two queries. It does not simply output a list of variable names that are still relevant. It provides data on which you can run an algorithm, which will reveal that list.

#### Style Dependent

Another acknowledged downside of this approach is that it does not work on all OpenFisca code. The code needs to have been written in this particular way for that algorithm to work, and the application developer needs to know that the code has been written that way. That breaks the separation of concerns. The application developer ought not need to know how the code was implemented in order to be able to access the useful information. But that’s a natural result of the fact that uncertainty and explanations are not features of OpenFisca. We’re just trying to use OpenFisca in such a way as to achieve a similar effect.

### An Example

I’ll try to do this up as a Colab Notebook, so you can play along if you’re interested.

Let’s say we want to encode the rules for eligibility for the Guaranteed Income Supplement in OpenFisca. We might write a variable like this:

```python
class gis_eligible(Variable):  
  value_type = bool  
  entity = Person  
  definition_period = DAY  
  label = "Whether Person is eligible for Guaranteed Income Supplement"

def formula(person, period, parameters):  
    married = person('marital_status',period) == marital_status_options.MARRIED  
    common_law = person('marital_status',period) == marital_status_options.COMMONLAW  
    partnered = married + common_law  
    max_single = parameters(period).benefits.old_age_security.guaranteed_income_supplement.maximum_income_single  
    max_partner = parameters(period).benefits.old_age_security.guaranteed_income_supplement.maximum_income_partnered  
    max_both = parameters(period).benefits.old_age_security.guaranteed_income_supplement.maximum_income_two_recipients

max_income = where(partnered,max_partner,max_single)  
    max_income = where(person("partner_receiving_oas",period),max_both,max_income)  
    income_requirement = person("income",period.this_year) < max_income  
    age_requirement = person("age",period) >= parameters(period).benefits.old_age_security.eligibility_age

return person('oas_eligible',period) * income_requirement * age_requirement
```

In pseudo code, the `formula` function reads “a person is partnered if they are married or common law. The person’s maximum income depends on if they are sincle, and if their partner is receiving oas. If they are eligible for OAS, and they have less than the maximum income, and are at least as old as required, they are eligible for GIS.”

If we were to run a trace on how this variable is calculated, we would see a tree that looks like this:

```yaml
gis_eligible:  
  - marital_status  
  - partner_receiving_oas  
  - income  
  - age  
  - oas_eligible
```
That would be accurate, but it does not allow us to know whether `age` is still relevant when `oas_eligible` is false. That’s the kind of information we want to add to the output.

So what we are going to do is we are going to break this variable down into sub-variables, so as to get a tree that looks more like this:

```yaml
gis_eligible:  
  - income_is_eligible:  
    - income  
    - gis_eligible_income_max:  
      - marital_status  
      - partner_receiving_oas  
  - age_eligible:  
    - age  
  - oas_eligible
```
Now, if eligibility for GIS is not known, but we know that the income is eligible, we also know that we don’t have to ask about any of marital status, partner OAS, or income that have not already been collected. We also have the information required to be able to explain to the user why their maximum income was set at the level it was.

So where we had just gis_eligible, we now have four variables, encoded like this:

```python
class gis_eligible(Variable):  
  value_type = bool  
  entity = Person  
  definition_period = DAY  
  label = "Whether Person is eligible for Guaranteed Income Supplement"

def formula(person, period, parameters):  
    return person('oas_eligible',period) * person('gis_eligible_income',period) * person('gis_eligible_age', period)

class gis_eligible_age(Variable):  
  value_type = bool  
  entity = Person  
  definition_period = DAY  
  label = "Whether the person's age meets the requirements for Guaranteed Income Supplement"

def formula(person, period, parameters):  
    return person("age",period) >= parameters(period).benefits.old_age_security.eligibility_age

class gis_eligible_income(Variable):  
  value_type = bool  
  entity = Person  
  definition_period = DAY  
  label = "Whether the person's income meets the requirements for Guaranteed Income Supplement"

def formula(person, period, parameters):  
    income_requirement = person("income",period.this_year) < person("gis_eligible_income_max", period)  
    return

class gis_eligible_income_max(Variable):  
  value_type = int  
  entity = Person  
  definition_period = DAY  
  label = "The person's maximum income for GIS eligibility"

def formula(person, period, parameters):  
    married = person('marital_status',period) == marital_status_options.MARRIED  
    common_law = person('marital_status',period) == marital_status_options.COMMONLAW  
    partnered = married + common_law  
    max_single = parameters(period).benefits.old_age_security.guaranteed_income_supplement.maximum_income_single  
    max_partner = parameters(period).benefits.old_age_security.guaranteed_income_supplement.maximum_income_partnered  
    max_both = parameters(period).benefits.old_age_security.guaranteed_income_supplement.maximum_income_two_recipients

max_income = where(partnered,max_partner,max_single)  
    max_income = where(person("partner_receiving_oas",period),max_both,max_income)  
    return max_income
```

That’s a lot more code to generate the exact same answer. But it still isn’t capable of dealing with whether or not the conclusions are known. So now we’re going to create four new variables, with the name `{original_variable_name}_known`, so that we will be able to relate them to each other. And the formula for each will set out the circumstances in which the value of the variable is certain.

For example, whether or not the person qualifies for GIS is now a conjunction of three things: they are eligible for oas, they meet the age requirement, and they meet the income requirement. A conjunction is known, whether true or false, if all of its parts are known. But a conjunction is also known to be false if any of its parts are known to be false. So we encode `gis_eligible_known` as follows:

```python
class gis_eligible_known(Variable):  
  value_type = bool  
  entity = Person  
  definition_period = DAY  
  label = "Whether it is known if the Person is eligible for Guaranteed Income Supplement"

def formula(person, period, parameters):  
    all_known = person('oas_eligible_known',period) * person('gis_eligible_income_known', period) * person('gis_eligible_age_known', period)  
    oas_false = person('oas_eligible_known',period) * not_(person('oas_eligible',period))  
    income_false = person('gis_eligible_income_known', period) * not_(person('gis_eligible_income', period))  
    age_false = person('gis_eligible_age_known', period) * person('gis_eligible_age', period)  
    any_false = oas_false + income_false + age_false
```

Now OpenFisca has a way of determining whether or not it knows the answer to GIS eligibility with certainty, and can explain why. But to make it work, we need to create the “known” variables for all of the other intermediate conclusions, and “known” variables for all of the inputs like age and income, too.

So in this example, the code goes from one variable to eight variables, not counting the increases for the input variables, and all of the math that goes into determining whether or not the person is eligible for OAS.

### Output

If I run the code with an input where I provide only the person’s age, ask about GIS eligibility, and ask whether GIS eligibility is known, here is what the output looks like:
```
  gis_eligible<2021-12-08> >> [ True]  
    oas_eligible<2021-12-08> >> [ True]  
    gis_eligible_income<2021-12-08> >> [ True]  
      income<2021> >> [0]  
      gis_eligible_income_max<2021-12-08> >> [18216]  
        gis_eligible_income_max_partnered<2021-12-08> >> [False]  
          marital_status<2021-12-08> >> ['SINGLE']  
          marital_status<2021-12-08> >> ['SINGLE']  
        partner_receiving_oas<2021-12-08> >> [False]  
    gis_eligible_age<2021-12-08> >> [ True]  
      age<2021-12-08> >> [75]

  gis_eligible_known<2021-12-08> >> [False]  
    oas_eligible_known<2021-12-08> >> [False]  
    gis_eligible_income_known<2021-12-08> >> [False]  
      income_known<2021> >> [False]  
      gis_eligible_income_max_known<2021-12-08> >> [ True]  
        gis_eligible_income_max_partnered<2021-12-08> >> [False]  
        gis_eligible_income_max_partnered_known<2021-12-08> >> [ True]  
          marital_status_known<2021-12-08> >> [ True]  
        marital_status_known<2021-12-08> >> [ True]  
        partner_receiving_oas_known<2021-12-08> >> [False]  
    gis_eligible_age_known<2021-12-08> >> [ True]  
      age_known<2021-12-08> >> [ True]  
    oas_eligible_known<2021-12-08> >> [False]  
    oas_eligible<2021-12-08> >> [ True]  
    gis_eligible_income_known<2021-12-08> >> [False]  
    gis_eligible_income<2021-12-08> >> [ True]  
    gis_eligible_age_known<2021-12-08> >> [ True]  
    gis_eligible_age<2021-12-08> >> [ True]
```

As a small aside, the output above is taken from the Python API, not the Web API. You would need to regenerate these trees from the data returned by the `/trace` endpoint when using the Web API.

The first tree shows how GIS eligibility was calculated. It shows that it determined that the person qualifies by virtue of being 75 years of age. Because a person’s income is presumed to be zero if not provided, they also qualify under the income requirement. Their maximum income was set to $18,216.

The second tree shows us the corresponding true/false values for whether or not the conclusions in the top tree are known. So we can check to see if “income_max”, which was set to $18,216, is actually known, and we can see that it is.

From the top tree we can see that it depends on whether the person is partnered, and whether the person’s partner is receiving OAS. From the bottom tree we can see that we know the person is not partnered, and that we do not know whether their partner is receiving OAS. But because whether their partner is receiving OAS is a dependency only of values in the top tree that are already known, we know it isn’t relevant for asking the question at the bottom of that tree.

The relevant questions to ask are the ones that are unknown, and are descendent only of unknown things above them in the dependency tree.

### Take Aways

This approach is deeply suboptimal, just because OpenFisca isn’t designed for powering expert systems. That’s not a critique of OpenFisca. A wrench makes a lousy hammer, and that’s not a critique of the design of wrenches.

But the approach shows that at least some of what you need can be achieved in OpenFisca. But the increase in the amount of boilerplate code you need to write is really high, and I think it’s reasonable to assume that it increases the risk of errors. It is also deeply non-standard, so tools that rely on understanding the implications of OpenFisca’s definition of a parameter and a variable may not be able to use this approach.

One benefit is that if you don’t need the “known” variables, you can run your calculations without them. So at that point the only thing slowing things down is having divided single variables into multiple variables.

OpenFisca could be made to support this sort of approach more easily. We could add features for uncertainty of variables, or syntactic sugar for making it easier to build complicated variable trees. You could also add endpoints to the Web API when what you need is the answer and the certainty, and the justification tree all at once, for example. We could also move some of what is currently post-processing into a different end point, and have OpenFisca return the lists of relevant variables, if it integrated this approach more fully.

I’m not sure any of that is actually worth it, though. It depends on how much effort it would take to get equivalent capabilities out of other approaches, and whether those other approaches might also be able to output something that could be turned into OpenFisca code, or code for some other microsimulation tool.

Getting microsimulation to work off of encodings of laws is an important objective. It remains to be seen whether that means OpenFisca code should be a starting place, or a destination.