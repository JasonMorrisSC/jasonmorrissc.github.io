---
title: "Explanations for Defeasibility in an Open World Semantics"
date: 2022-03-25T14:26:31-06:00
draft: true
---
As you might have been able to tell from the title, we're going to nerd
out pretty hard, in this one.

## The Problem: Too Much Info

Currently, I'm working on adding defeasibility to the v1 series of Blawx.
I've made a lot of progress, but I've run into a design problem, and I don't
really know yet how I'm going to solve it.

The problem is, Blawx is using s(CASP) for its reasoning engine, and I have created a defeasibility method, but s(CASP) uses an open world reasoning,
which means that its explanations for what's going on in a defeasible world
are... complicated.

## The Example: Tweety is a Penguin

Let's imagine that we're encoding the Bird Act, which reads:

```
1. Penguins are Birds.
2. Birds fly.
3. Despite section 2, penguins don't fly.
```

We have a default in section 2, and an exception in section 3. We encode those
in Blawx, and then we create a test case that basically says "Tweety is a Penguin."

Now, what questions can we ask about this fact scenario, and what sort of answers will we get?

The defeasibility method that I'm working with uses a description for when a
conclusion holds, legally. We can use that to ask what conclusions hold,
and what conclusions do not hold.

## What Conclusions Hold?

If I ask the reasoner what legal conclusions ultimately hold, we get the
expected single answer that tweety does not fly. But the explanation is
pretty complicated.

* Tweety does not fly, because
  * According to section 3, tweety does not fly, and
  * there is no evidence that the conclusion in section 3 is defeated, because
    * according to section 3, tweety does not fly, proved above,
    * there is no evidence that the conclusion in section 3 is rebutted, because
      * the conclusion that tweety flies and the conclusion that tweety does not
        fly are opposed, and
      * section three does not overrule itself, and
      * section three is not overruled by anything other than section three, and
      * nothing other than tweety flies opposes the conclusion that tweety does
        not fly.

That's a lot. And while it's consistent with the reasoning that's implemented in
Blawx's defeasibility theory, it's probably too much information most of the time.

But it gets worse.

## What Conclusions Don't Hold?

You might also ask Blawx what legal conclusions don't hold. That's a bit dangerous, because it's not actually what you mean. But Blawx will answer it happily:

* Everything concluded by something other than section two and section three,
* Everything concluded by section 3 other than tweety doesn't fly.
* Everything concluded by section 2 other than tweety does not fly.
* The conclusion that tweety does not fly in section 2.

Only the last one is what we were really interested in, so we should have asked
"what conclusions that were defeasibly held by any section of the law did
not ultimately hold legally", and we would have gotten only the last answer.

Why do the other answers exist? Again, just because we didn't tell Blawx that
section two reaches any other conclusions doesn't necessarily mean we know
for sure that it doesn't. The possibility needs to be accounted for.

The explanation for the one answer to the question we meant to ask 
there is a little more straightforward.

* There is no evidence that it legally holds that tweety flies, because
  * according to section two, tweety flies, and
  * the conclusion in section two is defeated, because
    * according to section two, tweety flies, proved above, and
    * the conclusion in section two is rebutted, because
      * the conclusion that tweety flies opposes the conclusion that tweety does
        not fly, and
      * section three overrules section two, and
      * according to section three tweety does not fly.

## The Significance of Clause Order

One of the things that I dislike about this is the impact of the order of
the clauses in the defeasibility library for Blawx. You'll note that in answering
the negative version of the query, it starts by saying what all the sections are,
and then for each section considers all the possible conclusions, precluding
each. It might also have started with conclusions, and then for each conclusion
gone one-by-one through the relevant sections. Why didn't it? Because the
requirement of the conclusion opposing is listed first in the code for the
defeasibility library.

s(CASP) is making a choice to minimize the number of redundant explanations that
show up, and that's probably ultimately the best you can do in an open world,
but it is a design factor for how I write the libraries for Blawx, and it's
ultimately going to be something that people writing Blawx code might need to
be aware of, too.