---
toc: true
title: s(CASP) as a Rules as Code Tool
description: >-
  OK, I think I’m going to call it. Flora-2 is no longer my favourite tool for
  doing Rules as Code. My new favourite is s(CASP).
date: '2021-02-16T13:52:33.378Z'
categories: []
keywords: [scasp]
slug: /@roundtablelaw/s-casp-as-a-rules-as-code-tool-97ec3435975c
---

OK, I think I’m going to call it. Flora-2 is no longer my favourite tool for doing Rules as Code. My new favourite is s(CASP).

I’m working on a paper for the International Conference on Artificial Intelligence and Law at work, and I’m doing an experiment, the results of which are blowing me away.

I took a piece of legislation recommended by a potential industry partner, and encoded it in s(CASP). I did it as faithfully as I could, which means I did not try to encode what I thought it _meant_, I tried to encode what it _said_. I also wrote 26 tests, each of which creates a fact scenario and then asks a question that should generate at least one answer. Again, the point here was not to write tests that I knew would generate answers or not generate answers. The point was to faithfully describe fact scenarios, and what I believed the rules would _mean_ in those fact scenarios. That is to say, what legal conclusions could be reached.

At first, zero of the tests actually worked. Which was sort of expected, but as I did the debugging, I found that there were problems in the code, but fixing those problems did not make the tests pass. There were a number of problems in the law, and a disconnect between how I expected it to work and how it actually worked.

Investigating further, I found one syntactic ambiguity in the rule that I had encoded using the wrong possible interpretation. No biggie. But then I found two semantic errors in the legislation that could not be resolved by just choosing an equally plausible interpretation. There was no ambiguity at all. The drafters had used defined terms, consistently. And the result of what the drafters had written was that the encoded version of the legislation did _nearly none_ of the things I anticipated it would.

I tracked the problem down to two sections, and came up with proposed amendments for those two sections of the rule. I encoded those amendments, and re-ran the tests, with either of them, about half the tests passed, but with both of them all the tests passed.

### What I’ve Learned

Let me give you the short version.

#### Answer Set Programming Kicks Ass for Rules as Code

Until now I’ve been encoding laws in declarative logic languages. But answer set programming does not tell you just what the right answer is, but all of the ways that you could potentially reach that right answer, and everything else that you know about the world while that answer is true, and s(CASP) adds on an English-language explanation for each method of reaching the right answer.

That’s just a lot more information, and a lot of critically useful information if what you are trying to do is figure out whether or not you faithfully encoded the rule.

Most of the time, when you have a query that doesn’t behave as expected, you can throw a “not” in front of it, run it again, and get a natural language explanation for why the original query failed. I have never used a tool with that capability before, and it is blowing my mind.

Unreal powerful. Just a completely different coding experience.

#### Answer Set Programming Is Not Good For Implementation

Unfortunately, answer set programming gets exponentially slower and slower the more complicated the ruleset is, and with relatively small rulesets you quickly run into a situation where it is going to take too long to generate any useful answers to your questions. That means ASP is good for debugging small units of Rules, but not good for implementing a system that needs to quickly answer questions. It’s good for doing quality assurance on your code, but shouldn’t actually _be_ your code when it comes to building a web tool.

#### Doing Comparative Rules as Code Is Hard With Amendments That Change Section Numbers

If you are trying to figure out how a change to a ruleset changes the results, you need to have a set of tests that you can run on both rulesets. That gets really hard if the amendments change the section numbers, because you want to be able to ask questions about what conclusions each section came to, and how those conclusions were combined. Section 1 said “yes,” section 2 said “no”, and section 2 overrides section 1, so the answer is “no.”

See how often we said “section”? Now imagine that you propose an amendment that changes the section numbers. You are going to have to re-write your tests in order to find out whether or not it works, because the old section references are no longer correct.

There might be some way to avoid this problem if you are willing to look long and hard enough, but it’s not immediately obvious to me what that would be.

#### Legislative Errors are Like Black Matter

It’s everywhere, it drags everything down, but you absolutely cannot see it. You can only detect it when you do the math, and the math doesn’t work out. And in law, nobody is doing the math.