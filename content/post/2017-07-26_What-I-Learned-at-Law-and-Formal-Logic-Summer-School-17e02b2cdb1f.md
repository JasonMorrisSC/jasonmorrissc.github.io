---
toc: true
title: What I Learned at Law and Formal Logic Summer School
description: >-
  In the Piazza del Duomo in Florence, Italy, just next to a gelato store, and
  behind the caricature artists selling three-minute…
date: '2017-07-26T17:11:12.456Z'
categories: []
keywords: [logic]
slug: >-
  /@roundtablelaw/what-i-learned-at-law-and-formal-logic-summer-school-17e02b2cdb1f
---

![](/1__tqSgswGw61Y38VMalf__93A.jpeg)

In the Piazza del Duomo in Florence, Italy, just next to a gelato store, and behind the caricature artists selling three-minute masterpieces to tourists, there is a statue of the architect Brunelleschi. His eyes are lifted skyward and to the right, and on his face is an expression of thoughtfulness, hope, and perhaps a hint of concern.

He had every reason to be concerned.

If one follows the statue’s gaze, one sees that it is looking at the dome atop the Florence Cathedral, known as the Duomo. This dome is the architect’s masterpiece, and the defining feature of Florence’s skyline. But to understand the extent of what Brunelleschi accomplished, you have to understand the context.

Construction of the Florence Cathedral commenced in 1296. One hundred and twenty-four years later, it was finished, structurally, except for the dome. No masonry dome so large had ever been constructed, and none has been since. Nor had an octagonal dome ever been built without internal supports. Nor was there any known way to prevent the top of the dome from squeezing out the bottom without using flying buttresses, which were forbidden.

Brunelleschi was faced not only with the task of building the dome, but also the task of inventing the tools and techniques he would need to build the dome. So invent them he did. Sixteen years later, the dome was complete.

[Legalese.com](https://legalese.com) has a vision of something that can be built, but the tools to build it don’t exist yet.

The current crop of document-generation tools can get you pretty far for automating contract generation. But all of the _understanding_ of how the elements of a contract interact with one another in changing circumstances stay locked in the head of the person who drafts the template.

What if you had a contract that was written in a language that both computers and people could read, understand, compare to fact scenarios, and execute? What if you could prove mathematically that, if adhered to, the contract would prevent certain things from happening? What if when something happened triggering a requirement of the contract, the contract knew, and executed that requirement automatically? And what if the contract was no longer just words on a page, but was code capable of generating the words on a page that people expect to read, sign, and sue over?

Something along those lines is the long-term vision for Legalese, and it will require tools that don’t exist yet. The primary tool will be a domain-specific programming language Legalese calls L4.

Programming languages are built on logic. L4 needs to be built on the right logic for law.

![](/1__s0CyISdn9lQc0w6RPRTzRw.jpeg)

Law and Logic 2017 is a summer school offered at the Fiesole campus of the European University Institute, just outside of Florence. The campus is built on the site of a nearly millenium-old monastery, which a thousand years ago was the closest thing that existed to an institute of higher learning. The current buildings, however, date back “only” to the 1400s.

The main thrust of the course is an attempt to indoctrinate the students into a method of analysis of legal argument called the “logocratic method.” Alumni of the course are referred to as “logocrats,” and are encouraged to spread “logacracy” far and wide. There is a fair bit of comedy about it all, including an anthem set to the music of “Blowing in the Wind” by Bob Dylan.

But in a world that is grappling with “post-truth politics” there is also a sense of seriousness about whether and how we can critique arguments in a way that results in better decision-making.

Professor Scott Brewer of Harvard Law School is the acknowledged chief evangelist for logocracy. With the expectation that I am failing in the attempt, here’s my one-paragraph description of what logocratic method is.

> Arguments are myriad, but they have patterns that are limited in number. These include patterns of reasoning (e.g. inductive, deductive, analogical, inference to best explanation, etc.), and patterns of argumentation (e.g. undercutting, attacking the conclusion, attacking the premises, etc.). Understanding these patterns and qualities of arguments in a deep way enhances your ability to critique arguments. It makes you a better arguer, a more discerning audience for argument, in short a better thinker.

From the logocratic point of view, formal logic is to lawyering as materials engineering is to architecture. You must know the strengths and the weaknesses of the substances from which you build your edifice, whether an argument about the civil rights of an immigrant, or a dome atop a cathedral, lest either collapse under its own weight.

My objectives, of course, were less lofty. I am no Brunelleschi. To learn how bricks become a wall, I aimed to better understand the relationship between formal logic and the written law, whether in statute or in contract, to better understand what structures could and should underlie a tool like L4.

One major question in my mind is whether a tool for drafting contracts ought to implement deontic logic, the logic of duty. Deontic logic extends classical logic by adding in operators for Obligation, Permission, and Prohibition. Certain relationships exist between these ideas, and those relationships can be formalized into a logic that allows you to derive new true statements from known true premises.

The first thing that I learned is that in classic deontic logic, you require only one of these operators in order to express the rest. For instance, if O(A) means that to do A is Obligatory, you can express permission and prohibition by just adding the logical negation operator ‘¬’, which is read “not”. O(¬A) means it is obligatory not to do A, or A is forbidden. ¬O(A) means that it is not obligatory to do A. ¬O(¬A) means it is not obligatory not to do A. Combining these with a logical conjunction operator ‘˄’, read “and”, lets you say ¬O(A)˄¬O(¬A). That can be read “it is not obligatory to either do or not do A”, which is one definition of what is meant when we say that A is “permitted.”

However, there are problems with deontic logic. Some things that it becomes possible to express in deontic logic seem difficult to translate back into meaningful sentences that match our understanding of obligation in natural language. In order to see this more clearly, we need to add in another operator.

The material implication operator “→” can be read “implies,” but more typically you would translate “A→B” into something like “if A then B.”

For instance, one can say O(A→B), which states “it is obligatory that if A is done, B is done.” This sentence is hard to understand. It’s not clear how an implication can be obligatory. The statement O(A→B) is logically equivalent to O(A)→O(B), which translates to “if A is obligatory, then B is obligatory.” This seems more understandable. In logic, the meanings should be identical, but the two things do not say the same thing in our natural understanding.

The first suggests that the relationship between A and B is obligatory. It’s not clear what that would mean. The second suggests that one obligation follows necessarily from the other, which is a different thing to say. And then there is the third version, which more closely matches our understanding of how obligations work, A→O(B), if A is true, then B is obligatory.

Obligations seem inherently time-focused, somehow. Expressing them in a logic that doesn’t say when they arise, when they cease, and when they apply seems strange. Obligations, unlike the truth values involved in first-order logic, also don’t seem “monotonic.” Truth values in classical logic are presumed to be impervious to additional information. If it is true that “all men are mortal” that remains true no matter what else you might learn about the world. But it seems that an obligation can always be made false by additional premises being added to the theory.

I had a conversation with a fellow student on the last day of the course, and I put to him a thought experiment that I think shows the non-monoticity of obligations: Let’s say A is obligatory for you. Now, let’s say that a pandemic kills every human being on the planet except for you. What could A be, and _still be obligatory_ after the rest of the species is gone?

The answers, to the extent there are any, would seem to need to be absolute moral imperatives, not laws as we understand them today. No law holds sway in a world with only one human being. But in a world without human beings, 2+2=4 would remain true, and it would remain true that every atom was either hydrogen or not hydrogen. These sorts of truths don’t depend on the existence of people, and certainly not on the existence of more than one.

Deontic logic also has a number of paradoxes associated with it, like the gentle murderer paradox. Let’s imagine it is obligatory not to murder. _O_(¬_M_). Let us also imagine that if you murder someone it is obligatory that you do so without cruelty-that you are a gentle murderer. _M_→_O_(_GM_). And every gentle murder is a murder. _GM_→_M_. Now let us say that you murder someone, so M is true. Because M is true, _O_(_GM_) is true. So you are obliged to murder gently. It is a rule of deontic logic that if there is an implication _A_→_B_, and A is obligatory, then B is obligatory. So because _O_(_GM_) is true, and _GM_→_M_ is true, _O_(_M_) is true. So the murderer is obliged to murder. Because _O_(¬_M_) is also true, we know by the rule “whatever is forbidden is not obliged” that ¬_O_(_M_) is also true. If ¬_O_(_M_) and _O_(_M_) are true at the same time, you have a bare contradiction, and the logical system is now capable of proving anything, and useful for nothing.

The gentle murder requirement is called a contrary to duty obligation, and these are very difficult to model in deontic logic in a way that does not result in absurd implications.

These paradoxes arise, I believe, because of a number of things that classical deontic logic leaves out, like temporality, and the distinction between a type of thing and an example of that type of thing. Look at the two rules M→O(GM) and GM→M. The first says “if you murder, you are obliged to murder gently”. The second says “a gentle murder is a murder.” The M in the first rule refers to murders that have occurred, and a retroactive obligation that arises after that occurrance. In the second, M refers to the category of things that are murders.

From a philosophical standpoint, I don’t believe that absolute obligation is a good model for the law at all. I believe that the best model of the law is one in which expectations are set about what actions will and will not be done in the future, with the promise of consequences potentially applied to the person who has violated those expectations in the past.

I think that model would more reasonably represent contrary-to-duty obligations, because you would merely say that the consequences of, for example, murdering harshly are higher than the consequences for murdering gently. The only expectation would be not to murder.

I also think this model avoids falling into the trap of presuming that the obligations in the law have a specific moral character. I live in a part of the world where winter nights can, and regularly do, fall to temperatures that are deadly. Petty crimes go up in advance of these severe cold snaps in the weather, as homeless people take advantage of the criminal justice system for shelter from the elements.

These people fear for their lives, and commit crimes from which they anticipate no benefit and for which they do not seek to avoid punishment as a means of saving their lives. Can we say that their theft of a candy bar and then waiting in the store for the police to arrive was immoral because it was a breach of an obligation not to steal? No. It was illegal. But an act that harms others minimally and saves one’s own life from imminent serious harm is not immoral.

There are logics that deal with actions, and temporal logics. And more advanced forms called para-consistent logics deal better with contradictions, which happen all the time in the real world of law. But the further down that road you go, the more you begin to lose one of the main benefits of logic-based programming, which is the ability to computationally determine the outcomes.

From Legalese’s perspective, computability is the whole point. So representing legal concepts with more complicated logics may lose us more than it gains us.

That doesn’t mean that L4 ought not be able to express deontic ideas, and use deontic reasoning. It should still be possible in L4 to say that a thing is obligatory on a party, and to automatically detect when the same thing has also been made prohibited, and flag that as an error. But L4 does not need to implement formal deontic logic to be capable of that.

Brunelleschi, after all, did not know the math that could prove abstractly why his herringbone pattern bricks would hold their own weight. Those sorts of theories would not be discovered until long after his death. He had a practical problem to solve: How to get tonnes of building materials up in the air, and build eight curving walls that all hold one another up during and after construction without help from buttresses from outside or pillars from within.

Deontic logic is a fascinating field of intellectual inquiry, and an important thing for logocrats to understand. But for Legalese, first-order logic may be the herringbone brick.