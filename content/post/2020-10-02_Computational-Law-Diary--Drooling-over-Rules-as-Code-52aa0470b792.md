---
toc: true
title: 'Drooling over Rules as Code'
description: Video of Legislative Drafting Conference Presentation
date: '2020-10-02T23:05:07.857Z'
categories: []
keywords: []
slug: >-
  /@roundtablelaw/computational-law-diary-drooling-over-rules-as-code-52aa0470b792
---

### Video of Legislative Drafting Conference Presentation

I did an introduction to Rules as Code and demonstration of Blawx for the Canadian Institute for the Administration of Justice’s 2020 Legislative Drafters Conference. As promised, the video is now available on YouTube. If you hae 20 minutes, check it out at [https://youtu.be/bzBouePm5Js](https://youtu.be/bzBouePm5Js), or watch it here:

### Learning Drools

I’ve been spending a lot of time over the last couple of weeks trying to get my fingers dirty with [Drools](https://www.drools.org/), which is an open source BRMS tool for Java.

_Here’s how I imagine it was named: Someone wanted to use rules in Java, so they built something called Java Rules, abbreviated it to JRules, realized that would be pronounced “Drools” anyway, and went with the more intuitive spelling. I might be wrong, but I like that story, so I’m not even checking. If I’m wrong, don’t tell me._

Honestly, it’s a bit hard to figure out what the word “Drools” refers to. At the very least, there is a rule language called “Drools Rule Language”, or DRL. There is also an engine that does things with those rules, that is called the Drools Expert, and a graphical user interface that they seem to say is called Drools Workbench, but when you install it it’s called “Business Central.” And the abbreviation KIE (for Knowledge Is Everything) keeps showing up everywhere.

#### Programming Rules, Business Central Drools

What’s strange is that Drools Workbench/Business Central does a lot more different sorts of things than just DRL. It includes features for Business Process Modelling Notation, Decision Modeling Notation (which I was learning earlier this year), and others. In and of itself, that’s not strange. What’s more strange is that it does all of those things better than it does Drools Rule Language.

The interface for generating DRL code in a no-code way is just REALLY bad. It’s astonishingly bad. And the systems inside Drools Workbench/Business Central for generating and running test scenarios against your DRL rules is very limited in the kinds of scenarios you are able to easily generate. Tables are just not well suited as a way to set out arbitrary object-based data.

So the first thing that I learned about Drools, is if you want to use Drools, don’t use Drools Workbench. Instead, write Java.

#### VSCode FTW

I’ve been trying to limit the number of development environments I’m learning to use all at the same time, so decided to eschew the recommended Eclipse IDE (with its specific Drools extensions), in favour of writing Java and DRL code in VSCode. VSCode’s Java and Maven support is a bit finicky, but after installing a few extensions and following some online tutorials I managed to get things up and running.

#### Logic Programming vs. Production Rule Systems

One of the big differences between DRL and other languages I’ve used is that in Drools, the conclusions of a rule are imperative. That’s because Drools is not technical a logic programming language, it is a production rule system.

In a logic programming language, a “rule” is a condition and a conclusion, and when you write the rule what you are saying is “if the condition is proven, then the conclusion is also proven.” In a production rule system, a “rule” is a still condition and a conclusion, but you are saying “if the condition is proven, DO the steps listed in the conclusion.”

Drools manages to behave in a way similar to logical systems, because it has forward chaining and backward chaining ‘logic’. You have to be explicit in your conclusions about the kinds of changes that your rules make to the data in order for that to work, but it’s not an overwhelming amount of logical overhead.

But because conclusion are imperative, and not logical, there are still things that you cannot “say” in a conclusion in Drools that it is easy to say in other tools, like Flora-2. For example, you can say “if there is a pink elephant, there are no snuffalupagi” in Flora-2 like this:
```prolog
\neg ?_:Snuggalupagi :- ?_:Elephant[colour->pink].
```
You can’t say “there are no snuffalupagi” in Drools. You can not have any, but that’s not the same thing as concluding that their existence is impossible. You could delete any existing snuffalupagi, but that’s not the same thing, either.

#### Graph vs. Object Data

A major difference between different rule programming tools is how they represent data. Tools like Flora-2 ultimately save all the data in the form of predicates, but they provide a Frame Logic syntax to allow you to describe the data and query it in a way that seems very object-oriented. Tools like DMN or Neota Logic or Accord’s Concerto allow you to specify what are essentially data structures, but the structures do not include typed object references as a data type, making it more difficult to model objects that refer to other objects in complicated ways.

Drools, because it seems to have been created with the needs of Java programmers in mind, assumes that everything is a Java Object. That is more than sufficiently flexible to allow you to create whatever you want to in terms of object references. But because Java is a typed language, you can only use your data structure if you have defined it in advance. That makes it a little harder to write the code as you go, as if you want to add an attribute to an object you can’t just say so, you have to define the attribute and then fill it with data, and those two things happen in different parts of the code.

But that sort of cost comes with a lot of upsides. Because the data structures are defined, you can automatically generate interfaces for collecting the relevant data from users. So I’m a fan of typed data structures even if they require additional work.

You can either use `class` definitions inside your .java files, or you can use the `declare` method inside your DRL files, which gives you a much more efficient way of describing your data structure. I want to play more with the data structure declarations, but they seem to work relatively well, except that they require you to instantiate collection objects like lists before you can use them. That’s always seemed like so predictable a requirement, the reasoner should do it for you.

#### Defeasibility vs Priority

In some tools like Flora-2, you can specify that one rule defeats another rule when they generate conflicting results. In a production rule system, the conclusions are not logical statements, they are procedural instructions, so there is no meaningful way to say that they “conflict.”

For example, Blawx has a feature whereby if you create a boolean attribute, then setting that attribute to true conflicts with setting it to false. It can’t be both. If you try to make it both, you will get neither result, unless there is an “override” block specified, that says which rules overrides the other.

But the difference is maybe clearer when you use the negation operator. In declarative languages like Flora-2 you can say something like
```prolog
mortal(Socrates).  
\neg mortal(Socrates).
```
Both of these are logical fact statements, but they directly contradict one another. If you changed the order, it would make no difference. You still have a contradiction, and you still get no answer when you ask whether Socrates is mortal.

But in Drools, setting a variable to true is different from proving a statement true. So in the conclusion of one rule, you might say:
```Java
modify( socrates ) {  
  setMortal(true),  
  setMortal(false)  
}
```
The meaning of those two statements is “set the mortal property of the socrates object to true,” and then “set the mortal property of the socrates object to false”. Drools will happily do both, in order, and when you’re done, the answer is “false.” No problem.

So instead of defeasibility, which in a logical system determines which rule wins, Drools is concerned with priority, which is which rules get fired, and in which order. There are a number of features available to control the order in which rules fire like salience and agenda groups. I’m still learning them.

### Temporality and Events

Drools, unlike most of the tools that I’ve worked with (with the exception of Oracle Intelligent Advisor), also has built in features for dealing with events, which are facts that have a timestamp and/or a duration, and facts that are true for a period of time, and then are no longer true.

This is similar but not identical to the idea of a fluent, which is used in tools like LPS to set out something that was true during a period of time.

I’ve always been impressed with OIA’s ability to deal with fluents, and to have rules automatically switch from “when x is true, y is true” to “while x is true, y is true”. I don’t know yet how close to that you can get using Drools features, but I hope to find out.

### Quick Example

I’ve just started getting into actually modelling something DRL in order to understand how it works a little better, and learn the relative strengths and weaknesses. Here’s an early attempt at encoding clause 1(c) of Y-Combinator’s Simple Agreement for Future Equity:
```Java
// Section 1 (c)  
// (c)  Dissolution Event.  If there is a Dissolution Event before   
// the termination of this Safe, the Investor will automatically be   
// entitled (subject to the liquidation priority set forth in   
// Section 1(d) below) to receive a portion of Proceeds equal to the   
// Cash-Out Amount, due and payable to the Investor immediately   
// prior to the consummation of the Dissolution Event.

rule "Dissolution Event"  
  when  
     s : SAFE( terminated == false,   
               company != null,   
               c : company,   
               cash_out_amount != null,   
               co : cash_out_amount )  
     e : RWEvent( st : safe_type,  
                  st.name == "DISSOLUTION_EVENT" ) from c.events  
  then  
    SAFE_Result safe_result = new SAFE_Result();  
    safe_result.setCash_amount_due(Double.valueOf(co));  
    modify( s ) {  
      setSafe_result(safe_result),  
      setTerminated(true),  
      setTerminating_event(e),  
    }  
end
```
I have a lot more to learn about writing good DRL, but I’m starting to get the gist of it, and I’m interested to learn more. In particular, the temporality features mentioned above, but I would also like to understand better how unification works, and how it’s different from other tools I’ve used.

The Drools engine also offers an interactive mode, that allows you to repeatedly add data to the knowledge base, and it will update itself as the data is added. I would like to play with that more, as the non-interactive mode used by other tools is somewhat limiting. It also has features for something called complex event management, which seems like it would have application in the smart contracts space.

I’m going to keep working on a representation of SAFE in DRL, see what that experience teaches me, and I’ll let you know.

### Quick Notes

This list is getting long. It’s an exciting time to be doing Rules as Code stuff.

*   Oct 29: Stanford CodeX lunch webinar on [Blawx](https://www.blawx.com) (free to join, if you’re interested)
*   Nov 4: Cyberjustice Laboratory (University of Montreal) webinar on [Blawx](https://www.blawx.com)
*   Nov 17: European Commission Rules as Code [Blawx](https://www.blawx.com) demo in conjunction with the OECD’s “[Government After Shock](https://gov-after-shock.oecd-opsi.org/)” event.
*   Nov 17&18: Guest lecturing for Megan Ma’s course at Sciences Po.
*   Nov 24: Rules as Code session with Canada School of Public Service

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._