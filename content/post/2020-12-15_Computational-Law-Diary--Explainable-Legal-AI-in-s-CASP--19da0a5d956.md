---
toc: true
title: 'Explainable Legal AI in s(CASP)'
description: 'So yesterday, I learned about something called s(CASP) that I needed to try.'
date: '2020-12-15T23:42:31.279Z'
categories: []
keywords: [scasp]
slug: >-
  /@roundtablelaw/computational-law-diary-explainable-legal-ai-in-s-casp-19da0a5d956
---

So yesterday, I learned about [something called s(CASP)](http://www.cliplab.org/papers/arias19-event-calculus-asp-lopstr19.pdf) that I needed to try.

Today I tried it. And it’s awesome. Here’s how it went.

### Background

I actually came across s(CASP) by accident while looking for something else. The name, I take it, means stable model constraint answer set programming. Which is the sort of name only a computer scientist could love. It was created by a team at the Joaquin Arias, Zhuo Chen, Manuel Carro, and Gopal Gupta at the IMDEA Software Institute at Universidad Politecnica de Madrid.

### Installing Ciao

The instructions were [here](http://ciao-lang.org/install.html). I have WSL2 running on my Windows 10 machine, so I followed the instructions for downloading and installing the Ciao language, first. It did not work. I ran portions of the manual installation process, then restarted my terminal, and eventually it seemed to have done something. Maybe just restarting the terminal would have been enough.

### Installing s(CASP)

The instructions were [here](https://gitlab.software.imdea.org/ciao-lang/sCASP). They worked, first try. I created a file called `test.pl` that read as follows:
```prolog
p(A) :- not q(A).  
q(A) :- not p(A).  
?- p(A).
```
And then I ran `scasp test.pl` and got the following output:
```
QUERY:?- p(A).

ANSWER: 1 (in 1.395 ms)

MODEL:  
{ p(A),  not q(A) }

BINDINGS: ?
```
Which is what it was supposed to do. Great.

### Encoding Covid-19 Rules

Now that I’ve got a working system, I want to encode some covid-19 rules in order to show what it can do. The rules in Alberta have recently changed with regard to work from home, so I’ll do a small encoding of a subset of one rule in particular:

> Working from home is mandatory unless the employer requires a physical presence for operational effectiveness.

I encoded that using a small Ciao file that looks like this:
```prolog
#pred worker(X) :: '@(X) is a worker'.  
worker(jason). % jason is a worker.

#pred presence_required(X) :: '@(X)\'s presence is required at work'.

#pred wfh_mandatory(X) :: '@(X) must work from home'.

% working from home is mandatory unless the employer requires a physical presence for operational effectiveness.  
wfh_mandatory(X) :-  
    worker(X),  
    not presence_required(X).

?- wfh_mandatory(jason).
```
Note that the lines beginning with `#pred` provide information about how each predicate can be expressed in English.

### We Can Answer Questions!

The file ends with a question of whether or not Jason is required to work from home. You can now ask it that question by running the file like this:
```
jason@Arthur:~/scasp$ scasp wfh.pl  
QUERY:?- wfh_mandatory(jason).

ANSWER: 1 (in 0.407 ms)

MODEL:  
{ wfh_mandatory(jason),  worker(jason) }

BINDINGS: ? ;
```
So it finds one model in which it is mandatory for Jason to work from home. Which means “yes.”

### We Can Explain Our Deductions!!

We can also ask it to explain how it found the model by adding `--tree` to the command, which adds the following to the output:
```
jason@Arthur:~/scasp$ scasp --tree wfh.pl

...

JUSTIFICATION_TREE:  
wfh_mandatory(jason) :-  
    worker(jason),  
    not presence_required(jason).  
global_constraint.
```
You can see that the justification tree shows that Jason is a worker, and his presence was not required, therefore, working from home was mandatory.

### We Can Explain Our Deductions in English!!!

But you can also ask it to explain that in English by adding `--human` to the command, which changes the answer and the justification tree to look like this:
```
jason@Arthur:~/scasp$ scasp --tree --human wfh.pl  
QUERY:I would like to know if  
     jason must work from home.

ANSWER: 1 (in 0.126 ms)

JUSTIFICATION_TREE:  
jason must work from home, because  
    jason is a worker, and  
    there is no evidence that jason's presence is required at work.  
The global constraints hold.
```
### We Can Turn the Explanations Into Web Pages!!!!

But wait, there’s more. We can generate a web page with the answer to the question just by adding `--html` to the command above. What does that look like? Like this:

![](/1__dK8wMKlLOcdllTYyxdXl0A.png)

### So what?

My wish-list for the perfect declarative logic tool for encoding laws and contracts looks something like this:

*   Open Source
*   Explainable
*   Defeasible
*   Deals well with uncertainty
*   Understands the passage of time
*   Easy to Use
*   Case-based reasoning

Finding more than one of these things in the same tool is rare, particularly when open source is a requirement (as it is in so many Rules as Code applications).

Explanations and defeasibility (the ability to encode rules as exceptions to other rules) are the really hard technical parts. The only other hard part is ease of use, which is a design problem.

I spend a lot of time playing with Flora-2, because it is both open source and has defeasibility. But until today I didn’t have anything to play with that was both open source and explainable.

Today that changed. And s(CASP) has already been used to implement a simple version of event calculus, which suggests that it will not be complicated to add the other necessary features to it.

I had been spending some time writing code that was designed to add explanations to Flora-2. Now, I need to think about whether my time would be better spent adding defeasibility to s(CASP).

But we really need to come up with a better name for it.