---
toc: true
title: 'Blawx Runner Up at ATLA, and Visual Interfaces Rock'
description: Blawx Named Runner Up at ATLA
date: '2020-09-15T19:30:26.862Z'
categories: []
keywords: [blawx]
slug: >-
  /@roundtablelaw/computational-law-diary-blawx-runner-up-at-atla-and-visual-interfaces-rock-904809dfbd26
---

### Blawx Named Runner Up at ATLA

Much to my surprise, [Blawx](https://www.blawx.com), the open source web based tool for Rules as Code that I have been working on for almost two years, was named a finalist for the Startup Category of the inaugural [American Legal Technology Awards](https://americanlegaltechnology.com).

To my even greater surprise, yesterday [Blawx was named as the runner up](https://americanlegaltechnology.com/award-winners/) in that category. Congratulations to the winner [LegalMation](http://www.legalmation.com). And thanks again to the judges involved. That’s a real vote of confidence for open source and Rules as Code.

### Comparing “Guided Rules” to Blawx

I am in the process of teaching myself how to use the [Drools Rule Language (DRL) inside Business Central](https://docs.drools.org/7.43.1.Final/drools-docs/html_single/), which is a web-based IDE for Drools. It includes a feature called “guided rules”, which is supposed to provide a more user-friendly interface than just writing code in the DRL language.

For example, one rule in DRL might look like this:
```
package mortgages.mortgages;

import mortgages.mortgages.LoanApplication;  
import mortgages.mortgages.Applicant;

rule "No bad credit checks"  
 salience 10  
 dialect "mvel"  
 when  
  app : LoanApplication( )  
  ( Applicant( creditRating == "OK" ) or Applicant( creditRating == "Sub prime" ) )  
 then  
  app.setApproved( false );  
  app.setExplanation( "Only AA" );  
  retract( app );  
end
```
These mortgage examples come with the open source software, by the way. I still haven’t figured out how to write my own.

When you generate the same rule in the “guided rules” visual interface, you get something that looks more like this:

![](/1__6oyj6uBSZ3GzVuWJgraJYA.png)

You can see why the visual interface might be better. But I want to draw your attention to one part of that picture:

![](/1__RYZPcrSXzFDsX4pwjQZAsg.png)

All the green crosses are for adding different things. All the black symbols I can’t read are for removing things. And all of the arrows are for re-ordering them. None of them tell you what they actually do until you hold your mouse over them. I count about 32 interface elements to achieve those objectives, and you still don’t know which one to click a lot of the time.

I mean… yikes.

### Drag-and-Drop #FTW

![](/1__Cuf7OuXEI6nY__jZ1V9UafQ.png)

This is the same rule, as close as I can manage, implemented in [Blawx](https://www.blawx.com).

If you want to add something, you drag it from the menu on the left and plop it in where it goes. If you want to remove something, you drag it from where it is to the garbage can. And if you want to move something, you drag and drop it to where you want it to go. So you get all the same capabilities, with no visible user interface elements.

That’s an upside to Blawx that I hadn’t noticed before. The Blockly “puzzle piece” metaphor is easier to understand than code, yes, but it is also easier to read and use than other visual coding environments, because the puzzle pieces do all the work of the pluses, minuses, and arrows used in other tools.

### Quick Notes

*   I had a great time doing a Rules as Code talk and Blawx demo for the Canadian Institute for the Administration of Justice’s Legislative Drafting conference last week. I have met some wonderful drafters, and I’m learning a lot from them. If the video becomes publicly available, I’ll let you know.
*   I had the chance to chat briefly today with the organizers of a Rules as Code webinar being put on by the European Commission, in conjunction with the OECD’s “[Government After Shock](https://gov-after-shock.oecd-opsi.org/)” event on November 17. It sounds like I will be doing a Blawx demo in that environment, too. More details to come.
*   Last week I discovered the [ontology work being done at the UK Parliament](https://github.com/ukparliament/ontologies), and immediately sent a fan-boy email to the librarians. We’re going to chat next week, and you can be sure that I’ll be telling you more about that.
*   My “Coding the Law” class has started up at the University of Alberta Faculty of Law. The class is fully subscribed, and I’m looking forward to seeing what the students build with [Docassemble](https://docassemble.org) this semester. If you happen to be an access-to-justice organization in Alberta interested in having my students build you a prototype, let me know!

_I am a lawyer at Round Table Law, I teach “Coding the Law” at the University of Alberta Faculty of Law, and I’m a senior researcher at the Singapore Management University Centre for Computational Law. Computational Law Diary is a series of posts on what I’m thinking about in the computational law world. They are my own opinion, and do not reflect the opinions of the Centre, the Research Programme, SMU, U of A, or anyone else._