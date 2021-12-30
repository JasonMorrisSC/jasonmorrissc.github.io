---
toc: true
title: OpenFisca in Google Colab
description: >-
  I blogged recently about the opportunity to use s(CASP) inside SWISH, the
  web-based development environment for SWI-Prolog.
date: '2021-11-30T17:43:04.402Z'
categories: []
keywords: [openfisca]
slug: /@roundtablelaw/openfisca-in-google-colab-de92e2621d88
---

I blogged recently about the opportunity to [use s(CASP) inside SWISH](/post/roundtablelaw/s-casp-swi-prolog-ccf8e53c951a), the web-based development environment for SWI-Prolog.

![](/1__8aKzhnDy6oE0omexcsIeMw.png)

Today it is another web-based development environment for Rules as Code, [OpenFisca](https://openfisca.org/en/) inside [Google Colab](https://colab.research.google.com/).

#### What is OpenFisca?

[OpenFisca](https://openfisca.org/en/) is probably the world’s most popular open source tool for Rules as Code. It is used extensively in France, and is spreading around the world.

#### What is Google Colab(oratory)?

[Google Colab](https://colab.research.google.com/) is a web-based implementation of Jupyter notebooks-a “literate programming” tool for the Python programming language-and Google’s cloud services. A Python development environment as a (free) service.

#### Less Memo, More Memo

I could explain how it works, here, but nah.

Just [go try it yourself](https://colab.research.google.com/drive/1A-L2GwDPHJkH8H4wZUwedD7-hcKXm5WZ?usp=sharing).

Trust me, you won’t regret the click.

#### Upshot

I _really_ like the result, because it provides a good literate programming environment to do encoding beside the text of legislation, in any approach that is implemented in Python.

It also democratizes access to OpenFisca, eliminating the need to know how to set up a development environment, or even the need for a computer on which Python can be installed. Interestingly, that makes it realistic for a wider variety of public servants to be able to explore, which in turn will make the technology more trustworthy, and more trusted.

OpenFisca is not my favourite approach for Rules as Code. Imperative programming is fundamentally limited for encoding rules, and OpenFisca’s design is focused on the quality of the outputs-high speed, high accuracy micro-simulations of the effects of legislation-not on the qualities of the inputs. But those concerns are academic, like preferring emacs to Word. OpenFisca is popular, open source, and feasible to deploy in places many of my preferred approaches are not. It also has a growing ecosystem of tools and extensions, as demonstrated at [PolicyEngine.org](https://policyengine.org).

Democratizing access to OpenFisca can only help the Rules as Code movement, and I am 100% here for that.

Adding Colab to the mix, in addition to democratizing access, gives OpenFisca a lot of great interface features. And with very little overhead for the developer. That means the distance between being able to encode a Law, and being able to demonstrate the usefulness of that encoding, has been reduced to nearly zero. And zero is where it should ideally be.