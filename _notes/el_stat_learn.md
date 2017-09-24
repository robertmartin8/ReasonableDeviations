---
layout: default
title: Elements of Statistical Learning
---

# Elements of Statistical learning – Hastie, Tibshirani, Friedman

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [2. Overview of Supervised Learning](#2-overview-of-supervised-learning)
	- [Statistical decision theory](#statistical-decision-theory)
- [10. Boosting and Additive trees](#10-boosting-and-additive-trees)
	- [10.1 Discrete Adaboost](#101-discrete-adaboost)
	- [10.2 Boosting and additive models](#102-boosting-and-additive-models)
	- [10.3 Forward Stagewise Additive Modelling (FSAM)](#103-forward-stagewise-additive-modelling-fsam)
	- [10.4 Exponential Loss and AdaBoost](#104-exponential-loss-and-adaboost)
	- [10.5 Why Exponential Loss?](#105-why-exponential-loss)
	- [10.6 Loss functions and robustness](#106-loss-functions-and-robustness)
	- [10.9 Boosting trees](#109-boosting-trees)
	- [10.10 Numerical Optimisation via Gradient Boosting](#1010-numerical-optimisation-via-gradient-boosting)
	- [10.10.1 Steepest Descent](#10101-steepest-descent)
	- [10.10.2 Gradient boosting](#10102-gradient-boosting)
	- [10.11 Right-sized trees for boosting](#1011-right-sized-trees-for-boosting)
	- [10.12 Regularisation](#1012-regularisation)
	- [10.13 Interpretation](#1013-interpretation)

<!-- /TOC -->

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
- A **weak classifier** is one whose predictions are only slightly better than random guessing. 
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

- Boosting fits an **additive expansion** in a set of **basis functions**. In this case, the basis functions are $G_m(x) \in \\{ -1, 1 \\}$. More generally, additive expansions take the form:

$$f(x) = \sum_{m=1}^M \beta_m b(x;\gamma_m)$$

- $b(x;\gamma_m)$ are the basis functions, real functions of a multivariate *x* with parameters $\gamma$.
    - in neural networks, $b(x;\gamma_m)$ is the activation function on the weights
    - in trees, $\gamma$ parameterises the splits
- These models are fit by minimising some loss function

$$\min_{\{\beta_m,\gamma_m\}_1^M} \sum_{i=1}^N L\left(y_i, \sum\nolimits_{m=1}^M \beta_m b(x;\gamma_m)\right)$$

- It is hard to explicitly fit this, but we can find a good approximation if we are able to fit a single basis function

$$\min_{\beta, \gamma} \sum_{i=1}^N L(y_i, \beta b(x_i;\gamma))$$

### 10.3 Forward Stagewise Additive Modelling (FSAM)

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


### 10.4 Exponential Loss and AdaBoost

We can show that AdaBoost is equivalent to FSAM with loss:

$$L(y, f(x)) = \exp(-yf(x))$$

For AdaBoost, the basis functions are classifiers $G_m(x) \in \\{ -1, 1 \\}$. With an exponential loss function in FSAM, we have:

$$\begin{align*}
(\beta_m, G_m) &= \arg \min_{\beta, G} \sum_{i=1}^N \exp [-y_i(f_{m-1}(x_i)+ \beta G(x_i))] \\
&= \arg \min_{\beta G} \sum_{i=1}^N \exp[-y_i f_{m-1}(x_i)]\exp[-\beta y_i G(x_i)]
\end{align*}$$

Because $\exp[-y_i f_{m-1}(x_i)]$ is independent of $\beta$ and *G*, it can be regarded as a weight on the observations for that particular iteration $w_i^{(m)}$.

$$\therefore (\beta_m, G_m) = \arg \min_{\beta G} \sum_{i=1}^N w_i^{(m)} \exp[-\beta y_i G(x_i)]$$

To find the minimising $\beta$ and *G*, we manipulate the sum:

$$\sum_{i=1}^N w_i^{(m)} \exp[-\beta y_i G(x_i)] = e^{-\beta} \sum_{y_i = G(x_i)} w_i^{(m)} + e^\beta \sum_{y_i \neq G(x_i)}w_i^{(m)}$$

$$=(e^\beta - e^{-\beta})\sum_{i=1}^N w_i^{(m)} I(y_i \neq G(x_i)) + e^{-\beta} \sum_{i=1}^N w_i^{(m)}$$

By observation, the minimising *G*:

$$G_m = \arg \min_{G} \sum_{i=1}^N w_i^{(m)} I(y_i \neq G(x_i))$$

We can find $\beta$ by differentiating and setting to zero, giving us:

$$\beta_m = \frac 1 2 \ln\left(\frac{1-\epsilon_m}{\epsilon_m} \right)$$

where $\epsilon_m$ is the weighted error. All together, this is equivalent to saying that:

$$w_i^{(m+1)} = w_i^{(m)} \exp(-\beta_m y_i G_m(x_i))$$

which is the same as in AdaBoost.

### 10.5 Why Exponential Loss?

- Exponential loss leads to the elegant reweighting scheme of AdaBoost
- It estimates half the log odds:

$$E(\exp[{-yF(x)}]|x) = P(y=1|x)e^{-F(x)} + P(y=-1|x)e^{F(x)}$$

$$\frac{\partial E(\exp[{-yF(x)}]|x)}{\partial F(x)} = 0 \implies
f(x)=\frac 1 2 \ln \frac{P(y=1|x)}{P(y=-1|x)}$$

- This justifies using its sign as the classification rule

### 10.6 Loss functions and robustness

- The term $yf(x)$ is called the **margin**: observations with positive margin are classified correctly. 
- Therefore loss functions should penalise negative margins 
- The exponential loss function concentrates heavily on large negative margins. If there is noise, or a misspecification of class labels, the exponential loss function will give undue credit to the outlier. 
- An alternative to exponential loss is the **binomial deviance**, given by:

$$L(Y, f(x)) = -\ln(1+e^{-2Yf(x)})$$

- Although the population minimisers of exponential loss and binomial deviance are the same, the binomial deviance has a penalty which is roughly linear in the size of the margin – it is more **robust** to outliers. 
- For regression, squared error severely degrades performance when outliers are introduced. Instead, **Huber loss** can be used: it behaves like squared error loss near zero, but is linear elsewhere. 
- Although exponential and squared error losses are not robust, they lead to elegant models like AdaBoost.
- **Gradient boosting** can produce elegant algorithms for *any* differentiable loss function. 

### 10.9 Boosting trees

- Decision trees partition the feature space into $R_j, j=1,2,\ldots,J$ with a constant $\gamma_j$ in each region such that:

$$x\in R_j \implies f(x) =\gamma_j$$

- $\gamma_j$ is typically just the mean or mode of $y_i$ in $R_j$. 
- Our notation for a decision tree paramaterised by $\Theta= \\{R_j, \gamma_j\\}_1^J$ is then:

$$T(x;\Theta) = \sum_{j=1}^J \gamma_j I(x \in R_j)$$
 
 - We can estimate the parameters as follows:

$$\hat{\Theta} = \arg \min_{\Theta} \sum_{j=1}^J \sum_{x_i \in R_j} L(y_i, \gamma_j) \approx \arg \min_{\Theta} \sum_{i=1}^N \tilde{L}(y_i, T(x_i; \theta))$$

- The first term is the empirical risk for a given set of splits, which is very difficult to optimise. We thus approximate with an easier optimisation criterion. 
- The **boosted tree model** is the sum of such trees using FSAM:

$$f_m(x) = \sum_{m=1}^M T(x;\Theta_m)$$

- At each step *m*, FSAM must solve:

$$\hat{\Theta}_m= \arg \min_{\Theta_m} \sum_{i=1}^N L(y_i, f_{m-1}(x)+T(x_i;\Theta_m))$$

- Substituting the exponential loss would lead to something very similar to AdaBoost. 


### 10.10 Numerical Optimisation via Gradient Boosting

It is possible to numerically find fast approximations for any differentiable loss function. Generally, we want to minimise a loss function:

$$L(f(x)) = \sum_{i=1}^N L(y_i, f(x_i))$$

This can be treated as a numerical optimisation problem

$$\hat{\mathbf{f}} = \arg \min_{\mathbf{f}}L(\mathbf{f})$$

with 'parameters' $\mathbf{f} \in \mathbb{R}^N$ being the approximating function $f(x_i)$ at each of the *N* data points, i.e:

$$\mathbf{f} = \{f(x_1), f(x_2), \ldots, f(x_N)\}^T$$

Numerical optimisation procedures solve:

$$\mathbf{f}_m = \sum_{m=0}^M \mathbf{h}_m, \qquad \mathbf{h}_m \in \mathbb{R}^N$$

$\mathbf{f}_0 = \mathbf{h}_0$ is the initial guess, then each subsequent $\mathbf{f}_m$ is incremented with a **step**  $\mathbf{h}_m$. Different algorithms choose this step differently.

### 10.10.1 Steepest Descent

In steepest descent, we choose $\mathbf{h}_m = -\rho_m \mathbf{g}_m$, where $\rho_m$ is the scalar **step length** and $\mathbf{g}_m \in \mathbf{R}^N$ is the gradient of $L(\mathbf{f}\_{m-1})$. 

$$\mathbf{g}_m = \nabla L(\mathbf{f}_m-1) \implies g_{im} = \frac{\partial L(y_i, f_{m-1}(x_i))}{\partial f_{m-1}(x_i)}$$

The step length can either be constant, or variable as below:

$$\rho_m = \arg \min_\rho L(\mathbf{f}_{m-1} - \rho \mathbf{g}_m)$$

Then the update is simply:

$$\mathbf{f}_m = \mathbf{f}_{m-1} - \rho_m \mathbf{g}_m$$

Steepest descent is very greedy in that it has no consideration for global optima: the only thing that matters is the steepest descent in this iteration. Additionally, it only looks at the training set. However, it is computationally efficient and can work for any differentiable loss function. 


### 10.10.2 Gradient boosting

FSAM is similar to steepest descent; the new tree added at each iteration can be thought of as the gradient update. 

The goal of boosted trees is to minimise 

$$\sum_{i=1}^N L(y_i, f_{m-1}(x) + T(x_i;\Theta_m))$$

But this is difficult for robust loss functions. We can attempt to combine the benefits of steepest descent with those of boosted trees by *fitting new trees to the (negative) gradient* (called the **pseudoresidual**) 

$$\tilde{\Theta}_m = \arg \min_{\Theta} \sum_{i=1}^N (-g_{im} - T(x_i;\Theta))^2$$

This is a proxy method: fitting trees to the gradient (which points in the direction of decreasing loss) should decrease the loss without explicitly having to minimise the loss function. This is computationally efficient. 


**Gradient tree boosting algorithm**

> **Set** $~f_0(x) = \arg \min_\gamma \sum_{i=1}^N L(y_i, \gamma)$
> 
> **For** $~m=1,\ldots, M$
> - **For** $~i=1, \ldots, N$ 
>     - **Compute**:
> 
> $$r_{im} = -\frac{\partial L(y_i, f_{m-1}(x_i))}{\partial f_{m-1}(x_i)}$$
> 
> - **Fit** tree to pseudoresiduals $r_{im}$ giving:
> 
> $$R_{jm}, \qquad j = 1,2,\ldots,J_m$$
> 
> - **For** $~j=1,\ldots,J_m$:
>     - **Compute:**
> 
> $$\gamma_{jm} = \arg \min_\gamma \sum_{x_i \in R_{jm}} L(y_i, f_{m-1}(x_i) + \gamma)$$
> 
> - **Set** $~f_m(x)=f_{m-1}(x)+ \sum_{j=1}^{J_m} \gamma_{jm} I(x \in R_{jm})$
> 
> **Return** $~f_M(x)$


### 10.11 Right-sized trees for boosting

- Large trees degrade the performance of boosting
- A good strategy is to restrict all trees to be the same size $J_m$
- Consider the ANOVA (Analysis of Variance) expansion of the target function $\eta = \arg \min_f E_{XY}L(Y, f(X))$, which represents various interactions within $X^T = (X_1, X_2, \ldots, X_p)$:

$$\eta(X) = \sum_j \eta_j(X_j) + \sum_{jk} \eta_{jk}(X_j, X_k) + \sum_{jkl} \eta_{jkl} (X_j, X_k, X_l)$$

- Most problems involve only low order interactions, but large trees favour higher-order terms.
- The tree size $J$ limits the order; $J-1$ is the maximum:
    - $J=2$ (decision stump) only captures main effects
    - $J=3$ allows two variable interaction
- In general, choose $4 \leq J \leq 8$

### 10.12 Regularisation

- Each iteration usually reduces the training risk (it can be made arbitrarily small). However, there is an optimal $M^*$ for generalisation that can be estimated with CV.
- We can also use **shrinkage** to scale down the contribution from new trees:

$$f_m(x) = f_{m-1}(x) + \nu \sum_{j=1}^{J_m} \gamma_{jm} I(x \in R_{jm})$$

- $\nu$ can be regarded as the learning rate. Empirically, it works best if we set $\nu < 0.1$ and compensate with larger *M* (chosen with early stopping).
- **Stochastic gradient boosting** grows the next tree on a subsample of training observations (typically about half). Such subsampling only performs well when combined with shrinkage. 

### 10.13 Interpretation

For a single tree, the (square) importance of variable $X_l$ is given by summing over all nodes where $l$ is the splitting variable the decrease in error caused by that split:

$$I_l^2(T) = \sum_{t=1}^{J-1}\hat{i}_t^2 I(v(t)=l)$$

This is generalised to additive trees by averaging:

$$I_l^2 = \frac 1 M \sum_{m=1}^M I_l^2 (T_m)$$

These *I* measures are relative: convention is to set the maximum importance to 100 then scale the rest accordingly. 

