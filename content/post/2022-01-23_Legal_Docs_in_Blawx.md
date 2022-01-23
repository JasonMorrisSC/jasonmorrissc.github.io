---
title: "Encoding Legal Documents in Blawx"
date: 2022-01-23T11:26:10-07:00
draft: false
toc: true
keywords: ['blawx']
---
I've been spending some time recently working on adding a way to describe
legal documents to the development version of Blawx.com.

## Objectives

Big picture, I want to have a data structure inside Blawx that is going to
allow people to cite the authority for rules using a specific reference, as
pictured here:

![](/rule_auth_cite.png)

### Importing

The structure should allow the user to add to and modify information imported
from a structured source, such as a LegalDocML file.

### Minimize Repeated Text

As much as possible, any text that occurs once in a legal document should
occur once in the encoded version.

### Matching Structure

If the law is divided into parts, divisions, sections, sub-sections, etc., all
of those arbitrary divisions should be represented in the encoding, so that it
is possible to refer to any subsection.

### Intelligent Quoting

If the user refers to a specific sub-part of a legal document, Blawx should be
able to tell from the encoding what portions of the text of the document need
to be displayed to the user in order for the text of that section to make sense.
This requires annotating which sections of text are continuations of which other
sections of text.

### Intelligent Pin-Point References

Laws are divided into many different types of sub-divisions, but not all of them
are always used for pin-point citations. For example, "Act, Section 3(1)(a)" might
actually be "Act, Part I, Division 1, Section 3(1)(a)" if you used the full "path",
but we don't do that. So the system needs to distinguish between path elements
that should be used when naming the sections, and those that should not.

Ideally, I want something that is going to automatically generate a tree-shaped
structure of pin-point references, each of which can be dereferenced into a useful
section of the legal text.

### Annotations

Sometimes legislation is not particularly well sub-divided, and a piece of code
may refer to only a portion of a long paragraph of text. In those circumstances,
I want the user to be able to create their own names for parts of the text, so
that they can refer to those parts specifically when specifying the authorities
for their code.

### KISS

And, of course, the objective is to keep it as simple as possible.

## The Current Version

My current version is a set of interface elements that looks like this.

![](/legal_doc_blocks.png)

The parts are:
* A root node for a legal document, which can contain any number of the other elements
* A sub-node element with a name that should become part of pin-point references, which can
  contain any number of any of the non-root elements.
* A sub-node element with a name that should not be part of pin-point references,
  which can contain any number any of the non-root elements.
* A text element that can be printed without a prefix.
* A text element for text that should be printed with the previous text element.
* A text element for text that should be printed with the text of its parent element.

### An Example

It's still very much a work-in-progress, but here's an example of what it looked like 
when I used the current set to encode
a portion of the Old Age Security Act:

![](/legal_doc_example.png)

I feel like if I referred to `OAS Act, section 3(1)(b)(iii)(non-resident requirement)`
Blawx would be able to tell that section will not make sense without the text that
follows it, and without the text in section 3(1) and the text in section 3(1)(b), so
despite the fact that everything has been typed only once, it should be able to
generate something intelligent.

## Still To Do

The structure does not yet distinguish between names that occur in the legislation,
and names that are only being used for the convenience of the developer. If it did,
the output could provide the text of the pin-pointed section in context, with only
the relevant section highlighted, which would be an improvement.

It seems like there might also be an opportunity to distinguish between conjunctive
and disjunctive lists, in a way that might allow some portions of the rules to be
pre-built for the person doing the encoding. But for now I'm not trying to capture
any semantic meaning, so that will be a challenge for later.

It would also be worthwhile to look into adding URI references for the sections
that have them. It would also be nice to be able to change from one sort of text
block to another without needing to drag out a new block. There are lots of issues
with it, still. But for minimum functionality I think it's almost there.
