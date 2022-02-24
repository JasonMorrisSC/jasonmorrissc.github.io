---
title: "Progress on Blawx, SMU Conference, Beeck Centre Rules as Code Paper"
date: 2022-02-24T11:52:35-07:00
draft: false
toc: true
keywords: ['blawx','akoma ntoso','smucclaw','CLSC2022','Django','Beeck Center']
---

## Progress on Blawx

Just a quick note to let you know what I've been doing with Blawx over the last few weeks.

Blawx was originally a single web page that could contain a single workspace of code, much
like an IDE that could only have one file open at a time. Version 1 changed that by creating
"workspaces" that are saved server-side, and giving you the ability to create multiple
workspaces and edit each one separately.

For the last few weeks I've been working on allowing the user to  organize their code by
the section of legislation it corresponds to.

### Deciding on Akoma Ntoso

The first question to answer was how to model the structure of the legislation inside the
system. The options were basically to model the legislative structure inside the Blawx
database directly, or to store text in the database that described the details of the legislation.
I decided on the latter, partly because it simplified the database schema for the app, but
also because it allowed me to integrate with other modern tools that are dealing with
legislative text much more easily, by using the Akoma Ntoso standard as the internal
representation of legislation.

I was bolstered in this choice by the work of Laws.Africa, which is a Legal Information Institute
for Africa, and has developed a series of tools for generating and parsing Akoma Ntoso, for manipulating
Akoma Ntoso in Python, and the Indigo Platform for legislative life cycle management, all of which
are open source.

### Changing the Interface

Once I decided that people would input laws using Akoma Ntoso markup, I needed to figure out how to
get Blawx to automatically turn an Akoma Ntoso document into a navigation tool that would allow
the user to quickly and easily find the section of law they were interested in and review or create
some Blawx code for that section.

Here's what the current development version looks like.

![Blawx Dev Screenshot](/blawx_legal_doc_preview.png)

The left side is a collapsible nested legal document with radio button selectors for each section of law,
and the right is the Blawx code for the selected section. You can see in the example that I'm using a
version of the rules of Rock Paper Scissors as the law, and the selected section and encoding covers the
idea that signs are a thing, and that there are three of them.

The format on the left is a little messed up. The [Slaw](https://github.com/laws-africa/slaw) tool from [laws.africa](https://github.com/laws-africa/) that I'm using to
generate Akoma Ntoso is based on a particular style of legislative drafting. The
examples on which the navigation tool was built use a different style, so there are still a lot of rough edges to be 
sanded down.

Eventually, I would like to have a WYSIWYG editor for Akoma Ntoso built in, to allow easy access to editing
the law as you go, and to put a boundary around the kinds of Akoma Ntoso we might end up with. I'm hopeful that can be done, because it has been done in tools like LawMaker, from
LDAPP. Check out [their recent video](https://youtu.be/WBmwiHY4Q-Q). But as of now, 
a WYSIWYG Akoma Ntoso editor is not something that exists in open source, so it's going to have to be future work.

### Learning Django

I wont go into all the reasons that organizing code this way is a good idea, but it is. There are a lot of
really cool features that I want to add to Blawx in the near future that depend on getting this right first.
And I have already _felt_ the advantages in terms of user experience. It just feels like there is less
friction between what you want to do and what the app lets you do.

But my learning curve with Django is still very steep. I spend more time guessing and navigating error messages than
just "using" it. That's been frustrating, but I'm attempting to power through. If you're a Django genius,
and you'd be willing to help me out, let me know.

I'm hoping that the stuff I'm working on now should be ready to release, and push to the development server,
by next week. But the conference next week might delay things.

## SMU Computational Legal Studies Conference

But next week I'm also attending the 
[Singapore Management University Computational Legal Studies Conference](https://cclaw.smu.edu.sg/events/computational-legal-studies-2022), where I'm presenting on my paper on OpenFisca.
A [draft of that paper](https://jasonmorrissc.github.io/post/2022-03-02_openfisca_expert_systems/) is
also on the blog, if you are interested. The TL;DR is that OpenFisca is
great for microsimulation, and less helpful if you need reasons, relevance, or the ability to deal with unknown
inputs and still give useful answers.

The conference papers are under consideration for publication in the MIT Computational Law Report, and I'm
hoping before that happens there will be an opportunity to update the paper with an apples-to-apples comparison
of encoding the same rules in OpenFisca and the new version of Blawx.

Registration in the conference is free, the conference is virtual, online, and if you are interested in the space
the slate of keynotes they have are top-notch, including Kevin Ashley, and Daniel Katz.



## Rules as Code Paper from Beeck Center

I had never heard about the Beeck Center before this week, but they wrote [a report on using Rules as Code
in the US Benefits system](https://beeckcenter.georgetown.edu/report/benefit-eligibility-rules-as-code/), and it's an absolute must-read if you're interested in the space.

I'm delighted by it because it opens up the US market, which has seemed a bit of a black box, and because it
reveals that the smartest people in this space are already doing part of what Rules as Code advises: isolate the
legal logic in tools that are better suited to that purpose.  They just aren't doing the part where they
release the code publicly (probably for proprietary reasons), and they are still doing it in tools that are
sub-optimal for the job at hand.

