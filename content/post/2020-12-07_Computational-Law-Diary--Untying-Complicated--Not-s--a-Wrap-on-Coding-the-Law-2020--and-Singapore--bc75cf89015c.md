---
toc: true
title: >-
  Untying Complicated “Not”s, a Wrap on Coding the Law
  2020, and Singapore…
description: >-
  I’m working on some materials to help bring our junior researchers up to speed
  on Flora-2, and I noticed something interesting when trying…
date: '2020-12-07T19:06:33.382Z'
categories: []
keywords: [flora2]
slug: >-
  /@roundtablelaw/computational-law-diary-untying-complicated-not-s-a-wrap-on-coding-the-law-2020-and-singapore-bc75cf89015c
---

![](/1____XHYbehONmLXCVDlBXlF0g.png)

I’m working on some materials to help bring our junior researchers up to speed on Flora-2, and I noticed something interesting when trying to explain how Flora-2 does negation differently than other logic programming languages.

### Three Nots

In Flora-2 there are three different ways of saying “not”, and they all mean different things.

#### Classical Negation

Classical negation comes from formal logic, and is expressed using the operator `\neg`. If you say `\neg raining`, what you are saying is that the truth-value of the statement `raining` is _known to be false_. That is to say, you don’t need to look out the window and check. You are certain it is not raining.

#### Prolog Negation

Prolog-style negation is expressed using the operator `\+`, the same as in Prolog. This is a form of “negation as failure”, which it has always seemed to me would make more sense if it was described the other way around, “failure as negation.” What `\+ raining` means is that it is the system failed to prove that it is raining. That is to say, if you are in a house with no windows and doors, so you can’t check, `\+ raining` would be true.

#### Well-Founded Negation as Failure

Most of the time in Flora-2 you will be using Flora-2’s form of negation as failure, which adheres to the well-founded semantics. You use this form of negation by using the operator `\naf`. What `\naf raining` means is _either_ we know it is not raining (`\neg raining`), _or_ we cannot prove that it is not raining (`\+ raining`).

If you need to know the difference, you can ask which one it is, and Flora-2 will tell you. But if you’re just interested in known that one of them is true, `\naf` has got you covered.

### Well-Founded What Now?

I know, I know. It’s all very jargony. But it actually turns out that well-founded negation as failure is a very familiar concept to lawyers.

Consider this phrase:

> “The accused was found not guilty.”

If you understand how findings of guilt work in criminal trials, then the “not” in the above phrase is semantically identical to `\naf` in Flora-2.

“Not guilty” means _either_ the person was known to be innocent (though that is rare outside episodes of Matlock), _or_ that there was no proof they were guilty.

Either is enough. But if all you know is that they were “not guilty”, you don’t necessarily know which one it is.

For the purpose of encoding legal knowledge, it seems helpful to have a version of “not” that behaves the way lawyers might expect it to.

### Coding the Law 2020 Is Wrapped

My “Coding the Law” class at University of Alberta Faculty of Law wrapped up last Friday, with pitch presentations by three student groups. I’m so impressed by and proud of the work that the students have done. The group that will be representing the University of Alberta at Georgetown Law’s Iron Tech Lawyer Invitations in the spring is group 3. They built a tool to help people easily file effective complaints about the Calgary Police Service, in an effort to do something concrete about systemic racism in policing in Alberta.

Not only did they pick an excellent access to justice problem to try to do something about, they were impressively thoughtful about making it a friendly tool that anyone could use, and that would be an end-to-end solution for filing complaints.

All three groups are working with clients who fully intend to continue to develop and deploy the solutions they created. Real-world deployment is a challenge for a lot of law school courses like these. But not for these students.

I take that as a sign that [Docassemble](https://docassemble.org) is the right tool to be teaching these sorts of courses in. Docassemble is free, open source software, that a non-profit access-to-justice organization can realistically deploy with knowledgeable help, and around $250CAD a year in computing resources.

Here’s hoping we can keep the momentum going, and support these organizations as they continue to deploy automated legal services.

### Singapore Bound in 2021

I’m happy to announce that I have received preliminary permission from the government of Singapore to join my colleagues at Singapore Management University Centre for Computational Law in early January. There is no firm end-date, but I will be _in situ_ at SMU until at least summer of 2021. So look forward to these blog posts including more information on what and where I’m eating, and some photography!

Covid-wise, Singapore is doing much better than Alberta in containing the virus, and I will need to test negative before I leave, isolate for two weeks on arrival, and test negative during the isolation before I will be interacting with anyone in Singapore. So I’m feeling quite secure about travelling.

Friends [following me on Twitter](https://twitter.com/RoundTableLaw) have already started putting me in touch with locals and Canadian ex pats with offers to show me the ropes. If you have any contacts or suggestions, (particularly food) I’d love to hear them!

### Quick Notes

*   Dec 8: I will be a guest judge at Megan Ma’s Artificial Intelligence and Law course at Sciences Po. Megan was kind enough to join my class as a judge, and I’m looking forward to returning the favour.
*   Happy holidays!
*   Jan 12–14: Presenting on Blawx at the Legal Services Corporation Innovations in Technology (Virtual) Conference.
*   TBD (Jan ’21): Cyberjustice Laboratory (University of Montreal) webinar on [Blawx](https://www.blawx.com/).