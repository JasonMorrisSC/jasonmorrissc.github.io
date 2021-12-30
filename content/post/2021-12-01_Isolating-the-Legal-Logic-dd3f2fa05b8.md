---
toc: true
title: Isolating the Legal Logic
description: >-
  Just had a great conversation with a colleague that helped me understand a
  challenges involved in implementing Rules as Code that hadn’t…
date: '2021-12-01T18:56:07.801Z'
categories: []
keywords: []
slug: /@roundtablelaw/isolating-the-legal-logic-dd3f2fa05b8
---

Just had a great conversation with a colleague that helped me understand a challenges involved in implementing Rules as Code that hadn’t occured to me before.

The objective in Rules as Code is to write an encoding of the legislation that is as re-usable as possible. That means avoiding the temptation to put things into the encoding of the rules that only make sense to include in the context of a particular application.

What I didn’t realize, until I chatted with my colleague today, is that only gets you reusability, not maintainability. In order to get the benefit of maintainability, you _also_ need to avoid the temptation of implementing legal logic in the application.

The example today was something to the effect of “I already know what the answer to that query is going to be, so I’ll just skip it to run faster.”

That might be justifiable in certain scenarios where performance is critical, and the legislation is very unlikely to change in any meaningful way. But generally speaking, it’s a “Rules as Code Smell.”

You shouldn’t have to know anything about the law in order to write an app. If you do know something about the law, you might be able to make your app faster using that knowledge, but you probably shouldn’t. Because you also shouldn’t have to know anything about the law in order to maintain an app that was written 30 years ago. The likelihood that the next developer to maintain the app knows that you encoded your legal knowledge, has the same knowledge, and/or understands whether your knowledge is still true now that the law has changed, has got to be approaching zero.

If you want re-usability, don’t put application stuff into the encoding of the law. If you want maintainability, don’t put law stuff into the encoding of the application. Both sides need to play nice to get all the benefits.