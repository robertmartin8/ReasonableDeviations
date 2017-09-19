---
layout: default
title: Introduction to Statistical Learning
---

# Introduction to Statistical Learning – James, Witten, Hastie, Tibshirani

*So far I have only looked at Chapter 8, on tree-based methods, because I needed specifically to learn more about that. Because the boosting method wasn't sufficiently detailed, I turned to Elements of Statistical Learning (the big brother of this textbook)*

## 8. Tree-based methods

### 8.1.1 Regression trees

- The objective is to partition the feature space into box regions $R_1, R_2, \ldots, R_j$, such that the response for a new test example is the mean of the training responses in that particular region. 
- We use a **top-down, greedy** approach called **recursive binary splitting**, where at any iteration, the split with the lowest RSS is chosen.
- At each split, we choose a predictor $X_j$ and a cutoff point *s* to split the feature space into regions:

$$ R_1(j, s) = \left\{X | X_j <s\right\} \qquad 
R_2(j,s) = \left\{X | X_j \geq s\right\}$$

- *j* and *s* are chosen at each step to minimise:

$$RSS = \sum_{i: x_i \in R_1}(y_i - \hat{y}_{R_1})^2 
+ \sum_{i: x_i \in R_2}(y_i - \hat{y}_{R_2})^2$$

- If the number of predictors is moderate, this calculation can be done quickly. 
- To prevent overfitting, we can **prune** a tree $T_0$ into a **subtree** *T*, using 
**cost complexity pruning**. 
- For each $T \subset T_0$, \|T\| is the number of terminal nodes (a measure of complexity). We penalise larger values of \|T\| (very similar to regularisation). Minimise


$$\sum_{m=1}^{|T|} \sum_{i: x_i \in R_m}(y_i - \hat{y}_{R_m})^2 + \alpha |T|$$

$\hat{y}_{R_m}$ is the subset of the predictor space corresponding to the *m*th terminal node. 

**Decision tree algorithm:**

> 1. Grow tree with recursive binary splitting until each terminal node has fewer than k observations, where k is a preset minimum.
> 2. Obtain a sample of subtrees for different values of $\alpha$
> 3. K-fold CV to choose $\alpha$
> 4. Return tree

### 8.1.2 Classification trees

After partitioning, the response for a new test example is the *mode* of the training responses in that particular terminal node.
The classification error rate is only used when pruning. For deciding  There are two errror functions that are used for decision tree classifiers: 

**Gini index**

$$G = \sum_{k=1}^k \hat{p}_{mk} (1 - \hat{p}_{mk}) = 1- \sum_{k=1}^k \hat{p}_{mk}^2$$

- $\hat{p}_{mk}$ is the proportion of training observations in the *m*th region from the *k*th class. 
- smaller if $\hat{p}_{mk}$ is closer to zero or one, i.e if the nodes mostly contain observations from one class. 
- measure of **node purity** (smaller Gini => more pure)

**Entropy**

$$H = -\sum_{k=1}^K \hat{p}_{mk} \log \hat{p}_{mk}$$

- zero information gain if any $p=0$ or $p=1$, i.e very pure. 
- max when $p_1 = p_2 = \ldots p_k$, as it is no better than random guessing. 


There will sometimes be cases where both branches of a final split result in the same class being predicted. This reflects different confidences, as the split must have increased purity. 

### 8.2.1 Bagging

- **Bootstrap aggregation** (bagging) is a method of reducing variance by taking multiple bootstrapped samples (i.e random sample with replacement) from the training set. 
- If $\hat{f}(x)$ is the our estimate of the prediction function
    - Calculate $\hat{f}{}^1(x), \hat{f}{}^2(x), \ldots, \hat{f}{}^b(x)$ for *B* bootstrapped samples. 
    - For regression, the final model is:

$$\hat{f}_{avg}(x)= \frac 1 B \sum_{b=1}^B \hat{f}{}^b(x)$$
    
- If *Y* is categorical, we take the **majority vote**
- Bagging works well for decision trees: we can grow many deep trees without pruning, then average to reduce variance. 
- The probability that a given observation is not in a bootstrap sample of size *n* is $(1-1/n)^n$, which evaluates to around 37% for larger *n*. This means that 1/3 of our data can be used for an **out-of-bag** (OOB) error estimate. 
- Although bagging reduces interpretability, we can calculate the total decrease in RSS/Gini over a certain predictor's splits, averaged over the *B* trees. This will tell us the **variable importance**. 


### 8.2.2 Random Forests

- For each split, only a random subsample of the *m* predictors are considered for splitting. Typically, we choose $m \approx \sqrt p$. 
- Thus bagging is a special case of random forests, with $m=p$. 
- If there is a very strong predictor, each bagged tree would include it. However, this is not the case in random forests as such a predictor would only show up $(p-m)/p$ times. 
- Thus, random forest **decorrelates** the trees, which will further reduce variance when the trees are averaged. 
- We can further decrease *m* if we are worried about correlated predictors. 

### 8.2.3 Boosting regression trees

- In boosting, trees are grown sequentially: each tree fits a modified version of the dataset. 
- At each step, the new tree is fitted to the residuals of the previous iteration's tree. We then update the old tree with a shrunken version of the new tree. 
- $\lambda$ is the **shrinkage parameter** (typically 0.01 or 0.001). Effectively equivalent to the learning rate. 

1. Set $\hat{f}(x) =0$ and $r_i$ = $y_i$, where $r_i$ is the residual
2. For $b = 1,2,\ldots,B$: 
- Fit tree $\hat{f}{}^b$ with $d$ splits onto the training data
- Update $\hat{f}$: $\hat{f}(x) \leftarrow \hat{f}(x) + \lambda \hat{f}{}^b(x)$
- Update residuals $r_i \leftarrow r_i - \lambda \hat{f}{}^b(x_i)$
3. Output $\hat{f}(x) = \sum_{b=1}^B \lambda \hat{f}{}^b(x)$
