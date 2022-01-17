---
title: "Debugging_tax_forms_with_formal_verification"
date: 2022-01-17T13:24:31-07:00
draft: true
---



Again, I was pulled away from the computer for a portion of Chris Bailey's talk, but it was so interesting I went back and watched parts of it again, and I expanded this section of the recap.

Chris is a law student at the University of Illinois, and wrote about creating a library of encoded legal components that would be useable for formal verification-like tasks or software development tasks, both.

### An Aside on Formal Verification Technologies

His work is done in a fascinating tool under development at Microsoft Research called "Lean", which is a unique combination of a proof assistant and theorem prover.

It needs to be said at this point, that _I am out of my depth_, so take the rest of this technical stuff with skepticism. A proof assistant is a tool that might be used, for example, by a mathematician who was interested in proving a theorem about the behaviour of a certain type of mathematical element. Non-computerized proof-writing is an extremely complicated sort of puzzle-solving, where you take what others have proven, the thing that you are trying to prove, and you try to find a path from where you are to where you want to be using a limited number of possible moves.

Proof assistants allow you to manually say "try this", and will calculate the individual steps for you, and show you what you got, making the task of searching for proofs easier. So a proof assistant is just a way to
save the human being some time while they are trying to discover proofs.

A theorem prover is a tool which takes an entire proof, and uses various approaches in order to verify that the proof is correct. So you can use such a system to model a complex system, and then you can add to that model statements like "this system never crashes", and the theorem prover
will try to find counter-examples that are consistent with the rest of the model (possible) but do not adhere to your assertion. Counterexamples are proof that your theorem is false. If an exhaustive search for counterexamples fails, your theorem is proven.

This stuff is almost never used. Anywhere, for anything. I cannot overstate how unusual these tools are. Most programmers will never touch either of them in an entire career.

I have never used either of them.

Lean is a tool that is designed to be a combination of both, and Chris, who is a law student, is using it to encode laws.

## Ok, Back to Chris

No screenshots, here, as Chris was going hard-core and sharing his coding environment. :) But the sourcecode is [available online](https://github.com/ammkrn/prolala_demo).

So Chris' argument, if I understand it correctly, is that what we should do is we should first reformulate the law in code, however we like to reformulate it. Just write fast, good code. But, we should do that inside a language that is capable of being formally verified, like Lean.

Then, additionally, we should write a series of statements that represent things like "the code I wrote above for calculating taxes is compatible with section 3(4)(b) of the Tax Act," and then write code to tell Lean how to check to see if that is true.

Now, we can run the code and ask Lean to prove the theorems. If they are proven, we can have confidence that the algorithm we are using to calculate the taxes is provably correct, if our definitions of the proofs are correct. So we no longer care what the algorithm actually is, because we have a mathematical proof that it is legally accurate.

That allows us later to run the code in the algorithm by itself, without the overhead of the proofs, and get more efficient code for calculating taxes than it would be possible to get in tools that are using a more direct translation from the terms of the legislation.

He makes some comments about being able to avoid logical complexities,
which is also true.

So at this point in the conversation, I'm thinking to myself "that's great, unless it means that you can only explain the result by virtue of the path the algorithm took to find it, which might be wildly different from what the law says."

He mentioned that he had some difficulty combining rules that applied to the same task but from multiple different levels of delegated legislation. It's not immediately clear to me why that would be. I'm thinking that dividing the proof of consistency with rule parts would be easy, and that what was complicated was trying to separate out the algorithms into two different parts. Because you can't calculate tax according to only a regulation without making reference to the rules of the law.

Interesting, that's actually a counter-argument for his argument that the artifact that you generate this way is better. The artifact that you generate this way is necessarily harder to modify if additional rules are added, because you need to modify how the law is encoded every time a regulation is added.

### Not Amenable to Computers

Chris talked about some boolean factors that were included in the code but not really being used by the code, except to generate caveats in the explanation given to the user.  If I'm following him correctly, he included them so as to be complete, but because there was no input that could be used to calculate whether or not they were true, they weren't actually used in the formulas, and instead were just treated as reminders about what must also be true when providing explanations to the user.

The way he put it is "that sort of reasoning is not very amenable to computers", in the talk, and he's close, but not quite. If you use s(CASP), for example, you can put those variables right into the code, and when
you are running your queries specify them as abducible. The reasoner will
find all models in which your query holds, and if all of them include
the abducible variable, then it is a necessary element of that query holding.

It's not an easy way of calculating it, because necessity is a property of the set of stable models that you get for the query, so there is some post-processing of query results involved. But you can do it.

## Back to Chris

The sorts of things that you can do with a tool like Lean are different form the things you can do with functional, logical, or imperative tools. It gives you access to hard-core formal verification, where you can mathematically guarantee that certain properties are true of your model of the legislation, and if your model of the legislation is good, those properties may also hold true of your legislation, too.

Imagine being able to say that you have a mathematical proof that your proposed amendment to the tax laws doesn't increase anyone's taxes.

I need to look much more closely at it, but what I saw was very impressive. It seemed to have a very strong structural isomorphism to the law, and Chris Bailey did not seem completely discouraged from the attempt, both of which are good signs.
