---
toc: true
title: 'A Computer Takes the LSAT: Question 8'
description: >-
  This is the 6th in a series of posts about encoding LSAT Puzzles in the Ergo
  Lite programming language. To start from the beginning, go to…
date: '2019-04-25T21:51:52.761Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-question-8-bf39194443da
---

This is the 6th in a series of posts about encoding LSAT Puzzles in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

In this post, we will encode Question 8. It reads:

> 8\. If Limelight is never shown again during the festival once Greed is shown, then which one of the following is the maximum number of film showings that could occur during the festival?

> (A) three

> (B) four

> (C) five

> (D) six

> (E) seven

To get at this question, we need to be able to say that limelight was not shown after Greed in a given schedule, and limit our answers to Schedules where that is true.

But before we can get to that, we need to deal with the word “after”. We know what “after” is for Slots, because we said `First[after->Second]`, for example. But we don’t know what “after” means if we are talking about _either later the same day or later in the festival._

So to get at that, we are going to have to define ‘after’ for Days, not just slots. As we will see in a moment, we also need to define ‘before’ for Days. So we will go back to the code that we used to define the days and slots, and we will replace it with this:
```prolog
Day[|after=>Day,before=>Day|].  
Thursday:Day[before->{Friday,Saturday}].  
Friday:Day[after->Thursday, before->Saturday].  
Saturday:Day[after->{Thursday,Friday}].

Slot[|after=>Slot|].  
First:Slot.  
Second:Slot[after->First].  
Third:Slot[after->{First,Second}].
```
The answers to question 8 are also dealing with the total number of screenings in a Schedule. We can simplify the process of referring to the total number of screenings by making a rule that counts the screenings and sets the number to the attribute screening_count.
```prolog
?x[screening_count->?C] :-  
    ?x:ValidSchedule,  
    ?C = count{?s|?x[screenings->?s]}.
```
`count{x|y}` is an aggregate function provided in Ergo Lite. So now if something is a valid schedule, it will have an attribute `screening_count` that is equal to the number of screenings in that schedule.

Now we can create a rule to confirm that limelight was not shown after greed. That will be true if the object is a valid schedule, and we find out what day and what slot Greed was shown (because it is a valid schedule we know it was shown at least once), and we can confirm that Limelight was not shown later in the same day, and Limelight was not shown on a later day.

Did that seem right? It isn’t. Close, but not quite. Here’s why:

What if the schedule has Greed, and then Limelight, and then Greed, with no more Limelight showings?We have the problem that our search will find the schedule, and it will find two different showings of Greed. Based on the definition we just gave, we would accidentally mark the schedule as compliant based on the second screening of Greed, when the first screening should have made it non-compliant.

So what we need to be sure of is that after the _first_ showing of Greed, there are no further showings of Limelight.

So, our test should be: it is a valid schedule, there is a showing of greed, there are no other showings of greed on days before that one, there are no other showings of Limelight later on the same day as greed, and there are no other showings of Limelight on later days.

We know that there can’t be another showing of Greed before a given showing on the same day, so we only need to define “before” for days, not for slots.

Now we’re ready to code.
```prolog
?x[limelight_not_after_greed->\true] :-  
    ?x:ValidSchedule,  
    ?x[screenings->?_s[film->Greed,day->?_day,slot->?_slot]],  
    \naf exists(?_os)^?x[screenings->?_os[day->?_[before->?_day],film->Greed]],  
    \naf exists(?_os)^?x[screenings->?_os[day->?_day,film->Limelight,slot->?_[after->?_slot]]],  
    \naf exists(?_os)^?x[screenings->?_os[day->?_[after->?_day],film->Limelight]].
```
So this code reads “it is true that limelight is not after greed with regard to an object X if it is true that X is a valid schedule, there is a screening of Greed, there is no other screening of Greed on an earlier day, there is no other screening of Limelight later that day, and there is no other screening of Limelight on a later day.”

Now we can limit the schedules what we are talking about by referring to valid schedules for which limelight not after greed is true.

In addition to the `count{a|b}` aggregate function provided by Ergo Lite, there is also a `max{a|b}` function that can be used to find the largest of a series of values. So for each of the 5 possible answers, we will ask whether the maximum value of screening count for all valid schedules where limelight is not shown after greed is equal to the given number.

Our code for the answers looks like this:
```prolog
question(8,a) :-  
    max{?sc|?_x:ValidSchedule[limelight_not_after_greed->\true,screening_count->?sc]} == 3.  
      
question(8,b) :-  
    max{?sc|?_x:ValidSchedule[limelight_not_after_greed->\true,screening_count->?sc]} == 4.

question(8,c) :-  
    max{?sc|?_x:ValidSchedule[limelight_not_after_greed->\true,screening_count->?sc]} == 5.

question(8,d) :-  
    max{?sc|?_x:ValidSchedule[limelight_not_after_greed->\true,screening_count->?sc]} == 6.

question(8,e) :-  
    max{?sc|?_x:ValidSchedule[limelight_not_after_greed->\true,screening_count->?sc]} == 7.
```
Now, as per our usual pattern, we can ask what the correct answer is to question 8 by using the query `question(8,?x)`. And the response we get is:
```
?x = d
```
We can see this particular schedule by running another query that says “show me the valid schedules where the number of screenings is 6, and limelight is not shown after greed”:
```
?- ?x:ValidSchedule[limelight_not_after_greed->\true,screening_count->6].

?x = test_sched(pd2(Limelight,Harvest),pd2(Harvest,Limelight),pd2(Limelight,Greed))

?x = test_sched(pd3(Limelight,Greed,Harvest),pd2(Harvest,Greed),pd1(Greed))

?x = test_sched(pd3(Limelight,Greed,Harvest),pd2(Harvest,Greed),pd1(Harvest))

3 solution(s) in 0.046 seconds; elapsed time = 0.041

Yes
```
So you can see that there are three test schedules that are valid, in which Limelight does not appear after Greed in the schedule, and which have a total of 6 screenings.

That’s three questions down. In [question 9](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-9-f33d7200f48f), we’re going to change the rules, and then try and figure out what must always be true.

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.