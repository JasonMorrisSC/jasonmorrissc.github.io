---
toc: true
title: 'A Computer Takes the LSAT: Question 9'
description: >-
  This is the 7th in a series of posts describing how to encode LSAT Puzzles in
  the Ergo Lite programming language. To start from the…
date: '2019-04-25T21:51:49.756Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-question-9-f33d7200f48f
---

This is the 7th in a series of posts describing how to encode LSAT Puzzles in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

In this post, we will encode Question 9, which reads:

> 9\. If Greed is shown exactly three times, Harvest is shown exactly twice, and Limelight is shown exactly once, then which one of the following must be true?

> (A) All three films are shown on Thursday.

> (B) Exactly two films are shown on Saturday.

> (C) Limelight and Harvest are both shown on Thursday.

> (D) Greed is the only film shown on Saturday.

> (E) Harvest and Greed are both shown on Friday

So this question adds a new set of rules. We are no longer talking only about all valid schedules, we are talking specifically about schedules where the count of the screenings of the films is exactly as set out.

So we will create a rule that sets the attribute `q9_rule` to true if the schedule meets the requirements.
```prolog
?x[q9_rule->\true] :-  
    ?x:ValidSchedule,  
    count{?greed|?x[screenings->?greed[film->Greed]]} == 3,  
    count{?harvest|?x[screenings->?harvest[film->Harvest]]} == 2,  
    count{?limelight|?x[screenings->?limelight[film->Limelight]]} == 1.
```
So here we have said that the q9_true attribute of an object is true if that object is a valid schedule, if there are 3 showings of Greed, 2 showings of Harvest, and 1 showing of Limelight.

For the rest of this question, we can now rely on q9_rule being true to indicate both that it is a valid schedule and that it meets the requirements for this question.

The question asks which of these things is always true. In order to test that, we will break each answer into two rules, in the same way we did for question 7. The first rule will say that the answer is correct if we couldn’t find any counter-examples. The second rule will tell us whether any counter-examples exist.

For example, for answer a, we want to know whether all three films are shown on the Thursday. We already know, because we are talking about valid schedules, that a film cannot be shown more than once on the same day. And we know there are only three films. So the only thing we need to check is whether there are three films shown on Thursday. We can look for those counter-examples like this:
```prolog
question(9,a) :- \naf q9a.

q9a :-  
    count{?s|?_x[q9_rule->\true,screenings->?s[day->Thursday]]} !== 3.
```
Here we have used the `count{a|b}` quantifier in Ergo Lite to count how many screening objects are associated with each Schedule for which q9_rule is true, and then we have checked to see if there are any for which the count is not three.

The first rule will then determine that if it can’t find any counter examples, it must be true that all such schedules have three screenings on Thursday.

For answer (B), we can use the same technique to see if there were any schedules meeting the rules that had other than 2 screenings on Saturday.
```prolog
question(9,b) :- \naf q9b.

q9b :-  
    count{?s|?_x[q9_rule->\true,screenings->?s[day->Saturday]]} !== 2.
```
Answer (C) is different. It asks whether two films are always shown on Thursday. To turn this into a counter example search, we need to look for schedules where either film wasn’t shown on the Thursday. We can do that as follows:
```prolog
question(9,c) :- \naf q9c.

q9c :-  
    ?_x[q9_rule->\true,screenings->?_s],  
    (  
        \naf ?_s[day->Thursday,film->Limelight];  
        \naf ?_s[day->Thursday,film->Harvest]  
    ).
```
Note the semi-colon used between the check for Limelight and the check for Harvest. The semi-colon means “or”. The parenthesis around all of the options means that the questions inside the parenthesis should be answered first.

Answer (D) asks whether Greed is the only film shown on Saturday. In order to find a counter-example, we need to find a schedule where there was a screening of a film that was not Greed. We can do that as follows:
```prolog
question(9,d) :- \naf q9d.

q9d :-  
    ?_x[q9_rule->\true],  
    ?_x[screenings->?_[day->Saturday,film->?_film]],  
    ?_film !== Greed.
```
Answer (E) is the same as answer (C) but with a different day and different films. So we can use the same technique again.
```prolog
question(9,e) :- \naf q9e.

q9e :-  
    ?_x[q9_rule->\true],  
    (  
        \naf ?_x[screenings->?_s[film->Greed,day->Friday]];  
        \naf ?_x[screenings->?_s2[film->Harvest,day->Friday]]  
    ).
```
That’s it. We can now run the query `question(9,?x).` to check our work, and we get:
```
?x = e

1 solution(s) in 0.000 seconds; elapsed time = 0.001

Yes
```
We can see that this is the right answer if we look at the list of all of the schedules that satisfy the count requirements:
```
?- ?x[q9_rule->\true].

?x = test_sched(pd2(Greed,Harvest),pd2(Harvest,Greed),pd2(Limelight,Greed))

?x = test_sched(pd3(Greed,Limelight,Harvest),pd2(Harvest,Greed),pd1(Greed))

?x = test_sched(pd3(Limelight,Greed,Harvest),pd2(Harvest,Greed),pd1(Greed))

3 solution(s) in 0.000 seconds; elapsed time = 0.002

Yes
```
There are only three schedules that satisfy the requirements, and (E) is the only answer that is true for all three of them.

Next up is [Question 10](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-10-2172137fdc57).

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.