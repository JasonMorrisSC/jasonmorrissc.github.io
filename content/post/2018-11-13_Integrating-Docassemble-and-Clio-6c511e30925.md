---
toc: true
title: Integrating Docassemble and Clio
description: >-
  Clio is the leading cloud-based legal practice management tool in North
  America. Clio’s support of my Innovation Fellowship with the ABA…
date: '2018-11-13T18:10:30.263Z'
categories: []
keywords: []
slug: /@roundtablelaw/integrating-docassemble-and-clio-6c511e30925
---

[Clio](http://www.clio.com) is the leading cloud-based legal practice management tool in North America. Clio’s support of [my Innovation Fellowship with the ABA Centre for Innovation](https://medium.com/@jason_90344/why-i-study-old-artificial-intelligence-in-law-d0abf23e42aa) has given me the opportunity to expand the project in a couple of really interesting ways. Today, I’d like to share with you the first part of that expansion: making it possible to do analogical reasoning about case data held in your Clio account.

### docassemble-openlcbr

For background, my innovation fellowship project is to add the power of case-based reasoning by analogy provided by the [openlcbr](https://github.com/mgrabmair/openlcbr) tool to the open-source legal expert system and document automation tool [docassemble](https://docassemble.org). The result is an open-source expansion package for docassemble called [docassemble-openlcbr](https://github.com/Gauntlet173/docassemble-openlcbr).

So far in the project I have released a [live proof of concept site](https://medium.com/@jason_90344/legal-expert-systems-just-got-smarter-e7e12b75e872) that shows you what it would be like to use a tool doing case-based reasoning by analogy. I have also developed a website that demonstrates what an online interface for generating openlcbr databases would look like. If you’d like details on how the analogical algorithm works, check out [my deep-dive post](https://medium.com/@jason_90344/automating-case-based-reasoning-by-analogy-a-deep-dive-a1b015f234dd).

### Clio

[Clio](http://www.clio.com) is a cloud-based practice management solution for law firms. Last year Clio released a new version of their app, along with version 4 of their API, which allows for an unparalleled level of integration with other software tools online, such as [docassemble](https://docassemble.org).

### The Experiment

My goal for this experiment was to see whether it would be possible to have docassmble-openlcbr predict the legal outcome of a case obtained from a live Clio account online.

For the purposes of this experiment, Clio has provided me with a test account. Inside that account, I have created a single client file. I have also used Clio’s [custom fields feature](https://support.clio.com/hc/en-us/categories/200962417-Custom-Fields) to add the factors from the demonstration openlcbr trade secret database to the Clio account, and entered that data for the one test matter.

![](/1__QB49l__cFfnx2SWSqO__Zgrg.png)

I then created a modified version of the openlcbr-docassemble demo which gives you the option of either specifying a test case yourself, or downloading the data from the test Clio application.

![](/1__zuQ02MYh8SKCMNevHa__0TA.png)

### The Result

You can try the [demo interview live](https://testda.roundtablelaw.ca/interview?i=docassemble.clio%3Adata%2Fquestions%2Fclio_openlcbr_demo.yml), here.

### What I Learned

If you’re interested in the nitty-gritty, feel free to join the slack channel for my project at the [docassemble slack](https://docassemble.slack.com/) (channel name #analogyproject), and I’ll fill you in. Here’s the short version:

1.  It is slightly less convenient to add an API to docassemble than I would have anticipated. Integrating with the API configurations screen requires changes to the docassemble code, and can’t be done with just a package.
2.  Clio’s character limit for custom field names required me to make some changes to my code, and left me with a less-than-ideal interface for entering the data inside the Clio app. Luckily, I’m expecting to have an opportunity to talk to the Clions about it sometime soon.
3.  Even though I had to write the API code from scratch, connecting docassemble to Clio in useful and powerful ways was really easy. A more fully-featured docassemble-Clio package would be a powerful tool.

### What’s Next

Part 2 of the integration plan is to see whether it is possible to have data held in a Clio account used as the database-or to supplement the database-for a docassemble-openlcbr reasoning tool. Stay tuned.