---
title: "Avoiding the Clause Order Hassle in s(CASP)"
date: 2022-01-09T21:50:38-07:00
draft: false
toc: true
keywords: [scasp, "morris' laws"]
---

## The Clause Order Hassle Check

Learning to use s(CASP) to encode legislation over the course of the last year
or so has been at times frustrating. The reason for that is that I would often
find myself with code that did not work as expected, and I could not understand
why. Then, I would make changes, usually by changing the order of the lines
of code, or by changing the number of predicates I was using, and the code would start to work. But, I would
continue to not understand why. So I would continue to make the same mistakes over and
over, and need to spend time randomly trying things until the code worked.

In my head, this was the "clause order hassle." 

Recently Eric [pointed out a problem](https://medium.com/@eric64978338/hi-i-am-also-a-fans-of-logic-programming-7bcbba452d03) in some example code I had posted earlier
(thanks Eric!) that was a perfect example of the clause order hassle. The rule, defining the winner of a game of Rock, Paper, Scissors; would fail if
one line was first, or second, or fifth through ninth. But the code worked
if the clause was third or forth.

I asked the experts about it, and they gave me a little context
about why it was expected behaviour, if completely unhelpful. So I 
decided to use that insight to finally actually figure out what was going on.

I _think_ I managed it.

## You Can't Be Your Own Sister

There's a stereotypical example in writing logical code of the definition
of "sibling." The new coder forgets to include things that seem intuitive
to a person, but which, because of the way Prolog searches, are necessary
to make explicit.

This code seems correct at first glance.
```prolog
siblings(X,Y) :-
  parent(X,Z),
  parent(Y,Z).
```
Basically, "two things are siblings if they have the same parent." But in prolog, just because you named one variable X and the other variable Y doesn't
meant that they can't both refer to the same thing. Unless you want to say
that someone is their own sister, you need to add an extra line.
```prolog
sibling(X,Y) :-
  parent(X,Z),
  parent(Y,Z),
  X \= Y.
```

That's a simple example of how you have to understand the operational
semantics of the langauge you are using in order to know how to correctly
model things. The problem I was having with s(CASP) is the same sort. It
turns out that the problem I had arises in this code, too.

## s(CASP)'s shortcut

One of the features of s(CASP) is that it has default negation, and it automatically generates programs, called "duals" that describe when the default negation applies. All of which just means that when something is
not true, s(CASP) can explain why.

What follows is an simplification, but here's the sort of code that s(CASP) would
generate in the background for `sibling/2` above.
```prolog
not sibling(X,Y) :-
  not parent(X,Z).
not sibling(X,Y) :-
  parent(X,Z),
  not parent(Y,Z).
not sibling(X,Y) :-
  parent(X,Z),
  parent(Y,Z),
  X = Y.
```
The first one means "X doesn't have a parent." The second one means "X has a parent, but X and Y
do not have the same parent." The third one means "X and Y have the same parent, but X and Y are the same thing." Because s(CASP) has helpfully
generated this code for you, if you ask why two people are not
siblings, s(CASP) can give you a reason based on one (and helpfully, _only_ one) of those three rules.

That's a very nice thing to have when encoding laws.
If I tell someone that they qualify for
a benefit, they are likely to say "great!" It's when I tell them that they
_don't_ qualify that they are likely to way "why?" Being able to answer that
question with the minimum amount of hassle possible is exceptionally valuable.

So we want to work _with_ this feature of s(CASP).

## The Problem

The issue is that there are circumstances in which the code that s(CASP) 
writes for the dual
program allows for conclusions that are incorrect. For instance, if
we changed the code above by just changing the conclusion to `has_sibling`
we get this...
```prolog
has_sibling(X) :-
  parent(X,Z),
  parent(Y,Z),
  X \= Y.
```
Now the duals are going to be exactly the same,
but with the conclusion `not has_sibling(X)` instead of
`not sibling(X,Y)`.

The third dual will be
```prolog
not has_sibling(X) :-
  parent(X,Z),
  parent(Y,Z),
  X = Y.
```

In English, that's still "X and Y have the same parent, but X and Y are the same 
thing". When we are trying to prove that X and Y are not siblings, that's a good rule to follow. But if we are trying to figure out if X has any siblings at all, it's not.

This code will report that a person both has and does not have a
sibling at the same time. That's obviously no good.

This is the source of the clause order hassle problems that I didn't
understand before. But now that we know the source, we can avoid it.

## The Rule of Thumb

I have come up with what I think is the simplest
rule of thumb to follow that should help avoid the clause order hassle
in future. Maybe we'll call it "Morris' First Law of s(CASP)..."

> Don't include conditions where the opposite condition is not sufficient
> proof of the opposite conclusion.

The problem with `has_sibling/1` above is the requirement that the
two people are not the same person. While it is required for the predicate to
hold, is not sufficient to prove the opposite when reversed.
That is to say, just because two people with the same parent are the same 
person doesn't mean the person doesn't
have a sibling. So including the disequaility `X \= Y` violates the rule of
thumb.

## How to Follow the Rule of Thumb

If you run into a situation where you have a clause that is a necessary
condition of "yes", but not a sufficient condition of "no", you have two options:
* change the conclusion (e.g. from `has_sibling/1` to `siblings/2`), or
* replace the offending clause with an intermediary predicate, of which it
  **is** sufficient proof of falsehood when negated, and make it
  a condition of that new predicate.

For an example of the second option, here is what I ended up doing in my Rock, Paper, Scissors code.
This code:
```prolog
winner(Game,Player) :-
  game(Game),
  player(Player),
  player(OtherPlayer),
  participate_in(Game,Player),
  not_same_player(Player,OtherPlayer),
  throw(Player,Sign), 
  participate_in(Game,OtherPlayer),
  throw(OtherPlayer,OtherSign),
  beat(Sign,OtherSign).
```
turned into this code:
```prolog
game_has_two_different_players(Game,Player,OtherPlayer) :-
    game(Game),
    player(Player),
    not_same_player(Player,OtherPlayer),
    player(OtherPlayer),
    participate_in(Game,Player),
    participate_in(Game,OtherPlayer).

winner(Game,Player) :-
  game_has_two_different_players(Game,Player,OtherPlayer),
  throw(Player,Sign),
  throw(OtherPlayer,OtherSign),
  beat(Sign,OtherSign).
```

In the first version, `not_same_player` violated the rule of thumb. In the
second version, I have added a new predicate, `game_has_two_players`, where
`not_same_player` is both a necessary condition of true, and its opposite is
a sufficient condition for false. Then I have used that predicate instead in
the definition of `winner`.

## Caveats

This is a rule of thumb. It is not the only, or in some cases even the best
way of avoiding this problem with dual programs. It just seems like the rule
that will be easiest to remember and follow, so I'm going to try it.

Also, using this rule unfortunately does not eliminate all of the circumstances
in which the order of the clauses matters. There are other rules of thumb
that need to be followed, too, for best results.

But I'm hoping it will save me some time and effort later this year when I start
comparing s(CASP) code to code written in other languages. And I hope that sharing it saves at least one person the many hours I spent changing the order of clauses in s(CASP) for no reason.