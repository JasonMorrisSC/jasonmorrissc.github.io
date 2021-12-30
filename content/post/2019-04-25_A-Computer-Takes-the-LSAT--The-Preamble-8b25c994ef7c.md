---
toc: true
title: 'A Computer Takes the LSAT: The Preamble'
description: >-
  This is the second in a series of posts demonstrating how to encode LSAT
  Puzzle questions in the Ergo Lite programming language. To start…
date: '2019-04-25T21:51:42.464Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-the-preamble-8b25c994ef7c
---

This is the second in a series of posts demonstrating how to encode LSAT Puzzle questions in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

In this post, we will encode the “preamble” to the section of questions. These are the basic rules that apply to all of the following questions. By analogy to automating legal services, these rules represent statute law or regulations.

The preamble reads as follows:

> Exactly three films-Greed, Harvest, and Limelight-are shown during a film club’s festival held on Thursday, Friday, and Saturday. Each film is shown at least once during the festival but never more than once on a given day. On each day at least one film is shown. Films are shown one at a time. The following conditions apply:

> * On Thursday Harvest is shown, and no film is shown after it on that day.

> * On Friday either Greed or Limelight, but not both, is shown, and no film is shown after it on that day.

> * On Saturday either Greed or Harvest, but not both, is shown, and no film is shown after it on that day.

So the first thing that we need to do is we need to tell the software about the types of objects that exist in our problem domain. Which objects to encode and how are up to the person doing the encoding, but here’s how I approached it.

First, there is a type of object called a Film, and we know the names of all three of the possible films. In Ergo Lite, the `:` character means “is a”, and allows you to specify the object type of names that you provide to the code. So to say that Greed, Harvest, and Limelight are all films, we write:
```prolog
Greed:Film.  
Harvest:Film.  
Limelight:Film.
```
You will notice that all of the “sentences” in Ergo Lite end with a period.

Ergo Lite allows you to use a set of names as a shortcut to typing the same thing out over and over, which we will do to specify that Thursday, Friday, and Saturday are the possible days for a festival.
```prolog
{Thursday, Friday, Saturday}:Day.
```
Now, I’m going to say that a “schedule” is one or more screenings. Here I will use Ergo Lite’s syntax for creating an abstract type of object, like a blank form, and saying what fields exist on that form.
```prolog
Schedule[|screenings=>Screening|].
```
So here I have said that “Schedule” is a type of object, and that type of object has one or more “screenings”, each of which is a “Screening” object.

Now I need to describe what a Screening is. I will say that a Screening is a film, shown on a day, in a certain time slot.
```prolog
Screening[|film=>Film,day=>Day,slot=>Slot|].
```
We have already given information about films, and days, so now I need to describe a slot. We need slots in order to figure out whether films are being shown before, after, or at the same time as one another. I will say that a slot might be first, second, or third.
```prolog
{First,Second,Third}:Slot.
```
Several of the rules in the preamble make use of the criteria “after”. This is the sort of thing that is implicitly understood by people reading legislation, but which needs to be explicitly explained to the computer. So I will say that a Slot object has an attribute “after”, which is a list of one or more other Slots that the current slot is after. And then, I will populate that attribute.
```prolog
Slot[|after=>Slot|].  
Second[after->First].  
Third[after->First,after->Second].
```
Now I need to be able to distinguish between a schedule that follows the rules and one that doesn’t. So I will use the `::` operator, which means “is a type of”, so say that a ValidSchedule is a type of Schedule.
```prolog
ValidSchedule::Schedule.
```
This means that a ValidSchedule also has all of the parts of a schedule, which is a list of screenings, where each screening is a film, a day, and a slot.

That sets out the ontology of our question, the objects that exist and how they relate to one another. Now we need to set out the rules, and you can see that there are a number of them in the preamble. We will encode each, one by one.

A rule uses the basic structure `head :- body`. The head is known to be true if the body can be proved. It is similar to saying `if body then head` , except that in a declarative programming language the word “then” does not mean “next.” Instead, it means `if we know body is true then we know head is true`.

So we will start with a rule that defines what makes a ValidSchedule. The head of my rule says “something, called X, is a ValidSchedule”. The body says “if it is a schedule and these things are true about it.”
```prolog
?x:ValidSchedule :-  
    ?x:Schedule[  
        each_film_at_least_once->\true,   
        \naf film_twice_same_day->\true,   
        at_least_one_film_per_day->\true,   
        \naf more_than_one_film_at_a_time->\true,  
        thursday_rule->\true,  
        friday_rule->\true,  
        saturday_rule->\true  
    ].
```
You’re going to see `\naf` a lot in the code. It stands for “negation as failure”, and it is a type of negation. The specifics are not important for now. You can read `\naf` as “it is not true that”.

You will also notice the commas at the end of the lines. In Ergo Lite the comma `,` means “and”. The things on both sides of the comma must be true for the statement to be true.

You can see that I have taken the 7 rules about a valid schedule from the pre-amble, and made each of them a property of a Schedule object that might be true or false.

Now we need to explain to the software how to determine whether each of those factors is true.

First, whether each film is shown at least once. Intuitively, it is true that each film was shown at least once if there is at least one screening in a schedule for each of the three films. So we can ask, once for each film, whether there is any screening of that film.
```prolog
?x[each_film_at_least_once->\true] :-  
    ?x:Schedule[screenings->?_y1:Screening[film->Greed]],  
    ?x:Schedule[screenings->?_y2:Screening[film->Harvest]],  
    ?x:Schedule[screenings->?_y3:Screening[film->Limelight]].
```
When you use the same variable name like `?x` multiple times in a rule, you are referring to the same object. If you use different variable names like `?_y1` and `?_y2`, you might be referring to the same object, or to a different object. Inserting an underscore after the question mark in the variable name tells the code that you are only interested in whether there is an object that matches, you are not interested in knowing it’s name.

So, this rule can be read as follows:

> An object, called X, has the attribute “each film at least once” set to true if it can be shown that:

> That object X has a screening object y1, and that object y1 is for the film Greed, and

> That object X has a screening object y2, and that object y2 is for the film Harvest, and

> That object X has a screening object y3, and that object y3 is for the film Limelight.

Next we need to encode the rule that films are not supposed to be shown more than once on the same day. We defined ValidSchedule above as a situation in which this is not true. So here we need to define when it is true.
```prolog
?x[film_twice_same_day->\true] :-  
    ?x:Schedule[screenings->?_y:Screening[film->?_yf,day->?_day]],  
    ?x[screenings->?_z:Screening[film->?_yf,day->?_day]],  
    ?_y !== ?_z.
```
So here, the body of the rule can be read as follows:

“If an object X is a Schedule, and it has a screening called Y, and Y is a screening, and Y has a film called YF, and a day called DAY; and the object X has a screening Z that is a screening and its film is also YF and its day is also DAY, and Y and Z are not the same object.”

So we have asked it whether there is a schedule X that has two screenings, Y and Z, where Y and Z are for the same film on the same day, and they are not the same object.

We need to say explicitly that Y and Z can’t be the same object, because that is not implicit in the names. It can be true that “object x has a screening y” and “object x has a screening z” even if x only has one screening. We have just given that one screening two names.

That is because when you use the same variable name twice, you are referring to the same object. When you use different variable names, you MAY still be referring to the same object, but you might not. You need to check, if it’s important.

The next rule is that on each day at least one film is shown. So we can encode that rule as follows:
```prolog
?x[at_least_one_film_per_day->\true] :-  
    ?x:Schedule[screenings->?_y1:Screening[day->Thursday]],  
    ?x:Schedule[screenings->?_y2:Screening[day->Friday]],  
    ?x:Schedule[screenings->?_y3:Screening[day->Saturday]].
```
The next rule is that only one film is shown at a time. Again, we have negated this rule in the definition of a valid schedule, so here we need to figure out what makes it true. It is true if there are two different screenings on the same day and with the same slot.
```prolog
?x[more_than_one_film_at_a_time->\true] :-  
    ?x:Schedule[screenings->?_y[day->?_yd,slot->?_ys]],  
    ?x:Schedule[screenings->?_z[day->?_yd,slot->?_ys]],  
    ?_y !== ?_z.
```
Now we are left with the rules about a valid Thursday, Friday, and Saturday schedule. The rule for Thurdsay is that harvest must be shown, and it must be shown last.

As it happens “last” is something that we are going to use more later, so it is useful for us to encode the meaning of “last” separately. We will say that a Schedule has a function called last. If you give that function the name of a day, it will return the name of the film that is shown last on that day.

This will allow us to use the short hand `?x[last(Friday)->?film]` when we need to refer, for example, to the last film on Friday.

So here, the head of our rule describes the function, and the body of our rule calculates it.
```prolog
?x[last(?day)->?film] :-  
    ?x:Schedule[screenings->?_s[day->?day,slot->?_slot,film->?film]],  
    \naf ?x[screenings->?_os[day->?day,slot->?_[after->?_slot]]].
```
So this reads as follows:

“We know that a film called FILM is the last film of day called DAY in a object called X when we can prove that X is a Schedule with a screening s, which has a day, slot, and film; and it is not true that there is a screening OS in schedule x with the same day, and a slot that is after S’s slot.”

Here we do not need to say explicitly that S and OS are different objects, because we have already said that OS would need to have a slot that is after the slot for S.

Remember that we defined the property “after” for slots above.

So now we have a way of referring to the last film on a given day.

Let’s go back to the rule for Thursdays.. Harvest must be shown, and it must be shown last. We already know that Harvest must be shown once per festival, but there is no guarantee that it is being shown on Thursday, so we need to check to see if harvest is shown on Thursday, and if it is last on Thursday.
```prolog
?x[thursday_rule->\true] :-  
    ?x:Schedule[screenings->?_s[day->Thursday,film->Harvest]],  
    ?x[last(Thursday)->Harvest].
```
The rule for Fridays is that either Greed or Limelight is shown, but not both, and whichever is shown is shown last. So we need to look at two possibilities, either Greed is shown last on Friday (in which case we know it was shown, and nothing was shown after it) and Limelight isn’t shown, or Limelight is shown last on Friday and Greed isn’t shown.
```prolog
?x[friday_rule->\true] :-  
    ( // either  
        ?x:Schedule[last(Friday)->Greed],  
        \naf exists(?_s)^?x[screenings->?_s[film->Limelight,day->Friday]]  
    ) \or (  
        ?x:Schedule[last(Friday)->Limelight,screenings->?_s],  
        \naf exists(?_s)^?x[screenings->?_s[film->Greed,day->Friday]]  
    ).
```
Here you can see the `\or` command separates the two sets of possibilities, that are surrounded by parentheses. Each option (Greed and no Limelight, or Limelight and no Greed), is structured as two conditions. First, the film we are looking for was shown in a given schedule. Second, there are no screenings for that show for other film.

This second condition is expressed as `\naf exists(?_s)^?x[screenings->?_s[film->Limelight,day->Friday]]`. You might be wondering why we need the `exists(?_s)^` syntax in there. The reason is that `\naf ?x` in Ergo Lite means exists(not(?x)). Or put another way “is there an object in the database for which the value of ?x is false”. But you don’t want to know whether there is a screening that is not Limelight on Friday. What you want to know is whether it is true that there are no screenings of Limelight on Friday, or not(exists(?x)). For that sort of query, you need to be explicit about where the existential qualifier goes.

We will use the exact same structure for the Saturday rule, which requires that either Greed or Harvest is shown last, and the other isn’t shown.
```prolog
?x[saturday_rule->\true] :-  
    ( // either  
        ?x:Schedule[last(Saturday)->Greed],  
        \naf exists(?_s)^?x[screenings->?_s[film->Harvest,day->Saturday]]  
    ) \or (  
        ?x:Schedule[last(Saturday)->Harvest],  
        \naf exists(?_s)^?x[screenings->?_s[film->Greed,day->Saturday]]  
    ).
```
That’s it. We have encoded the pre-amble. In the [next installment](https://medium.com/@jason_90344/a-computer-takes-the-lsat-generating-fact-scenarios-3a52fd6fe908), I will show you how you can use Ergo Lite to generate all possible Valid Schedules.

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.