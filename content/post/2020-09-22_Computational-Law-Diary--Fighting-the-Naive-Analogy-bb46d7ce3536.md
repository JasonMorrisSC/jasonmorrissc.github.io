---
toc: true
title: 'Fighting the Naive Analogy'
description: Electronic Files and the Naive Analogy
date: '2020-09-22T18:06:50.652Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/computational-law-diary-fighting-the-naive-analogy-bb46d7ce3536
---

### Electronic Files and the Naive Analogy

When I started running a virtual law firm, I went to a lot of trouble to make sure that all of the pieces of information about one of my clients’ matters were all in the same place. Emails were converted to PDF, and stored in some single source of truth location, like an electronic version of a case file.

That was a giant waste of time. Now, emails live where they live, documents live somewhere else, invoices in a third place, and they are all linked by a reliable matter naming tool. If I need to find anything, I know where it is. If I need to find everything for one matter, I know what the buckets are, and just search for the same matter number three times.

What I was doing was trying to make the way the computer dealt with information more analogous to the way it was dealt with when it was on paper. But “having it all in one place” was the method, not the point. The point was “being able to find it when you need it.” And when documents are electronic, there are ways of achieving the point without copying the method.

Copying the pre-digital method is what I call the “naive analogy”. Just do it the same, but with electronic bits. It’s a mistake I make a lot. And I found myself making it again today.

### Rules as Code and the Naive Analogy

There is a suggestion from some people in the Rules as Code space that only new rules should be encoded, because the process of encoding requires clear rules, and rules generated without that process are insufficiently clear to be reliable. I was reminded of this argument last week after reading [a paper written by the authors of DataLex at AustLII](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3638771).

(If you don’t know what DataLex is, you [need to read this post](https://medium.com/@jason_90344/rules-as-code-can-and-should-be-done-without-programmers-fb3e0f4dafa5?source=friends_link&sk=d153db15befe09ee197f6c83b3e93db6).)

I have this impression that rules as code can be very valuable to legislative drafters, and I know that if legislative drafters adopted it as a technique for their own reasons, we would get a large number of follow-on benefits from that. So I’m interested in how and why legislative drafters might use rules as code.

I recently learned that the majority of a legislative drafter’s life deals with drafting amendments to existing legislation. If that’s true, it creates a problem for the “new laws only” argument. It’s difficult to get legislative drafters excited about a tool that is only useful to them when they are drafting legislation from scratch, because it will help them with only a small part of their job.

How could we make rules as code useful to a legislative drafter who was working on an amendment to an existing piece of legislation, for which legislative drafters had never done an encoding? We could give them the tools required to build their own encoding of the existing law, but as I said people argue that this is a bad use of time because encoding laws not designed to be encoded is difficult and unreliable.

### Encoded Laws Flow Uphill

But wait… Isn’t it happening anyway? Aren’t people implementing legislation in code all the time after the fact? In both the public and the private sphere? Intuit encodes the tax acts. Government department encode their own binding legislation. Even point-of-sale terminals enact tax legislation.

What if when the legislative drafter sat down to write an amendment to a law that had never been encoded _by drafters_, they had access to encodings that had already been done by other people, who were trying to implement the law? That could give drafters a head start if they want to encode existing legislation in order to test amendments to it.

Which would mean that the trick is to come up with a language that achieves both objectives: a language that makes it easier for people to implement laws, AND a language that makes it easier to draft and amend them, based on the same encoding. And, a convenient way of sharing encodings, and relating them to authoritative sources. An open source repository of encoded laws, whether they were encoded by drafters, or by people attempting to automate them after the fact.

Then you have a world in which both the producers and consumers of rules are motivated to convert them to code, piece by piece, as needed, and each can build off the work of the other.

The idea of open source version of laws that could be shared had occurred to be before. And the idea of having legislative drafters encode existing laws had occurred to be before. But it had never occurred to me that you could make the output of implementation an input for drafting. Because _in the naive analogy_, laws don’t flow from citizens to drafters.

But that’s the naive analogy. Encoded laws are not statutory instruments, and they can flow uphill.

### Quick Notes

*   I will be doing a Blawx demo for a Rules as Code webinar being put on by the European Commission, in conjunction with the OECD’s “[Government After Shock](https://gov-after-shock.oecd-opsi.org/)” event on November 17. More details to come.
*   I will also be guest lecturing for Megan Ma’s class at Sciences Po on the 17th and 18th of November.
*   My presentation for CIAJ’s Legislative Drafting Conference 2020 last week will be up on YouTube soon. Stay tuned.

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._