---
toc: true
title: 'A Computer Takes the LSAT: Question 10'
description: >-
  This is the 8th in a series of posts about encoding LSAT Puzzles in the Ergo
  Lite programming language. To start from the beginning, go to…
date: '2019-04-25T21:51:46.279Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-question-10-2172137fdc57
---

This is the 8th in a series of posts about encoding LSAT Puzzles in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

In this post, we will encode the last question, question 10. It reads:

> 10\. If Limelight is shown exactly three times, Harvest is shown exactly twice, and Greed is shown exactly once, then which one of the following is a complete and accurate list of the films that could be the first film shown on Thursday?   
> (A) Harvest   
> (B) Limelight   
> (C) Greed, Harvest   
> (D) Greed, Limelight   
> (E) Greed, Harvest, Limelight

The first thing to notice about this is that the restrictions on the number of showings for the three films are identical to the restrictions in question 9, so the code we used in question 9 so determine whether or not a schedule complies with those requirements can be re-used here. We just need to refer to schedules where q9_rule is true.

This is actually a lot easier to answer with Ergo Lite if we ignore the question and answer structure. We can get the answer to this question by simply saying “are there any films that were shown first on Thursday in a schedule where q9_rule is true?” That query and response looks like this:
```
?- ?_x[q9_rule->\true,screenings->?_[day->Thursday,slot->First,film->?film]].

?film = Greed

?film = Limelight

2 solution(s) in 0.094 seconds; elapsed time = 0.090

Yes
```
So we already know the correct answer here is (D).

But let’s stick to the pattern, so that we can use the `question(?x,?y).` query to get answers to all 5 questions when we are finished.

Because there are only three options, we can just ask, three times, whether in any object where q9_rule is true, each of the given films is ever shown first on Thursday, and use `\naf` to negate the ones that should not be included in the list for that answer.

For example, here is how we can encode option (a):
```prolog
question(10,a) :-  
    ?_x1[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Harvest]],  
    \naf ?_x2[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Limelight]],  
    \naf ?_x3[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Greed]].
```
This says “Is it true that there is a schedule for which q9_rule is true, and for which Harvest is shown first on Thursday, and that there is no schedule where q9_rule is true for which Limelight was shown first, and that there is no schedule where q9_rule is true for which Greed was shown first?”

Note that we use the variable names _x1 and _x2 because we don’t want to look for the same schedule multiple times. We want three different ones. But we don’t need to check to be certain they are different, because we know that q9_rule checks to see if they are valid, and if they are valid they never have more than one film in the same slot, so no single schedule would match more than one of the tests.

The rest of the answers differ only in which elements have been negated out.
```prolog
question(10,b) :-  
    \naf ?_x1[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Harvest]],  
    ?_x2[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Limelight]],  
    \naf ?_x3[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Greed]].

question(10,c) :-  
    ?_x1[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Harvest]],  
    \naf ?_x2[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Limelight]],  
    ?_x3[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Greed]].

question(10,d) :-  
    \naf ?_x1[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Harvest]],  
    ?_x2[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Limelight]],  
    ?_x3[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Greed]].

question(10,e) :-  
    ?_x1[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Harvest]],  
    ?_x2[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Limelight]],  
    ?_x3[q9_rule->\true,screenings->?_[slot->First,day->Thursday,film->Greed]].
```
And now we can anticlimactically ask Ergo Lite to tell us which is the correct answer for question 10:
```
?- question(10,?x).

?x = d

1 solution(s) in 0.125 seconds; elapsed time = 0.126

Yes
```
Yeah, we know. :)

That’s it. Check out [the next post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-conclusion-86dc7467b14d) for the conclusion of this series.

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.