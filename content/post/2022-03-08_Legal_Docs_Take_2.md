---
title: "Legal Documents in Blawx, Take 2"
date: 2022-02-08
draft: false
toc: true
keywords: ['blawx']
---

## Background

A [couple of blog posts ago](/post/2022-01-23_legal_docs_in_blawx/) I talked about how I was working on a way of getting information about statutes into Blawx,
so that they could be used for generating explanations, cross-references, and links to source material.

Playing around with it that way gave me what I needed to know in terms of the data structure that I needed to fill,
but it also taught me that the block-based interface was painful for drafting rules. If the rules could be imported
into that format, it might be convenient enough for editing them, but for writing it was unwieldy.

So I've been looking for something as a user interface that has the following features:

* it lets the user get all the information in that is required
* it adheres to the "don't repeat yourself" principle, so you only need to type things once
* the same interface can be used to edit the content of a rule, and also to select elements in that rule
* it doesn't suck if you need to type the law from scratch

I've come up with another prototype, that I'm a lot happier with. It's based on the [treed](https://jaredforsyth.com/treed/)
interface by Jared Forsyth, which is an edit-in-place tree interface in JavaScript.

![](/treed_prototype.png)


It's pretty straightforward, with a couple of little niggly bits for pin point citations and text completion.

## Short Pin-Points

The root element and any titled element list (e.g. Section, Subsection, Part, Division) can be followed with
parentheses that set out whether and how to refer to that element when generating pin-point references.

If:
* the parenthesis is empty, include the specific indicator if any
* the parenthesis says "none", do not include either the element name or the specific indicators
* otherwise, include the text in the parenthesis, followed by the specific indicator


So in the above issue, if you clicked on the words "this thing is true, or", Blawx would know that the short-form
pin-point citation for that section is `OAS s 1(a)`. `OAS ` is taken from the root, "Part" and "I" are ignored,
section is appreviated to `s `, with `1` appended for the specific section, and `(a)` appended for the sub-section.

## Text Completion

If Blawx wants to display to the user the text of `OAS s1(a)`, above, it needs to know how the pieces of text
relate to another. That is done by allowing the user to put elipses at the start and end of the text elements.

An elipses at the start of a text element indicates that it is a continuation of the text in the previous element
in the same list. An elipses at the end of a text element indicates that is continued by the next element in the
same list.

Because there is a "..." at the end of the first block of text and at the start of the last block of text in s 1,
Blawx will be able to determine that the relevant text for `OAS s 1(a)` is as follows:

```text
OAS s 1
This section says this thing if
...
(a)
this thing is true, or
...
and this final thing is true.
```

## Why I Like It

It's basically like typing. You can use enter, tab, shift-tab just like you were editing a multi-level list in
Microsoft Word. So the interface is intuitive. You can also collapse lists to make it easier to find things,
and you can select individual nodes to indicate which part of the rule you are currently encoding.

Plus it gets us everything we need to do text completion and short pin-point citations.

And, because it's just a list of lists, if the legislation does some strange inconsistent things in terms of
how sections are named, or numbered, the interface doesn't care. Just type whatever the law did, and indicate
how to cite it. If you want to go from Part A to Division 2 to Appendix III, you can. It's not trying to
impose a structure, just reflect the structure that is actually there.

Also, information that is relevant to how text is displayed, the elipses, are placed with the text they modify,
in a way that limits how much extra typing is required. If you are about to create a list of subsections, you can
add one elipses to the end of the text, and all the subsections will continue that text.  Information about
the section names, like how to abbreviate them, and whether to print them, is also with the section names, in a way
that avoids the need to say the same thing more than once.

## Why I Don't Like It

Avoiding the need to say the same thing more than once means that the "list of sections" and "individual section"
need to be two different elements, which makes the tree unnaturally "deeper" than it needs to be. But I can't
think of a way of solving that problem that doesn't cause a different problem when a section has two
distinct sub-lists, which doesn't happen a lot, but does happen.

It also doesn't deal with URLs to source material, yet.

The treed interface also has some bugs. Those can probably be worked out, but it will take some doing.

It is also under-specified as to how Blawx can distinguish between a heading (which has no identifiers underneath it)
and a name for a list of identified clauses, like "Section". It might be a thing that is implemented with
reserved words, but I'm not sure.

## The Plan

I've made a bunch of changes to [Blawx](https://www.blawx.com) over the last couple of weeks. There are a handful
of features to add to it, but linking code to sections of a legislative text, and using those linkages to power
defaults and exceptions is what will take Blawx from a logic programming tool to a Rules as Code tool, in earnest.

I'm hoping to be able to get that far by the end of March, and in April make some progress on a method of
automatically generating DataLex-like user interfaces for Blawx queries.

