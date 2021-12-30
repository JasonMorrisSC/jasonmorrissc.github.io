---
toc: true
title: Blawx Dev Notes
description: >-
  I took the opportunity over the last couple of days to spend some time working
  on long-planned upgrades to Blawx. It has been partly…
date: '2021-01-17T16:37:02.910Z'
categories: []
keywords: [blawx,flora2]
slug: /@roundtablelaw/blawx-dev-notes-ca2bdf03cf3b
---

I took the opportunity over the last couple of days to spend some time working on long-planned upgrades to Blawx. It has been partly successful, and partly an exercise in frustration.

The frustration comes from the fact that I’m working with limited processing power here in my hotel room in Singapore, having only brought a Surface Go tablet with me for writing code. To get any further, I think I’m going to have to bite the bullet and pay for a development server.

But here’s what I’ve accomplished so far…

### Dates, Times, and Durations

![](/1__gEtXYkd8yUBOv__HnrPp9rQ.png)

Blawx features strings, numbers, and booleans, and I have wanted for a very long time to expand that to include dates, times, datetimes, and durations. That process is now complete. The dev branch has new datatype blocks, new data value blocks, and I also implemented a number of date math functions.

![](/1__T__AMwQTIUmEnpoOigvfk4w.png)

One of the frustrations of dealing with Flora-2 has been how it does not have all the same date math methods built-in that its commercial cousin ErgoAI does. So last week I wrote some Flora-2 code to fill that gap. That code has been added to Blawx, and powers the blocks which allow you to calculate the duration between two dates or datetimes.

### String Methods

![](/1__gzHZmRrDy9bEg3UYoCVBeQ.png)

While I was at it implementing date methods, it seemed like a good opportunity to add some string methods. So the dev branch now has a bunch of methods for manipulating text.

Making the string functions work required moving away from the default string block definition, which makes it reverse incompatible.

### Emojis vs. Inline Graphics

One of the things that I have struggled with in the Blockly interface is that while you can choose a colour for the block to indicate it’s type, or the type of data it deals with, there is no obvious way to indicate the data required by a specific input inside a block.

Some people have proposed colouring the blocks as a gradient in order to indicate what data type goes where, but I’m not a fan.

![](/1__HyK9eTHjAu__Nnz__VPvM8DQ.png)

I’ve decided to try using emojis to represent data types, and put those emojis to the left of inline value inputs that require a particular type of data. I’m not 100% I’m going to stay with that design, but I’m not hating it, yet, either.

At the same time, in the course of generating a custom string value block I learned how Google has added inline images to their blocks, and I’m considering going back to the drawing board and seeing if there is something a little more monochrome and consistent that I could design. Part of the problem with using emojis is that they are going to show up differently depending on what system they appear on.

So don’t get attached.

### Renaming Strings and Booleans

I’m intent on keeping things as simple as I possibly can, which is why for a long time I have wanted to rename “string” inside Blawx to “text”, which makes more intuitive sense. That is now a part of the development version, and at the same time I have renamed “True/False” to “Yes/No”. I may change my mind on that one.

### Rewriting the Reasoner API in Python

I spent the better part of 12 hours working on rewriting the reasoner API in Python today, and that was not any fun at all. The libraries that I thought I was going to use turned out to be so badly written I needed to abandon that plan and effectively rewrite the parts that I needed. Then I ran into an obscure temporary file glitch that took me hours to debug. Then I needed to learn the right Apache and Docker incantations in order to get python scripts to respond to API requests.

I’m still not done, because of the above-mentioned processing limitations, but I’m close. And this is a thing that really needed to happen, because the old reasoner code was forcing the interface to slow down to a crawl in order to be able to deal with how Docassemble reports data with circular references.

It’s a whole thing.

Once I can get a dev environment with some actual horsepower, I should be able to rebuild it, test it with external data, and see if it actually works or not.

### Cardinalities

In my dream demo, you create Blawx code, draft a query in Blawx, and then that query gets auto-magically turned into an online app.

In order for that to happen, the app needs to know what data to collect, which means it needs to know _how many_ pieces of information you might need to collect. Which means that I need to update the attribute definition blocks to deal with cardinalities.

The code is written, but it hasn’t even been pushed to dev, yet, because I haven’t been able to test it. But here’s what it’s going to look like when I can.

![](/1__lpkAFozbefOFpqGO6fIOpA.png)

### What’s Next?

I am focused on a demo that I want to give at some point in the future. In this demo, a person opens up Blawx, imports some pre-written Rules as Code, and writes a question. Then they click a button, and they are taken to a Docassemble interview that collects the required information, answers the query, and gives them an explanation for that answer with links to source material.

So in the medium term, I’ll be working on explanations and auto-generated interviews, and all the interface elements required for both of those things.

In the shorter term, there’s a lot of progress sitting in the dev branch right now that needs to be pushed to 0.2.3-alpha. But that can only happen after I’ve done some more tests, and updated the documentation and some live demos that I’m committed to maintaining.