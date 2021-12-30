---
toc: true
title: 'A Computer Takes the LSAT: Generating Fact Scenarios'
description: >-
  This is the third in a series of posts that show how to encode LSAT Puzzles in
  the Ergo Lite programming language. To start from the…
date: '2019-04-25T21:52:03.574Z'
categories: []
keywords: [acttl,flora2]
slug: >-
  /@roundtablelaw/a-computer-takes-the-lsat-generating-fact-scenarios-3a52fd6fe908
---

This is the third in a series of posts that show how to encode LSAT Puzzles in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

In this post, I will show you how you can use Ergo Lite to automatically generate all possible schedules.

In some of the questions we are going to encode, the question will be whether something is always true or never true about a schedule that adheres to the rules. In order to allow the computer to easily answer those questions, we can have it generate all possible schedules, and simply search to see whether or not those things are always or ever true.

There are a number of ways that you can do this sort of data generation. This is just one example. Our strategy will be to create all possible valid schedules for a single day, and then combine them to make three-day festivals.

Note that we are limiting ourselves to valid schedules, because none of the questions that we are encoding ask questions about invalid schedules.

A valid schedule for a single day has between 1 and 3 screenings of different films. Schedules also have screenings, so will will start by saying that a Possible Day is a type of Schedule.

```prolog
PossibleDay::Schedule.
```

Now we need to “create” all of the one-screening possible days. Phrased as a rule, we could say that one possible day is a single screening of a film in the first slot, for each film we know of. We can achieve that in code by making a rule.
```prolog
pd1(?film):PossibleDay[screenings->ps1(?film):Screening[slot->First,film->?film]] :- ?film:Film.
```
Here you can see that we are making a rule. The body or condition of the rule is `?film:Film`. That means “there is an object in the database, which I will call ‘film’, which is an object of type Film.”

If you ask that question of the database, it will tell you that there are three such objects, and list them. So here, our rule will be triggered three times, and each time, the conclusion of the rule we be proven true.

The head of the rule is much more complicated, but basically says “there is an object called pd1(FilmName) which is a Possible Day, and has a screening which is an object called ps1(FilmName), which is a screening of FilmName in the First slot.”

The difference between “creating an object” and “proving that an object exists” is actually no difference at all. That’s because all statements in Ergo Lite are actually Rules, but some of them have parts missing.

A rule is `head :- body.`, and says “head is proven if body is true.” If you leave out the body, you get `head :-.`, which you are allowed to shorten to `head.`. If a rule has no body, then the rule is always true. So a rule with no body is called a “fact.” Proving something is true with a rule, and stating that it is true with a “fact” are just different ways of proving that it is true.

Then, if you have a rule with no head, you get `:- body.`, which you are allowed to state as `?- body.`. If you say “when this is true, nothing is proven”, by leaving out the head of a rule, Ergo Lite treats that statement as the question “When is this true?” and will search the database for matching objects and display them for you.

Queries are usually used when using Ergo Lite’s interactive terminal. We will use queries later in the series to get answers to our questions.

So we have created a new Possible Day object pd1(Film) for each Film in the database.

We can do the same thing for two-day and three-day possible schedules. Of course, in a two day schedule, there are two films, and we need to be explicit that they are different from one another, and we need to create two screenings.

Note that we could, if we wanted, generate invalid possible days by including days in which the same film is shown twice. We could then rely on our rules to distinguish between valid and invalid schedules. But if we followed that strategy, we would end up with nearly 60,000 possible schedules, each of which would need to be checked for validity each time we wanted to ask a question about only the valid ones. That would make our code much too slow to be usable.

Because we know we are only going to be asking questions about valid schedules, we can avoid that sort of problem by limiting the examples that we create to those that are as valid as we can make them. Here, we can make them more valid my making sure that films are not repeated in a given day.
```prolog
pd2(?first,?second):PossibleDay,  
pd2(?first,?second)[screenings->ps2(?second):Screening],  
pd2(?first,?second)[screenings->ps1(?first):Screening],  
ps2(?second)[slot->Second,film->?second],  
ps1(?first)[slot->First,film->?first] :-  
    ?first:Film,  
    ?second:Film,  
    ?first !== ?second.
```
And then, for three day schedules, the pattern continues.
```prolog
pd3(?first,?second,?third):PossibleDay,  
pd3(?first,?second,?third)[screenings->ps1(?first):Screening],  
pd3(?first,?second,?third)[screenings->ps2(?second):Screening],  
pd3(?first,?second,?third)[screenings->ps3(?third):Screening],  
ps3(?third)[slot->Third,film->?third],  
ps2(?second)[slot->Second,film->?second],  
ps1(?first)[slot->First,film->?first] :-  
    ?first:Film,  
    ?second:Film,  
    ?third:Film,  
    ?first !== ?second,  
    ?first !== ?third,  
    ?second !== ?third.
```
So now we have created all the possible days. As it turns out, there are only 15. Now we are going to need to combine three of these possible valid days to create all the valid festival schedules.

To do this we tell the code that a TestSchedule is a type of Schedule.
```prolog
TestSchedule:Schedule
```
Then we tell it that if there are three PossibleDays, there is a test schedule object with those three PossibleDays. In order to make these Schedules as valid as possible, and limit the size of the database we need to search, we will create a test schedule only out of days that are valid for the three days of the festival.

To do this, we need reformulate our Thursday, Friday and Saturday rules to determine not whether a schedule is valid, but whether a possible day is valid for that day of the week. We can do that by taking the three day rules, and the rule for “last”, and change references to Schedule into reference to PossibleDay, and by removing references to specific days. The code looks like this:
```prolog
?x[last->?film] :-  
    ?x:PossibleDay[screenings->?_s[slot->?_slot,film->?film]],  
    \naf ?x[screenings->?_os[slot->?_[after->?_slot]]].

?x[thursday_eligible->\true] :-  
    ?x:PossibleDay[screenings->?_s[film->Harvest]],  
    ?x[last->Harvest].

?x[friday_eligible->\true] :-  
    ( // either  
        ?x:PossibleDay[last->Greed],  
        \naf exists(?_s)^?x[screenings->?_s[film->Limelight]]  
    ) \or (  
        ?x:PossibleDay[last->Limelight],  
        \naf exists(?_s)^?x[screenings->?_s[film->Greed]]  
    ).

?x[saturday_eligible->\true] :-  
    ( // either  
        ?x:PossibleDay[last->Greed],  
        \naf exists(?_s)^?x[screenings->?_s[film->Harvest]]  
    ) \or (  
        ?x:PossibleDay[last->Harvest],  
        \naf exists(?_s)^?x[screenings->?_s[film->Greed]]  
    ).
```
Now we can write a rule that says a test schedule exists for each combination of a valid Thursday, Friday, and Saturday. The name given to these objects is designed so that when we search for the name of a test schedule, its name will tell us everything about its contents.
```prolog
test_sched(?ps1,?ps2,?ps3):TestSchedule[thursday->?ps1,friday->?ps2,saturday->?ps3] :-  
    ?ps1:PossibleDay[thursday_eligible->\true],  
    ?ps2:PossibleDay[friday_eligible->\true],  
    ?ps3:PossibleDay[saturday_eligible->\true].
```
Then we say that if a TestSchedule’s Thursday attribute (which is a possible day) has a screening, then the TestSchedule also has that screening object.
```prolog
?x[screenings->s(r,?f,?s):Screening[film->?f,slot->?s,day->Thursday]] :-  
    ?x:TestSchedule[thursday->?_[screenings->?_y[film->?f,slot->?s]]].
```
Then we do the same for Friday and Saturday.
```prolog
?x[screenings->s(f,?f,?s):Screening[film->?f,slot->?s,day->Friday]] :-  
    ?x:TestSchedule[friday->?_[screenings->?_y[film->?f,slot->?s]]].

?x[screenings->s(s,?f,?s):Screening[film->?f,slot->?s,day->Saturday]] :-  
    ?x:TestSchedule[saturday->?_[screenings->?_y[film->?f,slot->?s]]].
```
We have now generated all of the possible valid schedules, and a few invalid ones, because we did not check to make sure that each film is shown at least once. Of the 80 possible schedules generated, 16 are not valid for that reason. You could modify the rule that creates test_sched objects to check to see whether each film is included in one of the three possible days at least once, but that is left as an exercise to the reader.

Now we have the data that we need in order to be able to search all the possible Valid Schedules, and we’re ready to [start answering questions](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-6-884c47d55b2b).

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.