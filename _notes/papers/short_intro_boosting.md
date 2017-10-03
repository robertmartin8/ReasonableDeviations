---
layout: default
title: Introduction to Boosting
---

## A Short Introduction to Boosting

**Freund and Schapire**

Let a training set be denoted by $\\{(x_i,y_i)\\}^m$, where $x_i \in$ **domain/instance space**, and $y_i \in $ **label set** *Y*

### AdaBoost

- AdaBoost calls a weak learner in a series of rounds $t = 1, 2, \ldots, T$ and maintains a **weight** $D_t(i)$ on *i*th training example. 
- The weak learner must find a **weak hypothesis** $h_t: X \rightarrow \\{-1, 1\\}$, whose eror is defined by:

$$\epsilon_t = \sum_{i:h_t(x_i) \neq y_i} D_t(i)$$

- Each $h_t$ has an associated $\alpha_t$ given by:

$$\alpha_t = \frac 1 2 \ln \left( \frac{1-\epsilon_t}{\epsilon_t} \right)$$

- $\alpha_t$ is a weight on $h_t$, which reflects the fact that $h_t$ is a good weak learner if $\epsilon_t$ is close to zero or one ($\epsilon \approx 1 \implies -h_t$ is a good learner)
- Weights are updated according to whether h_t(x_i) is correct, and its confidence in the prediction (measured by $\alpha_t$).

$$D_{t+1}(i) = \frac{D_t(i)\exp(-\alpha_t y_i h_t(x_i))}{Z_t}$$

- Note that higher $\alpha_t$ causes bigger updates. $Z_t$ is a normalising factor.
- The final prediction is the sign of the weighted sum of hypotheses:

$$H(x) = \text{sign} \left( \sum_{t=1}^T \alpha_t h_t(x)\right)$$

### Error of AdaBoost

- For $h_t$, let $\epsilon_t = 1/2 - \gamma_t$. $\gamma_t$ measures the improvement over random guessing. It can be shown that the training error of *H* drops exponentially if each $h_t$ is better than random guessing. Specifically, the error is bounded by:

$$\prod_t [2\sqrt{\epsilon_t (1- \epsilon_t)}] \leq \exp(-2\sum_t \gamma_t^2)$$

- The generalisation error will depend on the number of boosting rounds *T*, the sample size *m*, the training error, and the **VC-dimension** of the weak-hypothesis space *d* (a measure of the expressive power of the learners):

$$\text{generalisation error} = \hat{P}[H(x) \neq y] + \tilde{O}\left( \sqrt{\frac{Td}{m}}\right)$$

- Larger margins on the training set lead to better generalisation. Additional boosting rounds may reduce the margin even though the training error stays at zero.

### Relationship to SVM

- We can approach AdaBoost from a perspective of maximising the minimum margin of any training example *i*. This can be written with norms as:

$$\max_{\boldsymbol{\alpha}} \min_i \frac{(\boldsymbol{\alpha} \cdot \mathbf{h}(x_i))y_i}{\Vert \boldsymbol{\alpha} \Vert \Vert \mathbf{h}(x_i) \Vert}$$

- Where $\Vert \boldsymbol{\alpha} \Vert_1 = \sum_t \vert \alpha_t \vert$ and $\Vert \mathbf{h}(x) \Vert_\infty = \max_t \vert h_t(x) \vert$
- SVM has the same goal, but with Euclidean ($\ell_2$ norms) instead of $\ell_1$ and $\ell_\infty$. 
- These norms result in very different margins, especially when dimensionality is high. 
- AdaBoost is a **linear programming** problem, whereas SVM is quadratic. This has computational implementations. 
- SVM and AdaBoost have different approaches to searching high-dimensional space:
    - SVMs can use **kernels** – low-dimensional functions that can act in high-dimensional space.
    - AdaBoost employs **greedy searches** with weak learners

### Experiments and applications

- AdaBoost is easy to implement, and the only meta-parameter that needs tuning is the number of boosting rounds *T*. 
- It is flexible with any choice of weak learner, though simple weak learners tend to have computational benefits. 
- Boosting can identify outliers: they are simply the datapoints with higher weights. 
- However, excess noise is very detrimental to AdaBoost because the algorithm will keep trying to learn from the noise. 

