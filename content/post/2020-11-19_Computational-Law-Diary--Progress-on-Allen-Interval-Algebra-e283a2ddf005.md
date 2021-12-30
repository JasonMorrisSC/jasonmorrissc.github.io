---
toc: true
title: 'Progress on Allen Interval Algebra'
description: >-
  This has been a very busy week, with 3 guest lectures at Sciences Po, and a
  Rules as Code presentation for the OECD’s Governing After…
date: '2020-11-19T17:11:28.103Z'
categories: []
keywords: [flora2]
slug: >-
  /@roundtablelaw/computational-law-diary-progress-on-allen-interval-algebra-e283a2ddf005
---

This has been a very busy week, with 3 guest lectures at Sciences Po, and a Rules as Code presentation for the OECD’s Governing After Shock web conference, plus a prep meeting for the Canada School of Public Service [Open Government conference next week](https://www.csps-efpc.gc.ca/events/digital-forum-intelligence/index-eng.aspx), which I understand is expecting more than 3000 attendees. If you’d like to register, follow the link.

But I have a little progress to report on the Allen Interval Algebra library I was [telling you about last week](https://roundtablelaw.medium.com/computational-law-diary-getting-smarter-about-time-7a72f5275ca7).

#### Shining Some Light on John

One example problem in temporal logic is the sentence “John was not in the room when I touched the switch to turn on the light.”

Using Allen algebra we need to break this sentence down into its intervals, and then set out what we know about the relationships between those intervals.

There are three intervals: the period of time during which John was in the room, the period of time during which I was touching the switch, and the period of time during which the light was on.

Using the code I’m working on, you can declare them as Allen Intervals like this.
```prolog
john_in_room:Allen_Interval.  
i_touch_switch:Allen_Interval.  
light_on:Allen_Interval.
```
#### Making the Links

You need to then describe what you know about the relationship between two of these intervals at a time, in terms of the 13 basic relations available in the Allen algebra.

For the relationship between John being in the room and my touching the switch, we know those two things did not happen at the same time. So the only remaining possible relationships are preceding, meeting, preceded_by, or met_by. So you can set that out explicitly like this:
```prolog
john_to_switch:Allen_Link[  
    from->john_in_room,  
    to->i_touch_switch,  
    relationship->john_to_switch_rel:Allen_Relationship[  
        precedes,  
        meets,  
        met_by,  
        preceded_by  
    ]  
].
```
With regard to the relationship between the light being on and touching the switch, we know it was off before I touched the switch, and turned on before or when I stopped touching the switch. And we know it stayed on after I stopped touching the switch. So the only valid relations are that touching the switch “meets” or “overlaps” with the light being on.
```prolog
switch_to_light:Allen_Link[  
    from->i_touch_switch,  
    to->light_on,  
    relationship->switch_to_light_rel:Allen_Relationship[  
        meets,  
        overlaps  
    ]  
].
```
#### Asking Questions

The point is to be able to infer things about the relationship in time between the intervals that we don’t have directly linking information for. So we want to ask questions about the relationship between John being in the room, and the light being on.

For example, we might ask “is it possible that John was in the room while the light was on?” In Allen algebra, the translation is to ask whether the relationship between the two intervals includes the possibility of one of the 9 different basic relations that involve some degree of overlap.

That question can be written like this:
```
john_in_room[strongest_implied_relation(light_on)->?_rel],  
?_rel[  
 overlaps;  
 overlapped_by;  
 finishes;  
 finished_by;  
 contains;  
 during;  
 starts;  
 started_by;  
 equal_to  
].
```
If you run that query, the answer is “yes.” It’s possible that john was in the room while the light was on, because he might have entered the room after it was turned on.

We can also ask questions like “is it possible that John was in the room before the light was on, and still in the room after it was off again?”

That question is easier to pose:
```
john_in_room[strongest_implied_relation(light_on)->?_rel],  
?_rel[contains].
```
And the answer you get back is “no”, because he was not in the room when the light was turned on, so he cannot have been in the room for the entire time the light was on.

#### A Timely Example of Formal Verification

One of the things that we are working on at the SMU Centre for Computational Law is creating a language that supports “formal verification” techniques. These are techniques to analyze and learn something useful about a set of encoded rules.

It turns out that Allen Interval Algebra provides an easy-to-understand example of how formal verification can work.

Let’s imagine that in order to fly during Covid, you need a negative covid test that was taken within 3 days of departure. And let’s imagine that in order to fly you need permission to land from your point of arrival, which requires a negative test result. And let’s imagine it takes two days to get a result of a Covid test, and it takes two days to get the permission.

You can clearly see that this is impossible. Two days for the test plus two days for the permission is 4 days, which is more than the maximum of three. So it doesn’t matter what you do, the rules are impossible to adhere to.

That’s obvious to us because the number of rules is small. But what if the overlapping rules were more complicated and the impossibility harder to see, or only happened some of the time? Could the computer notice a problem like that for you?

It turns out that with Allen Interval Algebra, it can. The trick is to try and derive Allen relationships between all combinations of the intervals, and combine that information with what you already know. If in the combined result there are any two intervals that have no relationship at all, then you know you have a set of rules that create an impossible-to-meet deadline.

I’m looking forward to being able to demonstrate that capability soon.

#### Next Steps

I anticipate that when I model “events” in code, each event will also becomes an Allen interval. And if we know exactly when the events occurred-that is, we have dates and durations for them-then we can derive the Allen relationships on the basis of that information, too.

But what using Allen interval algebra allows you to do that you can’t do with just dates and times is to give relative time information. Instead of saying “I got the ticket on January 7”, you can say “I got the ticket at some time prior to when my license expired.” Which may be all the temporal information you need in order to answer your question.

So the next steps are to integrate the algebra with a broader event system. Which means I need a broader event system. I’m currently reading up on Event Calculus. I will probably implement an event calculus system by itself, first, and then think about how difficult it would be to allow you to use Allen relationship data instead of dates and times in those events.

### Quick Notes

*   Nov 24: “Rules as Code” session with Canada School of Public Service as part of their [Open Government conference](https://www.csps-efpc.gc.ca/events/digital-forum-intelligence/index-eng.aspx).
*   Jan 12–14: Presenting on Blawx at the Legal Services Corporation Innovations in Technology (Virtual) Conference.
*   TBD (Jan ’21): Cyberjustice Laboratory (University of Montreal) webinar on [Blawx](https://www.blawx.com/).