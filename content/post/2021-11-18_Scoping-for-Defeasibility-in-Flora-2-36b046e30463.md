---
toc: true
title: Scoping for Defeasibility in Flora-2
description: >-
  In yesterday’s post, I talked a little about how it would be nice if there was
  a way to use legislative scope to refer to sections of law…
date: '2021-11-18T22:15:42.066Z'
categories: []
keywords: [flora2]
slug: /@roundtablelaw/scoping-for-defeasibility-in-flora-2-36b046e30463
---

In [yesterday’s post](https://roundtablelaw.medium.com/what-one-section-of-law-tells-us-about-what-rules-as-code-needs-a69a77784311), I talked a little about how it would be nice if there was a way to use legislative scope to refer to sections of law, particularly to implement, in a structurally-isomorphic way, defeasibility statements like “subject to this act and the regulations.”

Today I’m going to share a small experiment I did in Flora-2 to see if I can demonstrate a working approach. I’d be interested in your thoughts.

Remember that the objective of structural isomorphism is to have the smallest possible sections of code that correspond to the smallest possible sections of law, in a one-to-one relationship. Scoping is possible, defeasibility is possible. I’m specifically trying to find a way of doing scoping for defeasibility without damaging structural isomorphism.

### Our Law

Let’s pretend that we are dealing with the OAS Act, but that there are only two provisions we are interested in. Section 1 of the Act says “subject to this Act and the regulations, birds fly.” But there is a Regulation 1 under the OAS Act, rule1 of which reads “penguins do not fly.”

### The Library

I won’t share the code for the Flora-2 library I wrote, but suffice it to say it sets out a “LegalDoc” type, which includes “DocPart” elements, each of which has a parent element. LegalDocs also have a document type, and superior documents if they are subordinate to another document.

Using this library means that as you are encoding each section of the law, you document the new section as a “DocPart.” Then you use Flora-2 “tags” to associate that documentation with your rules. Then you can use the same names for those sections in your defeasibility statements.

If you’re playing with Flora-2 and would like to see how I implemented the LegalDoc type, let me know, I’ll be happy to share.

### Act, Section 1

Here’s how you would implement “Subject to this act and the regulations, birds fly.”
```prolog
oas_1:DocPart[parent->oas_p1,description->"section 1"^^\string].

@{oas_1}  
flies(?X) :- bird(?X).  
\overrides(?other_section,oas_1) :-  
  oas[descendent->?other_section[substantive->\true]];  
  oas[subordinate->?_reg[legaldoc_type->regulation,descendent->?other_section[substantive->\true]]].
```
The first line says “OAS Section 1 is a DocPart, whose parent is OAS Part I.”

The second line tags the following rule with the name of OAS Section 1.

The next line is the rule “birds fly.”

The next three lines encode “subject to this act and the regulations.” The first line says that the conclusion of Section 1 of the OAS can be overridden by another section. The next two lines set out two circumstances in which that is true. The first condition is when the section is a substantive part of the OAS Act. The second condition is when the section is a substantive part of a regulation that is subordinate to the OAS Act.

### Regulation Rule 1

Now we want to implement rule 1 of the regulation, which says only “Penguins do not fly”, without reference to which rules that might override.

Here’s how we might do that.
```prolog
reg1_1:DocPart[parent->reg1,description->"rule 1"^^\string].

@{reg1_1}  
\neg flies(?X) :- penguin(?X).
```
Again, the first line defines rule 1 of the regulation as being a child of the regulation itself. The second line tags the following rule as coming from that section of the law. The third line is the encoding of “penguins do not fly.”

### Maintaining Structural Isomorphism

You can see that this approach, of having documentation of the structure of the legal document, attaching elements of that structure to the individual rules, and then using the structure in order to specify which rules defeat which other rules, requires a little more work, but it allows you to faithfully implement statements like “subject to this act and the regulations” in a way that maintains the one-to-one relationship between the code and the law.

### Running the Code

If we create a fact scenario like this:
```prolog
bird(tweety).  
penguin(tweety).
```
We can then ask the question “does tweety fly” like this:
```prolog
?- flies(tweety).
```
And Flora-2 correctly answers “no”. Rule 1 of the Regulation says “no”, Section 1 of the act says “yes”, and Section 1 of the Act is defeated by Rule 1 of the regulation, so “no” wins.

### Other Uses for the LegalDoc Data Structure

You may have noticed that our encoding of the structure of the act included information about the description of each DocPart. This is done to demonstrate how the structure can be used for other benefits. For example, let’s say that you want to use the tags associated with a rule in order to help explain your answer. It would be useful to be able to have Flora-2 generate natural language descriptions for each section in a way that is consistent with the structure of the act. So I implemented a property of DocPart called “full_description”, which if you query combines the descriptions of the DocPart and all of its parents to generate a useful description of where that section of law is located.

For example:
```prolog
oas_1_b_i:DocPart[parent->oas_1_b,description->"paragraph (i)"^^\string].  
?- oas_1_b_i[full_description->?X].
```
Will return:
```
?X = "Old Age Security Act, section 1, subsection (b), paragraph (i)"^^\string
```
You may notice that the description excludes “Part I” of the OAS Act, which is the direct parent of section 1. While Parts are part of the legislative structure, they are not typically used to pinpoint legal text. So `oas_p1` exists as a part of the structure of the law, and you can use it to encode statements like “subject to the provisions of Part I”, but the code knows to exclude it from the description of a paragraph.

The data structure could likewise be extended with the actual text of the relevant section, whether the text of the section should be displayed as a continuation of the text of the parent section, a link to the source legislation, and other things that could be used to generate and improve natural language explanations for the conclusions of the rule.

### Thoughts

Flora-2’s defeasibility method is impressive, and easily extended in this way to be able to implement scopes. The only things on the list of features from [my last post](https://roundtablelaw.medium.com/what-one-section-of-law-tells-us-about-what-rules-as-code-needs-a69a77784311) that Flora-2 cannot do are some forms of date arithmetic, and hypothetical reasoning. Hypothetical reasoning might be possible to achieve, but date arithmetic has been intentionally removed from Flora-2 in order to nerf it in favour of its commercial cousin, ErgoAI. That is unfortunate, because the inability to subtract dates from one another to get a duration is probably fatal to the viability of Flora-2 in Rules as Code. And the closed source nature of ErgoAI, which adds explanations, and a wide array of libraries, is probably also fatal.

There is also a strong parallel between the kinds of information that are kept in the LegalDoc structure, and the kinds of information that are kept in XML Schemas like LegalDocML. In the future, it would be advantageous to be able to simply import the knowledge contained in a LegalDocML file, and then use the names of the elements in that document as the tags for your rules. Particularly if you live in a jurisdiction where LegalDocML is the format used, by default, to mark up legislation and regulations. Unfortunately, I don’t live in such a jurisdiction.