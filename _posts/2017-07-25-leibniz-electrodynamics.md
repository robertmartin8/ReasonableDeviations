---
layout: post
title: The Leibniz integral rule in electrodynamics
---

I have been slowly working through David Griffith's famous textbook, *Introduction to Electrodynamics*. It is 
known for having a large number of reasonably difficult exercises, which are instrumental in conveying some concepts not directly addressed in the main text.

I discovered an amazing shortcut to one of the questions, which was not noted in the solution manual. The shortcut involves (ab)using a very useful, if slightly overlooked theorem in calculus – the **Leibniz integral rule**. I will briefly discuss this rule, before working through the problem. 

## The Leibniz integral rule

The Leibniz rule concerns derivatives of integrals; the most basic form is as follows: 

$$\frac{d}{dx} \left( \int_a^b f(x,t) dt  \right) = \int_a^b \frac{\partial}{\partial x} f(x,t) dt$$

This can be proven by letting $I(x) = \int_a^b f(x,t) dt$, then using the definition of the derivative to evaluate $I'(x)$, which will be left as an exercise for the reader (lol).  However I hope you can get an intuitive feel for this: the integral measures the total variation with respect to $t$, whereas the derivative measures the rate of change with respect to $x$, so it makes sense that the order of operation doesn't really matter. 

A slightly more general version concerns the case where the limits of the integral are variable:

$$\frac{d}{dx} \left( \int_{a(x)}^{b(x)} f(x,t) dt  \right) =  f\left(x, b(x)\right)\frac{d b(x)}{dx} - f(x, a(x))\frac{d a(x)}{dx} + \int_{a(x)}^{b(x)}\frac{\partial}{\partial x} f(x,t) dt$$

Effectively, it is the same as what we had before, plus some additions to take into account of the variable limits. This formula can be explained in terms of the fundamental theorem of calculus, but I'd rather not go into that here as it is besides the point. For now, just accept the above rule (it is surprisingly useful). Let's go through an example to evaluate the following expression:

$$\frac{d}{dx} \int_{\sin x}^{\tan x} \sqrt{1+ t^3}dt$$

Hint: do *not* do it by evaluating the indefinite integral, substituting in the limits, then differentiating. Applying the Leibniz rule, we see that:

$$\begin{aligned}
\frac{d}{dx} \int_{\sin x}^{\tan x} \sqrt{1+ t^3}dt &= \sqrt{1+ \tan^3 x}\frac{d}{dx}\tan x - \sqrt{1 + \sin^3 x} \frac{d}{dx} \sin x\\
&= \sec^2x\sqrt{1+ \tan^3 x} - \cos x \sqrt{1 + \sin^3 x}  
\end{aligned}$$

## The electrodynamics problem

I encountered this problem in Chapter 2 (Electrostatics) of Griffith's *Introduction to Electrodynamics*. It is reproduced, with a slight edit, below:

> **Problem 27** Find the electric field on the axis of a uniformly charged solid cylinder, at a distance *z* from the centre. The length of the cylinder is *L*, its radius is *R*, and the charge density is $\rho$. 

Our general strategy will be to first find the potential (or rather, an *expression* for the potential), by decomposing the cylinder into simpler shapes. Then we will conclude by noting that $\mathbf{E} = - \nabla V$, and evaluating accordingly. 

A cylinder can be thought of as many discs; a disc can be thought of as many rings; a ring can be thought of as many point charges. We will thus build a cylinder. 

### Step 1: One point charge






