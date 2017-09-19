---
layout: default
title: Elements of Statistical Learning
---

# Elements of Statistical learning - Hastie, Tibshirani, Friedman

## 2. Overview of Supervised Learning

### Statistical decision theory

- Let $X \in \mathbb{R}^p$ be a random input vector, and $Y \in \mathbb{R}$ be a random output variable. The goal of machine learning is to find $f(X)$ to predict *Y*. 
- Training an algorithm requires a **loss function** $L(Y, f(X))$, such as the squared error loss $(Y-f(X))^2$
- Formally, we will want to minimise the expected prediction error (EPE). In the case of squared loss, and with the joint distribution of *X* and *Y* being $g(x,y)$:

$$\begin{align*}
EPE(f) &= E(Y-f(X))^2 \\
    &= \int \int [y-f(x)]^2g(x,y)dxdy
\end{align*}$$

- We can *condition* on *X* as follows:

$$\begin{align*}
E(L(x,y)) &= \int \left[ \int L(x,y)g(y|x)dy \right]g(x)dx \\
    &= E_X \left( \int L(x,y) g(y|x)dy) \right) \\
&= E_XE_{Y|X}(L(X,Y)|X)
\end{align*}$$

- We can then minimise the EPE pointwise, finding that the solution is the conditional mean:

$$\begin{align*}
f(x) = \arg \min_{c} E_{Y|X}([Y-c]^2|X=x) \\
\implies f(x) =E(Y|X=x)
\end{align*}$$

- Nearest neighbour methods implement this directly. For a test point *x*, we average the responses of all the points in the neighbourhood of *x*:

$$\hat{f}(x) = \text{Ave}(y_i|x_i \in N_k(x))$$

- For a large training sample size, there will be points in the neighbourhood closer to *x*, and as *k* increases the average will be more stable. 

## 10. Boosting and Additive trees

### 10.1 Discrete Adaboost

- Let $Y= G(X)$ be a classifier such that $G(x) \in \\{-1, 1\\}$
- A **weak classifier** is one whose predictions are only slighlty better than random guessing. 
- **Boosting** sequentially applies weak classifiers to modified versions of the data, producing: $G_m(x), m=1,2,\ldots,M$. Then:

$$G(x) = \text{sign} \left(\sum_{m=1}^M \alpha_m G_m(x)\right)$$

- Each training example has a weight $w_i$, which is updated by the algorithm (increased if the classifier fails the prediction)

**The AdaBoost algorithm**

> **Set** $~w_i = 1/N, ~~i=1,2,\ldots, N$
> 
> **For** $~m=1,\ldots, M$
> - **Fit** $~G_m(x)$ to training data using weights $w_i$
> - **Compute** error:
> 
> $$\epsilon_m = \sum_{\text{wrong}}w_i = \sum_{i=1}^N w_i I(y_i \neq G_m(x_i))$$
> 
> - **Compute** $~\alpha_m = \ln (\frac{1-\epsilon_m}{\epsilon_m})$
> - **Set** $~w_i \leftarrow w_i \cdot \exp(\alpha_m I(y_i \neq G_m(x_i)))$ for $i = 1,2,\ldots,N$ 
> 
> **Return**
> 
> $$G(x) = \text{sign}\left( \sum_{m=1}^M \alpha_mG_m(x)\right)$$

### 10.2 Boosting and additive models 

- Boosting fits an **additive expansion** in a set of **basis functions**. In this case, the basis functions are $G_m(x) \in \{ -1, 1 \}$. More generally, additive expansions take the form:

$$f(x) = \sum_{m=1}^M \beta_m b(x;\gamma_m)$$

- $b(x;\gamma_m)$ are the basis functions, real functions of a multivariate *x* with parameters $\gamma$.
    - in neural networks, $b(x;\gamma_m)$ is the activation function on the weights
    - in trees, $\gamma$ parameterises the splits
- These models are fit by minimising some loss function

$$\min_{\{\beta_m,\gamma_m\}_1^M} \sum_{i=1}^N L\left(y_i, \sum\nolimits_{m=1}^M \beta_m b(x;\gamma_m)\right)$$

- It is hard to expliticitly fit this, but we can find a good approximation if we are able to fit a single basis function

$$\min_{\beta, \gamma} \sum_{i=1}^N L(y_i, \beta b(x_i;\gamma))$$

### Forward Stagewise Additive Modelling (FSAM)

- FSAM is a technique that approximates the fitting of an additive model by sequentially fitting its basis functions. 
- At each iteration *m*, we solve for $b(x;\gamma_m)$ and $\beta_m$ then add it to the current expansion $f_{m-1}(x)$ to produce $f_m(x)$.
- For squared error loss in regression:

$$\begin{align*}
L(y_i, f_{m-1}(x_i)+\beta b(x_i;\gamma)) &= (y_i-f_{m-1}(x)-\beta b(x_i;\gamma))^2 \\
&= (r_{im} - \beta b(x_i;\gamma))^2
\end{align*}$$

- Thus the term that best fits the previous residuals will be added to the model. 

**FSAM algorithm**

> **Set** $~f_0(x) = 0$
> 
> **For** $~m=1,\ldots, M$
> - **Compute**:
> 
> $$(\beta_m, \gamma_m) = \arg \min_{\beta, \gamma} \sum_{i=1}^N L(y_i, f_{m-1}(x_i)+\beta b(x_i;\gamma))$$
> 
> - **Set** $~f_m(x)=f_{m-1}(x)+\beta_m b(x;\gamma_m)$
> 
> **Return** $~f(x)$

