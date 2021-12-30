---
toc: true
title: 'A Computer Takes the LSAT: Resources'
description: >-
  This is the resources post in a series of posts about encoding LSAT Puzzles in
  the Ergo Lite programming language. To start from the…
date: '2019-04-25T21:52:08.312Z'
categories: []
keywords: [acttl,flora2]
slug: /@roundtablelaw/a-computer-takes-the-lsat-resources-77e5d07d7e3a
---

This is the resources post in a series of posts about encoding LSAT Puzzles in the Ergo Lite programming language. To start from the beginning, [go to the introductory post](https://medium.com/@jason_90344/a-computer-takes-the-lsat-introduction-3a65fd8b982).

If you are interested in learning more about the Ergo Lite programming language, or if you would like to play along with the coding as a learning exercise, here are some useful resources.

The Official LSAT PrepTest from June of 2007 from which the example questions are taken is [available online](https://www.lsac.org/sites/default/files/legacy/docs/default-source/jd-docs/sampleptjune.pdf).

If you would like to use Ergo Lite, which is also called “Flora-2”, you can [get it at SourceForge](http://flora.sourceforge.net/).

If you are interested in a more user-friendly way to use Ergo Lite, check out [Blawx](https://www.Blawx.com), which is a prototype drag-and-drop visual development environment for Ergo Lite.

For help learning Ergo Lite, the primary resource is the [user manual](http://flora.sourceforge.net/docs/floraManual.pdf). However, [Coherent Knowledge](http://coherentknowledge.com/) makes a commercial version of the Ergo Lite language called ErgoAI, and they have learning resources and a mailing list for users of both languages available through their website. The two languages are similar, but not identical, so don’t expect everything that works in ErgoAI to work in Ergo Lite.

If you are using Ergo Lite on a Windows computer, I can recommend using [Visual Studio Code](https://code.visualstudio.com/) as a development environment. Once Ergo Lite is installed on your computer, you can edit the settings of VS Code to allow you to use a Ergo Lite interpreter inside a Terminal window, so you never need to leave VS Code to test what you have written.

This is done by going to File->Preferences->Settings->Features->Terminal and finding the Integrated>Env:Windows setting. Edit the settings.json file and add the line
```
"terminal.integrated.shell.windows": "C:\\path\\to\\\\runflora.bat"
```
Then when you hit CTRL-SHIFT-~, it will run Ergo Lite right inside VS Code.

If you would like to get a copy of all of the final code developed in this series, it is available as a Gist on GitHub, and is included here for your convenience.

If you find any bugs in the code in this series, or serious errors in the posts, please do let me know and I’ll do my best to fix them.

Jason Morris is an LLM Candidate in Computational Law at the University of Alberta Faculty of Law, the operator of [Round Table Law](https://www.roundtablelaw.ca), and co-founder of [Lemma Legal Consulting](https://www.lemmalegal.com). He can be reached at [@RoundTableLaw](https://www.twitter.com/RoundTableLaw) on Twitter. If you need help getting computers to do law, feel free to get in touch.