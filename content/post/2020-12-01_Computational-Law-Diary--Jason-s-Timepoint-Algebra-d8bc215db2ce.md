---
toc: true
title: 'Jason’s Timepoint Algebra'
description: >-
  If you’ve been following along over the last few weeks, you will know that
  I’ve been working on writing some code to do reasoning about…
date: '2020-12-01T00:26:14.898Z'
categories: []
keywords: [flora2]
slug: /@roundtablelaw/computational-law-diary-jasons-timepoint-algebra-d8bc215db2ce
---

If you’ve been following along over the last few weeks, you will know that I’ve been working on writing some code to do reasoning about time.

I’ve been working on Allen Interval Algebra, but in order to use Allen Interval Algebra you need to model your problem as a set of intervals. That is to say, you need to model the things that happen as happening over a period of time, as opposed to at a specific time. That feels a little strange, so I was curious whether it was possible to get the same benefit, but using only time points, not intervals.

I decided to find out if I could write some analogous code that only knew about “before, equal, and after,” but from those three properties could figure out the 13 properties in Allen Interval Algebra: “precedes, preceded by, overlaps, overlapped by, during, contains, finishes, finished by, meets, met by, starts, started by, and equal.”

It turns out you can. (And in the course of doing it, you learn a lot about how to write Flora-2 code.)

#### Jason’s Timepoint Algebra(TM)

The way the code works is that you define timepoints, and you define interval relationships between them, you add those definitions to a network of time points, and then you can ask questions about the network.

So to duplicate the effect of Allen Interval Algebra, we would need to say that there are four time points, `A, B, C, and D` the start and end of two intervals. And because we know that Allen Interval Algebra deals with intervals of non-zero duration, we can say that we know the relationship between two of them is `A<B` and `C<D`. Using my timepoints library, it would look like this:
```prolog
{A,B,C,D}:TimePoint.  
AtoB:IntervalRelation[from->A,to->B,relationship->tpr([before])].  
CtoD:IntervalRelation[from->C,to->D,relationship->tpr([before])].  
AllenIntervals:TimePointNet[point->{A,B,C,D},interval->{AtoB,CtoD}].
```
Now we can ask for all valid completions of the Allen Interval network, by saying this:
```prolog
AllenIntervals[completion->?x[valid]].
```
And what you get, in about 20 seconds, is a list of the 13 Allen interval basic relations. So it works, and not in an unreasonable amount of time.
```
flora2 ?- AllenIntervals[completion->?x[valid]].

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([after]), tpr([after]), tpr([after]), tpr([after])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([after]), tpr([before]), tpr([after]), tpr([after])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([after]), tpr([before]), tpr([after]), tpr([before])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([after]), tpr([before]), tpr([after]), tpr([equal])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([after]), tpr([equal]), tpr([after]), tpr([after])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([before]), tpr([before]), tpr([after]), tpr([after])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([before]), tpr([before]), tpr([after]), tpr([before])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([before]), tpr([before]), tpr([after]), tpr([equal])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([before]), tpr([before]), tpr([before]), tpr([before])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([before]), tpr([before]), tpr([equal]), tpr([before])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([equal]), tpr([before]), tpr([after]), tpr([after])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([equal]), tpr([before]), tpr([after]), tpr([before])])

?x = completion(AllenIntervals,[edge(A,C), edge(A,D), edge(B,C), edge(B,D)],[tpr([equal]), tpr([before]), tpr([after]), tpr([equal])])

13 solution(s) in 21.797 seconds; elapsed time = 22.069
```
I went through those 13 answers, and diagrammed them out, to confirm that they are in fact the 13 basic relations from Allen Interval Algebra. So it works!

#### Simple Gets Complicated Fast

Now we could theoretically use the code to ask interesting questions about other sequences of events: For instance, consider a game of Rock Paper Scissors. What we know is that the game starts when or before the players count, both players shoot after the count, both shoot at the same time as or before they see the other’s shot, each sees the other’s shot after the other player shoots it, and the game ends when or after each player sees the other’s shot.

If we set up that sequence of times and relationships in my time point algebra, we can ask “what are all the possible sequences of events that comply with those rules”, by typing this:
```prolog
rps[completion->?x[valid]].
```
Looks simple, right? But we have a network of 7 timepoints, with a total of 21 possible relationships between them, only 9 of which are known. That leaves 12 relationships, each of which could be any of three values (before, equal, or after), which means there are 3¹² different possible concrete completions of the network, each of which would need to be generated and then tested for validity. That’s 531,441 possibilities. Which is confirmed when we run this query asking Flora-2 to count them.
```
flora2 ?- ?count = count{?c|rps[completion->?c]}.

?count = 531441

1 solution(s) in 7.641 seconds; elapsed time = 7.891

Yes
```
The good news is that it was able to figure that out in less than 8 seconds. The bad news is even after throwing a couple of simple optimizations at the problem, my best estimate of how long it would take it to test them all for validity with the current code is 134 days on my laptop!

This is an example of the “combinatorial explosion” problem, which I think is part of the reason that Allen Interval Algebra is attractive. It reduces the complexity of the network by halving the number of entities. For every two timepoints, you have one interval instead. And reducing the number of entities has an exponential reduction in the number of relationships. If you can reduce from 7 entities to 4, you go from 21 possible relationships to 6. Which makes is faster to calculate inferences from data that fits into that model.

#### Next Steps

Luckily, the inability to quickly check for all possible completions is not an obstacle to what I want to do. The next step is to take the time point code and use it to model the relationship in time between events that are relevant to a contract.

### Quick Notes

*   Jan 12–14: Presenting on Blawx at the Legal Services Corporation Innovations in Technology (Virtual) Conference.
*   TBD (Jan ’21): Cyberjustice Laboratory (University of Montreal) webinar on [Blawx](https://www.blawx.com/).