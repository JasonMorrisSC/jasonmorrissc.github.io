---
toc: true
title: Explainable OpenFisca
description: >-
  Following up on the plan I wrote about a few days ago… By exposing all of the
  intermediate conclusions, and by adding separate variables…
date: '2021-12-16T09:35:02.716Z'
categories: []
keywords: [openfisca]
slug: /@roundtablelaw/explainable-openfisca-4c718d0fdc58
---

Following up on [the plan](/post/roundtablelaw/using-openfisca-as-an-expert-system-engine-e291ddc50815) I wrote about a few days ago… By exposing all of the intermediate conclusions, and by adding separate variables that represent whether the primary variables are “known,” I’ve managed to create an encoding in OpenFisca that is capable of doing some cool stuff over the Web API.

You can see the source code in [this GitHub repository](https://github.com/JasonMorrisSC/openfisca-canada/tree/stub_version), and the `demos/explanations.py` file demonstrates the use of the OpenFisca Web API with my rules as code encoding in Python.

You can also try it in [this Google Colab notebook](https://colab.research.google.com/drive/1ievnP8CAgfWBqKQI2XXRz2ZqeWTWu7VL?usp=sharing).

#### Explanations

First, you can hit the `/trace` endpoint of the Web API, and query both the answer, and whether the answer is known, in one go. By combining the regular and “known” variables and their dependencies into a single dependency tree, we can generate a textual explanation for how OpenFisca reached its conclusions.

For one of the two example scenarios, the output looks like this:
```
oas_eligible as of 2021-12-01 is unknown, because  
  oas_eligible_income_requirement_satisfied as of 2021-12-01 is unknown, because  
    oas_eligible__income_not_above_limit as of 2021-12-01 is unknown, because  
      income as of 2021 is unknown, and  
  oas_eligible_age_requirement_satisfied as of 2021-12-01 is True, because  
    oas_eligible__age_above_eligibility as of 2021-12-01 is True, because  
      age as of 2021-12-01 is 65, and  
  oas_eligible_legal_status_satisfied as of 2021-12-01 is unknown, because  
    oas_eligible_legal_status__qualifies as of 2021-12-01 is unknown, because  
      legal_status as of 2021-12-01 is unknown, and  
  oas_eligible_required_residency_duration_satisfied as of 2021-12-01 is unknown, because  
    years_in_canada_since_18 as of 2021-12-01 is unknown, and  
    oas_eligible_required_residency_duration_amount as of 2021-12-01 is unknown, because  
      place_of_residence as of 2021-12-01 is unknown, and  
  oas_eligible_residency_requirement_satisfied as of 2021-12-01 is unknown, because  
    oas_eligible_canadian_residency_requirement_satisfied as of 2021-12-01 is unknown, because  
      place_of_residence as of 2021-12-01 is unknown, and  
    oas_eligible_foreign_residency_requirement_satisfied as of 2021-12-01 is unknown, because  
      resides_in_agreement_country as of 2021-12-01 is unknown, because  
        place_of_residence as of 2021-12-01 is unknown, and  
      eligible_under_social_agreement as of 2021-12-01 is unknown
```
That’s not as readable as you would want to make it before you displayed it to a user, but the point is that all the relevant information is there, and your interface can do with it as you please.

#### Relevance

By looking for leaf nodes in the justification tree that have only “unknown” parents, we are able to come up with the list of relevant questions to ask the user. For the same example, the demonstration generates the following output for relevance:
```
The remaining relevant inputs are ['place_of_residence<2021-12-01>', 'legal_status<2021-12-01>', 'eligible_under_social_agreement<2021-12-01>', 'years_in_canada_since_18<2021-12-01>', 'income<2021>']
```
That information can be used to avoid asking the user questions that can no longer possibly impact the question you are trying to answer, which makes the application smarter, with less work for the developers, and in particular without the developers needing to know the logic of the legislation in order to predict when things will matter.

#### Contingent Conclusions

Sometimes, the answer depends on things that you aren’t going to ask the user, because the interface would be too complicated, or it’s not reasonable to expect the user to know. If you know the list of questions that can’t be posed to the user, but are considered in the code, and the list of relevant questions is entirely in that category, you can advise the user that the answer is “it depends”, and describe what it depends on, and advise the user what to do next. For another example, the demonstration output looks like this:
```
The remaining relevant inputs are ['eligible_under_social_agreement<2021-12-01>']  
There are no askable relevant variables, so this goal is contingently known.
```
#### Why it Matters

We now have proof that it is possible to generate explainable expert systems on the basis of encodings of legislation done in OpenFisca, and accessed over the standard OpenFisca Web API.

We also have a much better idea of how difficult it is.

Next we will satisfy our front-end developers that the OpenFisca Web API is friendly to them. Then work begins on a version of the encoding that is based more directly on the relevant legislation, as opposed to someone’s short-hand interpretation of it.

That will teach us another important lesson, which is how much more difficult, if at all, is it to do an encoding in OpenFisca that has higher fidelity with the source legislation? By comparing the difficulty of the two approaches, and taking a look at the nature of some of the amendments that have been made to the legislation over time, and examining how those amendments would be made to the code in both versions, we should get some idea of whether the increased effort in encoding is worth it in terms of maintenance and the ability to tie explanations to the source legal material.

We are also already starting to explore techniques to keep the encoding in sync with legislative changes. The possibility of generating OpenFisca parameter files, [like this one](https://github.com/JasonMorrisSC/openfisca-canada/blob/stub_version/openfisca_canada/parameters/benefits/social_agreement_countries.yaml), by scraping government web pages [like this one](https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/payroll/payroll-deductions-contributions/canada-pension-plan-cpp/foreign-employees-employers/canada-s-social-agreements-other-countries.html), is on the agenda. As is the possibility of tracking changes to legislation through tools like [LexBox](https://sso.lexum.com/auth/realms/lexum/protocol/openid-connect/auth?response_type=code&client_id=lexbox-web&redirect_uri=https%3A%2F%2Fapp.mylexbox.com%2Fapi%2Fsso%2Flogin?realm%3Dlexum&state=93e6e22f-adcf-4228-96f7-8c431eae80d3&login=true&scope=openid). Perhaps when the law changes, we can create a GitHub issue with a link to the legislative changes on CanLII.