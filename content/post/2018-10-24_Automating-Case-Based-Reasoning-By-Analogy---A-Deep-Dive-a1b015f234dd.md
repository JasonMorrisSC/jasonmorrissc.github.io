---
toc: true
title: Automating Case-Based Reasoning By Analogy-A Deep Dive
description: >-
  Next in my series of blog posts on my ABA Center for Innovation Innovation
  Fellowship, sponsored by Clio, I’d like to show you how a…
date: '2018-10-24T21:14:06.548Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/automating-case-based-reasoning-by-analogy-a-deep-dive-a1b015f234dd
---

Next in my [series](https://medium.com/@jason_90344/why-i-study-old-artificial-intelligence-in-law-d0abf23e42aa) of [blog posts](https://medium.com/@jason_90344/legal-expert-systems-just-got-smarter-e7e12b75e872) on my [ABA Center for Innovation](http://abacenterforinnovation.org/) Innovation Fellowship, sponsored by [Clio,](http://www.clio.com) I’d like to show you how a computer does case-based reasoning by analogy by taking a deep dive into the IBP algorithm.

If you’d like to try the live demo of the tool I’m describing, [click here](https://testda.roundtablelaw.ca/interview?i=docassemble.openlcbr%3Adata%2Fquestions%2Fexplain_lcbr_test.yml).

#### The Background: IBP

In 2003, Kevin Ashley and Stefanie Brüninghaus published [a paper](https://pdfs.semanticscholar.org/24a4/7ca6f5b2ebec9e809bf19d2b0f0da3dcab81.pdf) in which they described an algorithm they called “Issue Based Prediction” or IBP. IBP was later expanded upon by Matthias Grabmair. Matthias and Kevin recently agreed to re-implement IBP as an open source tool. That open source implementation is published [on GitHub as openlcbr](https://github.com/mgrabmair/openlcbr).

The short version of what IBP does is that it allows you to have the computer do reasoning by analogy to previous cases in a way similar to the way that a lawyer who was an expert in the field might do it. But that’s the too-short version.

Let’s get into the long version. And fair warning, it’s complicated. But it’s not so complicated you can’t understand it, I promise. And understanding it is an important part of being able to see its potential.

#### The Basics

IBP needs to know certain things about the law, and then when you give it a description of a fact scenario, your test case, it can make a prediction about the answer to the legal question in that case.

So let’s look at what IBP needs to know, and then what it does with it.

### What IBP Knows

IBP needs to know about the factors, the issues, and the cases.

#### The Factors

The first thing that IBP needs to know is the various types of things which might be true about a given case, and which side of the argument they favour. It’s that simple. An expert needs to set these out.

Here’s an example of a few of the factors that exist in the trade secret database which is a part of OpenLCBR:

“plaintiff disclosed its product information to outsiders” — favours Defendant

"plaintiff's disclosures to outsiders were subject to confidentiality restrictions" - favours Plaintiff

...

#### The Issues

The next thing IBP needs to know is the legal issue and sub-issues, and how the factors relate to them. An expert needs to set these out, too.

In addition to identifying whether sub-issues must all be satisfied or only one, the expert also specifies who succeeds on that issue if it is not raised.

Here is a plain-language version of the entire issue structure for the trade secret database.

There was a misappropriation of a trade secret if (all of)  
* The information was a trade secret, which is true if (all of)  
  * The information was valuable, which will be found for the  
    plaintiff if unraised and depends on the following factors...  
  * The information was kept secret, which will be found for the  
    plaintiff if unraised and depends on the following factors...  
* The information was misappropriated, which will be found for the  
  plaintiff if unraised, and is true if (any of)  
  * There was a breach of confidentiality by the defendant,  
    which is true if (all of)  
    * The information was used, which is found for the plaintiff  
      if unraised, and depends on the following factors...  
    * There was a relationship of confidentiality, which is  
      found for the plaintiff if unraised, and depends on the  
      following factors...  
  * There was improper means used by the defendants, which  
    depends on the following factors...

I have left out the lists of factors that are relevant to each leaf issue, but otherwise, that is the issue tree. This will all look very familiar to anyone who has ever studied for an exam in law school.

The only other piece of information that is provided in the issue model is a list of factors called “knock-out” factors. These are factors which are more important than average, and can be used to justify ignoring precedential cases that do not share them with the test case.

In the test trade secret database, for example, there are two knock-out factors. The first is whether the plaintiff disclosed the alleged trade secret in a public forum, and the second is whether the plaintiff completely failed to protect their trade secret.

#### The Cases

The last thing that the algorithm needs to know is what has happened before. It needs a list of cases, and each case must have two pieces of information.

1.  What factors existed in that case?
2.  Who won?

### What IBP Does

So now you want to know: What does IBP actually do with all of this information to mimic reasoning by analogy?

OK, fair warning. This part is a bit… dense. But you don’t need to be able to repeat it from memory. Just give it a read through to satisfy yourself that it is neither magic, nor stupid.

Ready? Here we go.

You tell IBP the factors that exist in the case you are trying to predict. The algorithm starts at the root of the issue tree, the main legal question you are trying to decide. It then goes through the issue tree following these 7 rules:

1.  If the current issue was not raised, and there is no default, the algorithm “abstains” with regard to that issue. If the current issue was not raised, and there is a default, it predicts the default. “Raised” means either a relevant factor for the current issue exists in the test case, or the same is true of a sub-issue.
2.  If the current issue was raised and has sub-issues all of which must be true, it repeats the process from 1 for each sub-issue, and predicts for the plaintiff only if the prediction for all the sub-issues is also the plaintiff. It predicts for the defendant if the defendant is predicted for any of the sub-issues. If it abstains from predicting any sub-issues, it abstains from predicting this issue.
3.  If the current issue was raised and has sub-issues one of which must be true, it repeats the process from 1 for each sub-issue, and predicts for the plaintiff if any sub-issue is predicted for the plaintiff. If none is predicted for the plaintiff but any are abstained, this issue is also abstained. And if they are all predicted for the defendant, this issue is predicted for the defendant.
4.  If the current issue has no sub-issues, and all of the factors relevant to that issue are to the favour of the same party, the prediction is for that party.
5.  If the current issue has no sub-issues, and there are factors relevant to that issue going in both directions, the software collects all of the cases which share any of the factors of the test case. If it finds any precedents, and all of those cases were decided for the same party, that party is the prediction for this issue. If it finds precedents, but not all those cases were decided for the same party, it checks to see if any of the cases decided for the defendant can be explained away because they do not have the same knock-out factor as in the test case. If any cannot be explained away, it predicts for the defendant. If they can all be explained away, it predicts for the plaintiff.
6.  If the current issue has no sub-issues, and there are factors relevant to that issue going in both directions, and there are no precedents which share all of the factors in the test case, then it will search for cases with all but one of the relevant factors favouring the plaintiff, dropping each factor in turn. For each of those sets of factors, it re-does the analysis in 5 as if those were the only factors in the test case. If the result of all of those analyses is a prediction for the plaintiff, then a prediction for the plaintiff is made for the current issue. If the result of any of those predictions if for the defendant, the current issue is abstained.
7.  If it is a raised issue, with factors favouring the plaintiff, but there are no precedential cases even after dropping one factor at a time, the prediction for the current issue is abstained.

### Is there a shorter version?

FINE.

1.  If the issue is unraised, predict the default or abstain if there isn’t one.
2.  Predict conjunctive issues for the plaintiff if all sub-issues are for the plaintiff.
3.  Predict disjunctive issues for the plaintiff if any sub-issue is for the plaintiff.
4.  If the factors relevant to an issue are unanimous, follow the factors.
5.  If the cases sharing factors are unanimous, or become unanimous when you ignore cases without knock-out factors, follow the cases.
6.  If there are no cases, see if you can find some by ignoring one relevant factor at a time. If all those searches are for the plaintiff, predict for the plaintiff.
7.  Otherwise, abstain.

### That’s it?

Yes. That’s it.

#### It doesn’t seem THAT complicated.

Right? Which is why it’s so strange to me that it doesn’t seem to have ever been implemented anywhere, either commercially or in an open-source tool. (Until now.)

#### How Reliable Is It?

In the article that Ashley and Brüninghaus published, they were able to get 91% accuracy in a “leave one out” test across 186 published cases in their database.

#### Wow. 91% is pretty good.

You’re not kidding. I wonder whether most expert lawyers would be able to get that good a rate of prediction. To be fair, that result needs to be replicated before we can have an idea of the algorithm’s general usefulness. But that’s not really the point.

#### What’s the point, then?

The point is that with the exception of other academic works that have built on it, IBP has sort of been sitting on a shelf gathering dust for 15 years.

I’m not _cool_ with that. People need to play with it. Test it. Build things with it. Break it. Improve it. And understand whether and how it changes the boundaries of what legal services you can automate.

They can’t do that if it is a paper in an academic journal. But if all it takes to try it is copying and pasting an url address into their docassemble server and clicking “install”…

That’s the point of the docassemble-openlcbr project. To get automated legal case-based reasoning out of academic journals to where it can grow, change, and evolve.