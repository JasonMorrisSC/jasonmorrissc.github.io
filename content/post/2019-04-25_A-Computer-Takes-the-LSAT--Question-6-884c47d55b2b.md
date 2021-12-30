---
toc: true
title: 'A Computer Takes the LSAT: Question 6'
description: >-
  This is the 4th in a series of posts about encoding LSAT questions in the Ergo
  Lite programming language. To start from the beginning, go…
date: '2019-04-25T21:52:00.084Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-question-6-884c47d55b2b
---

This is the 4th in a series of posts about encoding LSAT questions in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

In this post, we will encode the first of 5 questions using the code we generated earlier.

Question 6 reads as follows:

> 6\. Which one of the following could be a complete and accurate description of the order in which the films are shown at the festival?  
> (A) Thursday: Limelight, then Harvest; Friday: Limelight; Saturday: Harvest   
> (B) Thursday: Harvest; Friday: Greed, then Limelight; Saturday: Limelight, then Greed   
> (C) Thursday: Harvest; Friday: Limelight; Saturday: Limelight, then Greed   
> (D) Thursday: Greed, then Harvest, then Limelight; Friday: Limelight; Saturday: Greed   
> (E) Thursday: Greed, then Harvest; Friday: Limelight, then Harvest; Saturday: Harvest

So to answer this question, we need to create these schedules in code, and ask if each one is a ValidSchedule.

One important thing to note, here, is that we are not going to tell the software what process to follow in order to figure out which answer is true. Instead, we are going to describe the possible answers and then we are going to ask questions about those possible answers.

So here, we need to encode the schedule set out in each answer, and then ask if that answer is correct.

To make it convenient to check for correct answers later, we are going to use the statement `question(1,a)` to mean the correct answer to question 1 is answer (a).

So first, we set out the schedule in a above:
```prolog
q1a:Schedule[screenings->{  
        \#:Screening[day->Thursday,film->Limelight,slot->First],  
        \#:Screening[day->Thursday,film->Harvest,slot->Second],  
        \#:Screening[day->Friday,film->Limelight,slot->First],  
        \#:Screening[day->Saturday,film->Harvest,slot->First]  
    }].
```
And then we make a rule that says the correct answer to question 6 is answer (a) if the schedule we just created, called `q1a`, is Valid.
```prolog
question(6,a) :-  
    q1a:ValidSchedule.
```
That’s it. Now we can ask whether it is true that the correct answer to question 6 is answer (a) by using the query `question(6,a).`. And Ergo Lite’s interactive reasoner will say `No`.

So we continue with the four other options.
```prolog
q1b:Schedule[screenings->{  
        \#:Screening[day->Thursday,film->Harvest,slot->First],  
        \#:Screening[day->Friday,film->Greed,slot->First],  
        \#:Screening[day->Friday,film->Limelight,slot->Second],  
        \#:Screening[day->Saturday,film->Limelight,slot->First],  
        \#:Screening[day->Saturday,film->Greed,slot->Second]  
    }].

question(6,b) :-  
    q1b:ValidSchedule.

q1c:Schedule[screenings->{  
        \#:Screening[day->Thursday,film->Harvest,slot->First],  
        \#:Screening[day->Friday,film->Limelight,slot->First],  
        \#:Screening[day->Saturday,film->Limelight,slot->First],  
        \#:Screening[day->Saturday,film->Greed,slot->Second]  
    }].

question(6,c) :-  
    q1c:ValidSchedule.

q1d:Schedule[screenings->{  
        \#:Screening[day->Thursday,film->Greed,slot->First],  
        \#:Screening[day->Thursday,film->Harvest,slot->Second],  
        \#:Screening[day->Thursday,film->Limelight,slot->Third],  
        \#:Screening[day->Friday,film->Limelight,slot->First],  
        \#:Screening[day->Saturday,film->Greed,slot->First]  
    }].

question(6,d) :-  
    q1d:ValidSchedule.

q1e:Schedule[screenings->{  
        \#:Screening[day->Thursday,film->Greed,slot->First],  
        \#:Screening[day->Thursday,film->Harvest,slot->Second],  
        \#:Screening[day->Friday,film->Limelight,slot->First],  
        \#:Screening[day->Friday,film->Harvest,slot->Second],  
        \#:Screening[day->Saturday,film->Harvest,slot->First]  
    }].

question(6,e) :-  
    q1e:ValidSchedule.
```
And now, instead of asking whether each one is proven, we can ask “what is the correct answer to question 6” by using the query `question(6,?x).`. Ergo Lite’s interactive reasoner will respond:
```text
?X = c  
1 solution(s) in 0.000 seconds; elapsed time = 0.001  
Yes
```
So the correct answer to question 6 is c.

And we can confirm that. The answer in (a) does not show Greed at all. The answer in (b) shows both Greed and Limelight on Friday. The answer in (d) doesn’t have Harvest as the last film on Thursday. The answer in (e) does not show Limelight last on Friday. Only (c) is correct.

You might wonder why Ergo Lite says “Yes” in response to that query. That is because every query (and condition to a rule) in Ergo Lite is implicitly a question in the form of “are there any objects in the database for which all the following things are true?” If there are any, it will say “Yes”, and if there are none, it will say “No.” If you have asked what they are, it will also tell you what they are. But you can also tell Ergo Lite that you don’t want to know what they are, you just want to know if they exist, by putting an underscore in front of all of the variable names you are not interested in seeing the specifics of.

So the query `question(6,?_x).` would just give you:
```text
Times (in seconds): elapsed = 0.001; pure CPU = 0.000  
Yes
```
So we’ve started answering questions. But this was the easy one, because it didn’t add any complications to the rules, it just gave us fact scenarios. [Question 7](https://medium.com/@jason_90344/a-computer-takes-the-lsat-question-7-aa98d14d3217) will ask us to say what it true or false about all valid schedules, so our testing data will become useful.

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.