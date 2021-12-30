---
toc: true
title: 'A Computer Takes the LSAT: Question 7'
description: >-
  This is the 5th in a series of posts describing how to encode LSAT Puzzles in
  the Ergo Lite programming language. To start from the…
date: '2019-04-25T21:51:56.038Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-question-7-aa98d14d3217
---

This is the 5th in a series of posts describing how to encode LSAT Puzzles in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

In this post, we will encode Question 7, which reads:

> 7\. Which one of the following CANNOT be true?

> (A) Harvest is the last film shown on each day of the festival.

> (B) Limelight is shown on each day of the festival.

> (C) Greed is shown second on each day of the festival.

> (D) A different film is shown first on each day of the festival.

> (E) A different film is shown last on each day of the festival.

You will recall that when we encoded the pre-amble we created a rule that allows us to ask which film is last on each day of a schedule. We will need that functions for encoding the answers to this question.

We know that we have generated all of the valid schedules, so finding out if something cannot be true just means finding a counter-example. If there is one schedule for which that thing is true, we know it is not impossible.

So we will encode these answers in two parts. The first part will say that the answer is correct if no counter-examples were found. The second part will say whether a counter-example exists.

For answer (A), we need to find a counter example in which Harvest is the last film for all three days. We can do that as follows:
```prolog
question(7,a) :- \naf q7a.

q7a :-  
    ?_x:ValidSchedule[last(Thursday)->Harvest,last(Friday)->Harvest,last(Saturday)->Harvest].
```
So the first rules says that answer a is the correct answer for question 7 if it is not true that a counterexample was found. The second rule says that a counter-example is found if there is an object that is a ValidSchedule, and has Harvest as the last film for all three days.

For answer (B), we try to find examples in which Limelight is shown on each day of the festival, and if we can’t find any, then B must be the correct answer.
```prolog
question(7,b) :- \naf q7b.

q7b :-  
    ?_x:ValidSchedule[screenings->?_[day->Thursday,film->Limelight]],  
    ?_x:ValidSchedule[screenings->?_[day->Friday,film->Limelight]],  
    ?_x:ValidSchedule[screenings->?_[day->Saturday,film->Limelight]].
```
For answer (C), we want to know if there are any valid schedules in which Greed is shown second all three days. While “last” needed some extra work to make sense in the programming language, we already have a good way to refer to the film that is second, so we can encode this answer as follows:
```prolog
question(7,c) :- \naf q7c.

q7c :-  
    ?_x:ValidSchedule,  
    ?_x[screenings->?_[film->Greed,day->Thursday,slot->Second]],  
    ?_x[screenings->?_[film->Greed,day->Friday,slot->Second]],  
    ?_x[screenings->?_[film->Greed,day->Saturday,slot->Second]].
```
For answer (D), we want to know if it’s possible for the first film of each day of the schedule to be different. We can get that answer as follows:
```prolog
question(7,d) :- \naf q7d.

q7d :-  
    ?_:ValidSchedule[screenings->?_[film->?_rf,day->Thursday,slot->First],screenings->?_[film->?_ff,day->Friday,slot->First],screenings->?_[film->?_sf,day->Saturday,slot->First]],  
    ?_rf !== ?_ff,  
    ?_ff !== ?_sf,  
    ?_sf !== ?_rf.
```
For answer (E), we want to know if it’s possible for the last film of each day in the schedule to be different. We can do that by using our “last” rule.
```prolog
question(7,e) :- \naf q7e.

q7e :-  
    ?_:ValidSchedule[last(Thursday)->?_rf,last(Friday)->?_ff,last(Saturday)->?_sf],  
    ?_rf !== ?_ff,  
    ?_ff !== ?_sf,  
    ?_sf !== ?_rf.
```
Now that we have encoded all of the answers, we can ask Flora-2 to tell us what the right answer is.
```text
?- question(7,?x).

?x = a

1 solution(s) in 0.000 seconds; elapsed time = 0.001

Yes
```
The correct answer is (A). Which makes sense when we remember that one of the rules for a valid schedule is that either Greed or Limelight is shown last on the Friday.

Next we encode [Question 8](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-8-bf39194443da)!

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.