---
layout: page
title: Revision notes
---

Over the course of my studies, I have amassed a large quantity of notes and summaries. Over time, I will try to upload as many as possible – it'd be nice if they were all in one place.


<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:0 orderedList:0 -->

- **Cambridge Natural Sciences**
	- [Foundations of Computer Science (Standard ML)]({{ site.url }}/notes/FoCS)
	- [Standard ML exercises and solutions]({{ site.url }}/notes/SML_problems)
	- [Object Oriented Programming (Java)]({{ site.url }}/notes/oop)
	- [IA Materials Science]({{ site.url }}/notes/IA_materials_science)
	- [IA Physics]({{ site.url }}/notes/IA_physics)
- [**Textbook notes**](#textbook-notes)
	- [QED: The Strange Theory – Richard Feynman](#qed-the-strange-theory-by-richard-feynman)
	- [The Feynman Lectures on Physics – Richard Feynman](#the-feynman-lectures-on-physics)
	- [Why Chemical Reactions Happen – Keeler and Wothers](#why-chemical-reactions-happen)
	- [Introduction to Statistical Learning – James, Witten, Hastie, Tibshirani]({{ site.url }}/notes/intro_stat_learn)
	- [Elements of Statistical learning – Hastie, Tibshirani, Friedman]({{ site.url }}/notes/el_stat_learn)
	- [Advances in Financial Machine Learning – Marcos Lopez de Prado]({{ site.url }}/notes/adv_fin_ml)
- [**Notes on academic papers**]({{ site.url }}/academic_papers)
- **IB notes**
	- [Mathematics (higher level)]({{ site.url }}/notes/hl_maths)
	- [Physics (higher level)]({{ site.url }}/notes/hl_physics)
	- [Chemistry (higher level)]({{ site.url }}/notes/hl_chemistry)
- **Online courses**
	- [Bitcoin and Cryptocurrencies – Princeton]({{ site.url }}/notes/princeton_bitcoin) 
	- [Quantopian Lecture Series on Quantitative Finance]({{ site.url }}/notes/quantopian_lectures).
	- [Valuation – Aswath Damodaran]({{ site.url }}/notes/aswath_valuation)
- [**Miscellaneous notes**](#miscellaneous-notes)
    - [The Binomial Theorem](#the-binomial-theorem)
    - [CT1 Financial Mathematics](#ct1-financial-mathematics)
    - [Introduction to Boosted Trees](#introduction-to-boosted-trees)
<!-- /TOC -->

---

## Textbook notes

### QED: The Strange Theory, by Richard Feynman

[QED Chapters 1 and 2](https://reasonabledeviations.files.wordpress.com/2016/02/qed-chapters-1-and-2.pdf "QED Chapters 1 and 2")

[QED Chapters 3 and 4](https://reasonabledeviations.files.wordpress.com/2016/02/qed-chapters-3-and-4.pdf "QED Chapters 3 and 4")

A few years ago I read 'Surely You're Joking, Mr Feynman', and since then fell in love with Feynman's style of teaching physics. 'QED: A Strange Theory' is a semi-technical book in which Feynman discusses the physics substantively rather than just talking about physics as a discipline. Just to clarify, I have no complaint whatsoever with him talking about physics from a more 'meta' point of view, as was done in 'The Character of Physical Law'. I read the book, and loved it. He doesn't sacrifice any of his characteristic charm, while still explaining the counterintuitive theory. This book was transcribed from a series of lectures ([available on youtube](https://www.youtube.com/watch?v=eLQ2atfqk2c)), so is naturally quite verbose.

### The Feynman Lectures on Physics

So far I've only done a few chapters of volume 1. I really intended to work through it in its totality at one stage, but I realised that I should use my time before university to learn things other than physics.

- [Feynman 6](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-6.pdf "Feynman 6")
- [Feynman 7](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-7.pdf "Feynman 7")
- [Feynman 8](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-8.pdf "Feynman 8")
- [Feynman 9](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-9.pdf "Feynman 9")
- [Feynman 10](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-10.pdf "Feynman 10")
- [Feynman 11](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-11.pdf "Feynman 11")
- [Feynman 12](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-12.pdf "Feynman 12")
- [Feynman 13-14](https://reasonabledeviations.files.wordpress.com/2016/02/feynman-13-14.pdf "Feynman 13-14")


### Why Chemical Reactions Happen

Link: [Why Chemical Reactions Happen (notes)]( {{ site.imageurl }}../notes/why_chemical_reactions_happen.pdf)

In January 2015, when I was examining the reading lists for Cambridge and reading example personal statements for Natural Sciences, I noticed that a common denominator was the book _Why Chemical Reactions Happen_, by Keeler and Wothers. I proceeded to read the first two chapters, and was able to easily follow the physics-based reasoning. I therefore listed it on my UCAS personal statement, with the full intention of reading the whole thing before the interview. However, after the first few chapters, the material becomes rather dense (especially with the focus on MO theory). I did endure through it, and made some notes along the way. The notes are quite verbose, because the book itself is quite succinct anyway. I wasn't asked anything about the book in my interview, which I suppose is fortunate because I would have probably struggled. 

---

## Miscellaneous notes

### The Binomial Theorem

Link: [Notes on the Binomial Theorem](https://reasonabledeviations.files.wordpress.com/2016/02/binomial.pdf "Binomial")

An acquaintance of mine was once having trouble wrapping their head around the Binomial Theorem. As someone who likes maths (and likes teaching people maths), I felt some responsibility to try and explain what was happening.  It wasn't as easy as I thought, especially because the Binomial Theorem was so intuitively ingrained in my head. For example, while I understand in my head the origin of the combinatorial coefficient, it's hard to put in words why it should be there.

$$(a+b)^n = \sum \limits_{r=0}^n \binom{n}{r} a^{n-r}b^r$$

Sure, one can simply plug things into the formula, but that is certainly not a helpful explanation, and may let you down for more challenging problems.

I got a bit carried away in trying to intuitively explain this topic, and ended up producing a LaTeX document explaining the Binomial Theorem, and providing exercises with worked solutions.

### CT1 Financial Mathematics

Link: [Notes on CT1 Financial Mathematics]({{ site.imageurl }}../notes/ct1.pdf)

As I was considering a career in actuarial sciences, I thought it might be interesting to take one of the exams to get a better understanding of the material. Following the 'study plan' from the Institute and Faculty of Actuaries, I was torn between CT1 (financial maths) and CT3 (statistics). I decided to choose CT1, because I was already confident with the CT3 content, so I thought that I would learn more from studying CT1.

These notes were made following the free [Warwick notes](https://bcgts.wordpress.com/), which were a sufficient guide for the exams in my opinion.

### Introduction to Boosted Trees

Link: [Introduction to Boosted trees]({{ site.imageurl }}../notes/intro_boosted_trees.pdf).

As I have mentioned elsewhere, for a period I became very interested in gradient tree boosting, particularly XGBoost – a review can be found in [this post]({{ site.baseurl }}{% post_url 2017-10-10-gradient-tree-boosting %}). One of the best expositions of XGBoost was some informal [slides](https://homes.cs.washington.edu/~tqchen/pdf/BoostedTree.pdf), made by its creator, Tianqi Chen. I think it's well worth a little bit of effort to understand XGBoost, because it is a very powerful algorithm.
