---
layout: default
title: Bayesian Methods for Hackers
---

# Bayesian Methods for Hackers - Cameron Davidson-Pilon

*The interactive textbook can be cloned on [GitHub](https://github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers). I'm a huge fan of this style of publishing!*

## Introduction

- Bayesian inference is about updating your beliefs in light of new evidence. 
- The Frequentist view is that the probability is the long-run frequency of an event (across alternate realities if need be), while the Bayesian view is that probability is a measure of belief. Thus it is perfectly reasonable for people to have different estimates of Bayesian probability. 
- The **prior probability** of event *A*, $P(A)$, is our initial estimate of the probability. When we update this in light of evidence *X*, we get the **posterior probability** $P(A | X)$.
- In the limit of an infinite amount of evidence, Bayesian and Frequentist statistics are the same, because the prior is infinitely diluted by new evidence. 
- Generally, Bayesian analysis is more useful for small datasets because it informs you of the uncertainty in its estimates (which will be larger for small datasets).
- The fundamental formula of Bayesian inference is Bayes' Theorem:

$$P(A | X ) = \frac{P(X|A)P(A)}{P(X)}$$

- PyMC3 is a **probabilistic programming language**. Probabilistic programs allow variables to be distributions rather than point values, and they run both forward and backwards (propagating assumptions, making inferences).

## PyMC3

The `Model` object defines the parameters and the distribution. e.g. if we are trying to estimate the mean of a Poissonian random variable:

```python
import pymc3 as pm 

with pm.Model() as model:
    lambda_1 = pm.Exponential("lambda_1", 1.0)
    lambda_2 = pm.Exponential("lambda_2", 1.0)
    tau = pm.DiscreteUniform("tau", lower=0, upper=10)
```

- PyMC3 variables can either be stochastic or deterministic. Deterministic variables can be made up from stochastic variables, because if you knew the values of the stochastic variable, there would be no randomness to get to the deterministic variable.
- We ca make multi-dimensional stochastic variables using the `shape` parameter, e.g. `betas = pm.Uniform("betas", 0, 1, shape=N)`
- Any expressions on PyMC3 variables must be compatible with `theano` tensors (on which PyMC3 is built). 

## MCMC



## The Law of Large Numbers


## Loss functions


## More on priors







