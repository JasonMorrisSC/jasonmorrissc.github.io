---
title: Constraining Dates in s(CASP)
description: A demonstration of using constraints over reals in s(CASP) with dates.
date: 2021-12-30
draft: false
toc: true
keywords: [scasp,dates,constraints]
---
I've been playing around with an s(CASP) library for doing date math over
the last little while, and finally found some time today to test it with
s(CASP)'s ability to do constraints over reals. I was interested to see
if there was a way that we could convert the dates into timestamps and then
have s(CASP) treat them as a constraint.

### The Problem

Here's my imaginary scenario. I am allowed to do something on any day that is in both the first and last 200 days of 2021. Which days can I do it?

### The Code

```prolog
#include '../dates.pl'.
valid(Start,End,X) :-
    date_add(date(2021,1,1),duration(1,0,0,200),End),
    date_add(date(2021,12,31),duration(-1,0,0,200),Start),
    epoch_date(End,E1),
    epoch_date(Start,E2),
    X #< E1,
    X #> E2.
?- valid(Start,End,X).
```

The `valid/3` predicate tells me what the start date of the valid period is,
what the end date of the valid period is and what the timestamps of all the
valid days in that period are. This is the interesting feature of s(CASP),
that it is possible to treat things as constraints that may not be resolved
before the answer is provided.

The first condition says "add 200 days to the first of the year, call that 'End'". The second says "subtract 200 days from the last day of the year, call that 'Start'". The next two lines convert the Start and End dates into
timestamps, and the last two apply the constraints. A valid timestamp must
be later than the start time and earlier than the ending time.

## Output

Here's the output:

```text
% QUERY:?- valid(Start,End,X).

        ANSWER: 1 (in 525.172 ms)
...
BINDINGS: 
Start = date(2021,6,14) 
End = date(2021,7,20) 
X #> 738261,X #< 738297 
```

So the valid dates are between June 14 and July 7, with timestamps between 738261 and 738297.

How is that useful? Essentially, it allows
us to ask useful questions without providing specific fact scenarios.

If we had used the non-constraint versions:

```prolog
    X > E1,
    X < E2.
```

Then the query `valid(Start,End,X)` would have failed, because `X` is
never bound to anything. There is no way to choose what `X` should be unless
the user provides a value for X.

By using constraints we don't need to provide as much information in order
to be able to ask useful questions, and get useful answers. And the
constrained values can propogate through your code. A constraint on the
application date automatically becomes a constraint on the approval date,
for example.

This is similar to the timepoint algebra I was working on about a year ago,
but much easier to use.