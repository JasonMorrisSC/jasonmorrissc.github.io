---
toc: true
title: Can Visual Programming Work for Lawyers?
description: >-
  I posted the image below to Twitter last night. It is a prototype I’m working
  on for a legal reasoning programming tool for lawyers.
date: '2018-12-27T19:12:05.291Z'
categories: []
keywords: [blawx]
slug: /@roundtablelaw/can-visual-programming-work-for-lawyers-b9f834fd32c8
---

I posted the image below to Twitter last night. It is a prototype I’m working on for a legal reasoning programming tool for lawyers.

![](/1__MhMJTZicTs45Esxi3cMduA.png)

I got an interesting reply about how visual programming attempts have usually failed, because they don’t scale well with complexity of the code, and they tend not to be integrated with the other software development tools. That sent me on a search for more detail, which led me to [this blog post](http://mikehadlow.blogspot.com/2018/10/visual-programming-why-its-bad-idea.html) by Mike Hadlow.

In it, he says that visual programming tools like scratch and UML are a bad idea (except, perhaps, for limited purposes like teaching), for three main reasons:

1.  The visual code is actually harder to read than the textual code, leading to one of two problems. Either the visual code is incomprehensible, or you end up doing the coding in the property dialogues, which usually sucks.
2.  Visual programming languages don’t support the critical functions of abstraction and decoupling.
3.  The languages don’t integrate with all of the other advantages available in a modern IDE, like source control and auto-completion, and syntax checking, etc.

So I asked myself whether these three concerns apply equally to what I’m trying to build.

The language that I am creating a visual version for is a derivative of Prolog. It is a declarative language, which sets out rules of inference, not procedures. Because of that, the “chunks” of code tend to be smaller, and the issue of visual complexity less severe.

But the “it gets bigger” effect of going to visual code is clearly visible, so far. The last set of blocks in the image above would be represented by the code

```prolog
\naf ?V:Voyage[week->week_4,destination->Jamaica].
```

But if the typical rule is going to be a few lines long, the expansion effect is probably not going to reach the point where the visual code is unintelligible. This complaint also ignores the major benefit of the visual programming language, which is that the one-dimensional nature of a line of text doesn’t give the user any clues about the relationships between the syntactical elements, whereas the two-dimensional nature of the visual version does.

That problem is typically solved in textual programming languages with the use of brackets and indentation, in order to artificially give the language a second dimension. And Python, one of the most popular and fastest-growing languages currently out there, has made this second dimension a part of the syntax itself.

There is a balance to be had, but I feel like this is not likely to be a roadblock.

As for abstraction and decoupling, I’m not sure I understand what Mike’s talking about, there. It may have to do more with earlier versions of Scratch or UML? My experience is that the current technology for visual programming languages allows you to visually create classes, functions, and whatever else you would like to create.

Even if that weren’t the case, again, the language I’m using is declarative, and so the need for abstraction and decoupling is relatively low because the rules are written at a higher level than the procedures would be.

Integration is a real concern. But complaining that it doesn’t have auto-complete doesn’t really make sense. You can’t auto-complete a block, because you don’t draw it, you just select it from a drawer. Syntax checking is an important point. But the Blockly tools allow for a large degree of type checking directly in the visual interface. So instead of typing something and then seeing a red underline, in Blockly when you try to make the same mistake, the second block just “doesn’t fit.”

As for source code control, that’s an interesting point. There are many features of IDEs that programmers have the benefit of that people using visual programming languages do not. But I feel like this misunderstands the point. The point of visual programming is not to provide the world’s current programmers with a new style of programming tool that they can use instead of their current toolset. The point of visual programming is to provide the people who find the current toolset impossible to learn with something more approachable, so they are not left with nothing.

If they get to the point where they want the information to be more compact on the screen, they want the benefits of an IDE, and they want to be able to abstract and decouple more efficiently, the tools for that already exist.

It’s an interesting perspective on the history of attempts and failures in the realm of visual programming. But it doesn’t give me any reason to think that what I’m doing can’t work.