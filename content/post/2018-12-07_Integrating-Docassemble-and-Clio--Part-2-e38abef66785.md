---
toc: true
title: 'Integrating Docassemble and Clio: Part 2'
description: >-
  My ABA Innovation Fellowship project, through the ABA’s Centre for Innovation,
  is docassemble-openlcbr, an open-source tool to bring the…
date: '2018-12-07T22:31:00.891Z'
categories: []
keywords: []
slug: /@roundtablelaw/integrating-docassemble-and-clio-part-2-e38abef66785
---

My [ABA Innovation Fellowship](https://www.americanbar.org/groups/centers_commissions/center-for-innovation/Fellowships/20182019Fellows/) project, through the [ABA’s Centre for Innovation](https://www.americanbar.org/groups/centers_commissions/center-for-innovation/), is [docassemble-openlcbr](https://github.com/Gauntlet173/docassemble-openlcbr), an open-source tool to bring the power of analogical reasoning to [docassemble](https://docassemble.org), the leading open source legal expert system tool.

If you’d like some background, check out my [post on why I study expert systems](https://medium.com/@jason_90344/why-i-study-old-artificial-intelligence-in-law-d0abf23e42aa), and my post on [introducing analogical reasoning to docassemble](https://medium.com/@jason_90344/legal-expert-systems-just-got-smarter-e7e12b75e872).

[Clio](http://www.clio.com) is the leading cloud-based legal practice management software in North America. Clio has generously agreed to sponsor my fellowship, which has allowed me to expand the scope of project considerably, including by integrating it with Clio.

Today, I’m announcing the second part of the integration between docassemble-openlcbr and Clio. In the [first integration](https://medium.com/@jason_90344/integrating-docassemble-and-clio-6c511e30925), docassemble-openlcbr used the data in your Clio account as the test case-the fact scenario that you wanted to ask a legal question about. All the precedent cases were held in docassemble itself.

In this second version of the integration, docassemble-openlcbr can now compare one case in your Clio account against any number of other cases in your Clio account.

![](/1__wbm10RIinxHRAqgFGIP3MA.png)

#### How Does It Work?

On the Clio side, you create a set of [custom fields](https://support.clio.com/hc/en-us/categories/200962417-Custom-Fields) which matches the factors in your reasoner. If you’re curious how the reasoner works, check out [this deep dive post](https://medium.com/@jason_90344/automating-case-based-reasoning-by-analogy-a-deep-dive-a1b015f234dd).

It’s best if you put these fields in a [custom field set](https://support.clio.com/hc/en-us/articles/203139244-Custom-Fields-Creating-Contact-Matter-Custom-Fields-Sets#Custom%20Field%20Sets) so that they can be added to the matter all at once. As you can see in the image below, the fields are just a list of check-boxes that indicate whether or not something was true about that file, and a drop-down that indicates whether the issue in the reasoner was eventually decided for the plaintiff or the defendant.

![](/1__c40eT5aCZzlUZoNCVfwvWA.png)

Then, you need to connect your docassemble interview to your Clio account. Using the new [“bulk actions” features](https://app.clio.com/api/v4/documentation#tag/Bulk-Actions) of [Clio’s v4 API](https://app.clio.com/api/v4/documentation), the interview can then download all of the relevant custom fields from closed matters in a given practice area. In the demo, the practice area is “Trade Secrets”.

![](/1__JoNhQhvAqOtnNXQD4ALYGA.png)

Once downloaded, docassemble adds that data to the database of cases used by docassemble-openlcbr, and then the reasoner runs as usual.

#### Just Try It Already

[You can try the demo here.](https://testda.roundtablelaw.ca/interview?i=docassemble.clio%3Adata%2Fquestions%2Fclio_openlcbr_demo.yml)

#### So Why Does This Matter?

A couple of reasons.

Most artificial intelligence tools that predict legal outcomes require a database of cases to work from. Most will start by working with publicly-reported (and hopefully freely-available) cases.

But _reported_ case data is not most of the cases, or the right cases. Reported cases tend to go to trial and get reported for a bunch of reasons that are unrelated to how representative they are of the entire population of lawsuits.

But unreported decisions are… well, unreported. Which usually means unavailable.

Unavailable, that is, unless your law firm uses a practice management tool with API integration features like Clio. If it does, you can record the relevant factors about the matters your firm handles, and then use your own closed cases as the database for making predictions about future ones.

So the first benefit is that connecting to practice management tools improves the quantity and quality of the data that these sorts of AI tools have access to. All other things being equal, that ought to improve the quality of the predictions these tools can make.

Second, it gives forward-thinking law firms an opportunity to create value (automated case outcome predictions) for people at near-zero marginal cost, using assets they already own.