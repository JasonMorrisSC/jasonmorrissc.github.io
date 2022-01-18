---
title: "Debugging Tax Forms with Lean: Chris Bailey at ProLaLa 2022"
date: 2022-01-17T13:24:31-07:00
draft: false
toc: true
keywords: ['lean','ProLaLa 2022']
---

## Chris Bailey's ProLaLa 2022 Presentation

As I mentioned in the [last blog post](/post/prolala), Chris Bailey's presentation at ProLaLa 2022 sort of requires
its own blog post in response.

I didn't take any screenshots of Chris' presentation, because he just shared his VS Code window. The sourcecode is [available online](https://github.com/ammkrn/prolala_demo). It might help to read
this first, though.

Chris is a law student at the University of Illinois, and spoke about creating a library of encoded legal components that would be useable for formal verification-like tasks or software development tasks, both. I think his presentation was mistitled. It should have been called "how I proved the
tax forms were wrong using a theorem prover.

### An Aside on Proof Technologies

His work is done in a fascinating tool under development at Microsoft Research called [Lean](https://leanprover.github.io/), which is a unique combination of a proof assistant and theorem prover.

It needs to be said at this point, that _I am out of my depth_, so take the rest of this technical stuff with enormous grains of salt.

First, let me say that in this entire post I'm using the word "proof" (or trying to) in the logical and mathematical sense, not in the legal sense.  If I say that something is proven, I'm not saying that we have sufficient evidence to decide that it's more likely than not, or that it's true beyond a reasonable doubt. When I refer to a "proof" here, I'm talking about a proof
with absolute certainty. A mathematical, incontrovertible proof. 

A proof assistant is a tool that might be used, for example, by a mathematician who was interested in proving a theorem about the behaviour of a certain type of mathematical concept. Non-computerized proof-writing is an extremely complicated sort of puzzle-solving, where you take what others have proven, the thing that you are trying to prove, and you try to find a path from where you are to where you want to be using a limited number of possible moves.

Proof assistants allow you to manually say "try this", and will calculate the individual steps for you, and show you what you got, making the task of searching for proofs easier. So a proof assistant is just a way to
save the human being some time while they are trying to discover proofs.

A theorem prover is a tool which takes an entire proof, and uses various approaches in order to verify that the proof is correct. So you can use such a system to model a complex system, and then you can add to that model statements like "this system never crashes", and the theorem prover
will try to find counter-examples that are consistent with the rest of the model (possible) but do not adhere to your assertion. Counterexamples are proof that your theorem is false. If an exhaustive search for counterexamples fails, your theorem is proven.

Again, lay explanations, by a non-expert. Probably wrong, but hopefully good enough to give you an idea.

I cannot overstate how uncommon these tools are. Most programmers will never touch either of them in an entire career. This stuff is almost never used. Anywhere, for anything. Formal verification is the exception to the rule, but formal verification is also very rare in the software world.

Some of the tools that I use have strong similarities to how proof assistants and theorem provers work, and so I have always been interested in their applicability to encoding legislation. Answer set programming works on a similar model of being able to find models that adhere to all the logical rules.But I've never actually used a proof assistant or a theorem prover.

Lean is a unique tool that is supposed to be a combination proof assistant and theorem prover. The current version, Lean 4, which Chris used, has been around for only aboput a year. This is a very new tool in a very obscure category of tools.

## How To Use Lean to Encode Laws

Chris' argument, if I understand it correctly, is that what we should first reformulate the law in code, however we like to reformulate it. Just write fast, good code. 

When I say that we should "reformulate", what I mean is don't worry about maintaining consistency with the structure of the law. If the law is a series of 5 rules with exceptions to exceptions, but you can reduce that to a simple algorithm that has two possible results, then go ahead. Just write whatever algorithm you think works.

But, instead of doing that in a language like JavaScript or Java, or Python, which is frequently done now, we should do that inside a language that is capable of being formally verified, like Lean.

Then, after we have written code that we think works, we should write a series of formal verification statements that represent things like "the code I wrote above for calculating taxes is compatible with section 3(4)(b) of the Tax Act," so Lean can check to see if they are true.

Now, we can run the code and ask Lean to prove the statements about our algorithm being consistent with the law. If they are proven, (and if we modeled our assertions consistently with our algorithm, and both those models are analaogous to the law) we can have confidence that the algorithm we are using to calculate the taxes is provably correct. So we no longer care what the algorithm actually is, because we have a proof that it is legally accurate.

That allows us later to run the code in the algorithm by itself, without the overhead of the proofs, and get more efficient code for calculating taxes than it would be possible to get in tools that are using a more direct translation from the terms of the legislation.

It's worth noting that for the purposes of "Rules as Code", all of this needs to be open source. Which Lean, thankfully, is.

## Problems with Modularity

Chris mentioned that he had some difficulty combining rules that applied to the same task but from multiple different levels of delegated legislation. I'm thinking that dividing the proof of consistency with rule parts would be easy, but what was complicated was trying to separate out the reformulated algorithms into two different parts. If you try to encode a regulation without referring to the law, in a reformulated way, it doesn't work. If you do both, there is no way to divide them into different files.

What Chris is discovering here is the inconsistency between reformulation and maintaining structural
isomorphism with the law. Reformulated algorithms are like children. You can create one by combining two
parents, but once you've done it it's more or less impossible to tell what came from which parent.

The bad thing about reformulation is that when the law changes, you need to change your algorithm to match. But because your algorithm is this mixed up bundle of everything, you don't really know what parts of the algorithm you need to fix. You need to "fix" the whole thing.

What this approach offers above the usual approach, though, is that you could just re-write your theorems, and that then turns into a set of tests that tell you whether or not your algorithm has been fixed. So the legislation is being used as a software specification that generates tests with the strength of proofs, but we create the software in much the same way we currently do.

Not a complete solution to the problem, but an improvement on the status quo.

### Not Amenable to Computers

Chris then talked about some boolean factors that were included in the code but not really being used by the code, except to generate caveats in the explanation given to the user.  If I'm following him correctly, he included them so as to be complete, but because there was no input that could be used to calculate whether or not they were true, they weren't actually used in the formulas, and instead were just treated as reminders about what must also be true when providing explanations to the user.

The way he put it is "that sort of reasoning is not very amenable to computers."

That's not actually true. It's not amenable to representation inside the sort of reformulated functional
code that he was writing, but computers generally are capable of it. In s(CASP), you would include these
sorts of variables in your code, and then when running a query mark them as "abducible." s(CASP) would then
create multiple models of how your query might hold. If the abducible queries don't appear in those models,
they are not important. If they appear in all of them, they are mandatory. If they appear in some of the
models, they are optional.

It's not an easy way of calculating it, because "necessity" is a property of the set of stable models that you get for the query, so there is some post-processing of query results involved. But it's computable. There's just not an easy way to do it in Lean. Perhaps "not yet".

## Debugging the Tax Forms

Chris described how he had encoded the statute, and some delegated rules, and proven that those encodings where consistent with the law.

He then described how he had also encoded a tax form that was relevant to the same rules, and had asked Lean to prove that the tax form and the law & regulation would give the same results in all cases. And Lean had given him a counter-example, showing that the theorem was false.

That is to say, that assuming his encoding of the law is faithful, and his encoding of the form is faithful, he generated a proof with mathematical certainty (not an argument, not an opinion, but a
categorical certainty) that the tax form has an error.

That's very impressive. For context, here's what the code looks like that he uses to show the discrepancy:

(I know I made a big deal about syntax highlighting when I migrated this month, but Hugo doesn't have syntax
highlighting for Lean built in, and I'm not sure how to add it. If you know, hit me up.)

```lean
/--
Uses a counter-example to show that in some cases, the basis in incoming property 
calculated by form 8824 is different from the basis calculated by the rules dictated in
the CFR. Specifically, this is the case when there is "other" property involved in 
the transaction (like outgoing stock).
-/
theorem basis_in_lkp_ne : ¬∀ (form : Form8824.Part3), form.basisIncomingLkp = form.toBaseLke.basisIncomingLkp :=
  let counterexample := fromBase CfrExamples.«CFR 1.1031(d)-1 example 2» 
  not_forall_of_exists_not <| Exists.intro counterexample (fun h => absurd h (by simp))

/-- 
This difference between the worksheet and the USC/CFR holds for larger hypothetical values of 
e.g. 1250 recapture, and even if there are transaction costs. 
-/
theorem basis_in_lkp_ne' : ∃ (form : Form8824.Part3), ¬form.basisIncomingLkp = form.toBaseLke.basisIncomingLkp :=
  let counterexample := { 
    CfrExamples.«CFR 1.1031(d)-1 example 2» 
    with 
    transactionCostsIncurred := 800
    recapture1245 := 0
    recapture1250 := 10000
  }
  Exists.intro counterexample (fun h => absurd h (by simp))
```

## Dealing with Underspecification

Chris talked about an approach to underspecification in legislation, where the legislation says what is
to be done but doesn't say how. This was possible to represent in Lean by simply assuming the presence of a function that had the desireable properties, and then proving that in the presence of any such
function the other requirements of the law would hold.

That's an extremely useful approach, because it allows you to encode what the law actually says, and prove things about what the law actually says, without being forced to add or invent things that the law
did not actually say.

Chris called it "vagueness" in his talk, but from the description he gives I would be inclined to call
it an underspecification, rather than a vagueness. Vagueness is typically used to describe terms like
"undue hardship", which are subjective, and ultimately determined by a trier of fact. Failing to say
how exactly to split up the assets of a couple on divorce for tax purposes is not "vague," it's just missing.

## Modelling Civil Procedure

He then goes on to show how he modeled some aspects of civil procedure, using something like a state and
transition model. He spoke briefly about the benefits of being able to demonstrate that these models were decidable, and that still goes completely over my head, so I want to learn more about that.

In an ideal situation, it would be possible to prove the existence of a sequence of steps that would go
from an initial condition (that describes the status quo) to a future condition (that describes your objective), by finding a counter-example to the theorem that no such path exists.  That would effectively be a solution to a planning problem, and you would have a way of giving people procedural advice, automatically based on their desired outcomes.

But time was short, so we didn't get to go into it further.

## My Take-Aways

There is a sort of a tension in the way that you model legislation, between doing it in a way that is structurally consistent with the legislation itself, and doing it in a way that is fast. The more of the one you have, the less of the other. Structurally consistent makes it easier to maintain, and generate automatic explanations. Speed makes the code usable in more real-world applications.

So the question in my mind is now this: If I can use Lean to write fast code that generates correct
answers (which it seems like Lean lets me do), and I can use Lean to write code that checks to see if my fast code is
legally correct (which Chris demonstrates it can be used to do), can I also use that verification code in Lean
to generate an after-the-fact proof for specific conclusions generated by the fast code, and translate that proof
into a natural language justification that match the structure of the original law?

I can't think of a reason you wouldn't be able to. The problem of proving a grounded theory (one with all the relevant facts) is much less complicated than the problem of proving things in the abstract. So the question is really how difficult would it be to get Lean to do the natural language justifications based on those rules.

All in all, this approach - using efficient but verifiable code for calculation, and less efficient code for verification and justification - may be the ideal temperate zone in this tension between speed and isomorphism.

There are other approaches, but this seems like one worth exploring.

## But How Easy Can It Be Made?

Then, as with all things I play with, the question becomes "how easy can this possibly be made for non-technical users?"

Here, there are indications in both directions. the challenge with Lean is going to be very high. It is a cutting edge tool written by depednent type theory computing science experts for use by math experts. It is also not a product, but an experiment, and can change any time the developers feel like going in a different direction. It has taken me 5 years of effort to get to the point where I can even _understand_ a presentation like this, the _second_ time I watch it.

Despite the fact we deal with logic all the time, there are very few law schools in the world where formal logic is a mandatory course for lawyers. A natural deduction proof would be completely foreign to most legal practitioners. So there are language differences between the culture of Lean and the culture of the law.

On the other hand, the Lean developers are not themselves mathematicians. They are just writing something that is extremely useful for mathematicians. And I understand that there are long and boring debates about the syntax and semantics that should be used in programming languages for these sorts of purposes. As a result, the Lean developers have added features to the language that allow you not only to write libraries of components that can be re-used later (which was the capability Chris named his ProLaLa talk after), but that allow you to change the syntax and semantics of the language itself.

So Lean may be exceedingly complicated, and sophisticated, but it is also specifically designed to be flexible in its user interface, so that it can be made simpler.

That suggests that people who are interested in writing domain specific language for law, and who would like those languages to have the features of formal verification, that Lean may a platform on which to try building those langauges.

Maybe. :)

## A Big Thanks to Chris Bailey

I am clearly a newbie when it comes to this style of programming, but it is something that I have heard people smarter than me talking about for about 5 years.

Chris' work is the first time I have seen it demonstrated in the real world. Not only did he encode the rules, and the form, but he proved the form was inconsistent with the laws. People working in tax departments take note.

This has made the whole conversation about formal verification much more concrete for me, and I really appreciate that.