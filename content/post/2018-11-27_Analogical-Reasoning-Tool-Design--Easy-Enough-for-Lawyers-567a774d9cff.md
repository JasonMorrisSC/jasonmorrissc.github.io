---
toc: true
title: 'Analogical Reasoning Tool Design, Easy Enough for Lawyers'
description: >-
  My ABA Innovation Fellowship project, through the ABA Center for Innovation,
  is docassemble-openlcbr. It is an open source software…
date: '2018-11-27T23:56:14.941Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/analogical-reasoning-tool-design-easy-enough-for-lawyers-567a774d9cff
---

My [ABA Innovation Fellowship](https://www.americanbar.org/groups/centers_commissions/center-for-innovation/Fellowships/20182019Fellows/) project, through the [ABA Center for Innovation](https://www.americanbar.org/groups/centers_commissions/center-for-innovation/), is [docassemble-openlcbr](https://github.com/Gauntlet173/docassemble-openlcbr). It is an open source software package that extends the capabilities of [docassemble](https://docassemble.org), a leading open source legal expert system tool.*

(How leading? Of the [top 20 web tools](http://www.abajournal.com/magazine/article/best_legal_apps_2018/) announced by the ABA this week, one _is_ docassemble, and two more _were built using_ docassemble. )

Docassemble-openlcbr allows docassemble interview developers to use another open source tool, [OpenLCBR](https://github.com/mgrabmair/openlcbr), to automate legal reasoning about subjective questions.

For a basic understanding of why that’s important, and a link to a live demo, check out [this blog post](https://medium.com/@jason_90344/legal-expert-systems-just-got-smarter-e7e12b75e872).

One of the goals of my project was to make openlcbr something that lawyers could play with. I can talk until I’m blue in the face about how useful openlcbr might be, but until people can actually play with it, we will never know how useful it really is.

#### Draw the F*(%#$ Owl

![](/1__g782APr9L34iwI____InLHSQ.png)

As a part of the training for the ABA Innovation Fellowship, the fellows received an introduction to design thinking and improvement kata, both of which suggest prototyping early, and often. [Clio](http://www.clio.com), the Canadian cloud-based legal practice management software company that has generously sponsored my fellowship project, has a different name for the same idea: “draw the [expletive] owl.” [Click here](https://www.reddit.com/r/funny/comments/eccj2/how_to_draw_an_owl/) for the NSFW meme that idea comes from.

So part of the fellowship project is to get lawyers playing with OpenLCBR, so that people can start drawing some owls. But playing is supposed to be fun and easy, and building an OpenLCBR reasoner is… well, here, this is what one looks like.

![](/1__Zbq3bEkXE2QMEd0JpDP6wg.png)

Now don’t get me wrong. That looks like fun to me. But I’m weird.

#### The Goal: User-Friendly Analogical Reasoner Design Tool

So one of my objectives for this project was to create a user-friendly tool for lawyers and other non-technical users to generate a OpenLCBR reasoner that can be used with docassemble-openlcbr.

Before OpenLCBR can do its magic, an expert needs to set out the factors, issues, and cases relevant to the legal issue. For details on what those are, and how they are used, check out this [deep-dive blog post about the IBP algorithm used by OpenLCBR](https://medium.com/@jason_90344/automating-case-based-reasoning-by-analogy-a-deep-dive-a1b015f234dd).

So I built a reasoner-building tool in docassemble, and included it in the docassemble-openlcbr package. The reasoner-building tool treats the YAML file above as the document to be assembled and takes you through a user-friendly interview for generating that file. You can build a new reasoner from scratch, one question at a time, or you can edit an existing one.

![](/1__eTClCzL5JCj67JKah4DPlQ.png)

The reasoner builder is effectively a small application, built entirely in docassemble. It uses a number of powerful features of docassemble, including review screens that allow the user to review a list of information they have provided, and make changes to that list later in the interview.

![](/1__nsuxg4jaVFmUv4Am1hxCow.png)

#### Can I Just Try It, Already?

Yes! I’m happy to say that this builder tool is now included in the docassemble-openlcbr package, and [you can try a live demo of it here](https://testda.roundtablelaw.ca/interview?i=docassemble.openlcbr%3Adata%2Fquestions%2Fdb_builder.yml).

#### How Can I Try Building and Using a Reasoner?

I’m working on documentation and tutorials that will eventually be at the [github page](https://github.com/Gauntlet173/docassemble-openlcbr), but if you’re curious what it would take to get yourself drawing some owls, here are the broad strokes.

1.  Get a docassemble server. The easy way is to get a [hosted docassemble server](https://community.lawyer/docassemble) from [Community.Lawyer](https://community.lawyer/) for a very reasonable price. (Community.Lawyer was also recognized in [the top 20](http://www.abajournal.com/magazine/article/best_legal_apps_2018/)!)
2.  Install docassemble-openlcbr package in docassemble’s package management by typing in the github address.
3.  Run the reasoner builder interview, and download your new reasoner file.
4.  Upload your reasoner file to the “sources” section of your docassemble server.
5.  Create an interview using the demo interview as a template that defines a test case and uses your new reasoner to predict the result.

![](/1__LmmQqqyEuFSQd9lA6Ctulw.png)

#### Is it really that easy?

No. Not really. Between steps 2 and 3 is where all the “legal knowledge engineer” (h/t Richard Susskind) stuff happens. This tool gives you an easy way to write down the analogical tool you have designed. But it does not teach you the law, or how to design an analogical reasoner well.

But don’t worry. Help is on the way.

My plan for the remainder of the fellowship project is to expand the demonstration integration with Clio, build a new demonstration reasoner from scratch, build a demonstration interview that uses that reasoner, and generate some tutorials and instructions based off that experience.

Stay tuned.

#### #Gratitude

Building this tool would have been _absolutely impossible_ without the support of Jonathan Pyle and the docassemble community. Thanks, all.

To stay in touch, follow me here, or [@RoundTableLaw on Twitter](https://www.twitter.com/RoundTableLaw). And if you’d like to help build any of these materials, or if you need help using the tool, join the #analogyproject channel on the [docassemble slack](https://docassemble.slack.com).