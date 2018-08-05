---
layout: default
title: DART
---

## DART: Dropouts meet Multiple Additive Regression Tree

*Rashmi, K. V., & Gilad-Bachrach, R. (2015). DART: Dropouts meet Multiple Additive Regression Trees, 38. Retrieved from http://arxiv.org/abs/1505.01866*

- In MART, subsequent predictors typically focus on a small part of the learning problem and have limited predictive power:
    - the first trees have significant contributions, but later trees are overspecialised, only affecting a small fraction of training instances
    - thus the ensemble is very sensitive to the decisions of the first trees.
- DART uses the dropout method from deep neural networks: each boosting round neglects a portion of the trees. DART is thus somewhere between MART and Random Forest – the latter two can be seen as special cases of DART.


### The DART algorithm

- Let *M* denote the current model after *n* iterations such that $M = \sum_{i=1}^n T_i$, with $T_i$ as the tree learnt during the ith iteration.
- DART randomly selects a subset $I \subset \\{ 1, \ldots, n\\}$ and creates a model $M = \sum_{i \in I} T_i$.
- The new tree is then fit to the negative gradient of the loss, as is the case with MART.
- But reintroducing the dropped trees along with this new tree may 'overshoot' the target, so the new tree is scaled by $\frac{1}{1 + \|D\|}$ and dropped trees are scaled by $\frac{\|D\|}{1 + \|D\|}$, where *D* is the set of dropped trees.
- Trees are selected to be dropped with a *Binomial-plus-one* technique: each existing tree is dropped with probability $p$, but if none are dropped then one will be randomly selected. 

### Experiment results

- On ranking tasks, DART beat the previous winners (who used MART) by the same margin that the winner beat 5th place.
- For regression, DART outperforms MART and Random Forest even when restricted to one drop per round.
- For facial recognition, DART had a higher recall but lower accuracy than MART.