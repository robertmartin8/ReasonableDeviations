---
layout: post
title: Gradient tree boosting and XGBoost
category: machine-learning
---

Decision trees make for pretty vanilla classifiers: they do an unspectacular job with most machine learning tasks, and you'd be forgiven for overlooking them when deciding on a classification algorithm. But decision trees happen to be the cornerstone of a powerful class of learning algorithms: *gradient tree boosting* methods. I will try to elucidate the (short) history of gradient tree boosting, starting with the pioneering implementation of boosted trees and ending with the state-of-the-art. I'm afraid there will be a hearty amount of mathematics, but it is really a very intuitive topic at its core.
<!--more-->

Note that I will *not* be covering the actual functioning of decision trees; for that, you may either refer to [my notes]({{ site.url }}/notes/papers/induction_decision_trees) on the original paper, or to the excellent [Wikipedia page](https://en.wikipedia.org/wiki/Decision_tree_learning). 

### Weak learners and boosting

As I have mentioned, I don't think decision trees on their own are especially useful. You might even say that they are 'weak classifiers', or generally, 'weak learners'. This, in fact, is a technical term which refers to predictive models which only do a little bit better than random guessing.

However, Robert Schapire (currently a Princeton professor) discovered in his classic paper [*The Strength of Weak Learnability* (1990)](https://www.cs.princeton.edu/~schapire/papers/strengthofweak.pdf), that it is possible to combine a number of weak learners into a strong learning algorithm -- a process called **boosting**. The first tangible algorithm for doing so came in the form of Adaptive Boosting, or AdaBoost, invented by Freund and Schapire in 1997 (their [paper](http://www.face-rec.org/algorithms/Boosting-Ensemble/decision-theoretic_generalization.pdf) now has about 15,000 citations). Essentially, the idea behind AdaBoost is that you can build a strong algorithm by making weak learners learn from their mistakes. This is done by sequentially fitting new classifiers, each one giving more weight to the datapoints for which their predecessors made incorrect predictions. In the end, you take the weighted 'vote' of all the weak classifiers, *et voilà*.  

I won't go into the details of AdaBoost, because it is slightly tangential to the goal of this post, but Chris McCormick has an excellent exposition of the algorithm [here](http://mccormickml.com/2013/12/13/adaboost-tutorial/).  

### Additive models

Around the same time, a lot of research was going on into **additive expansions**, which are linear combinations with coefficients $\beta$ of **basis functions** $b(x;\gamma)$:

 $$f(x) = \sum_{m=0}^{M} \beta_m b(x;\gamma_m)$$
 
 Here, $x$ is an input vector (e.g training data), and $\gamma_m$ denotes the parameters of the *m*th basis function. 

What is interesting is that additive expansions are fundamentally equivalent to boosting methods, as shown by [Friedman, Hastie and Tibshirani (2000)](https://web.stanford.edu/~hastie/Papers/AdditiveLogisticRegression/alr.pdf). If you take $b(x; \gamma)$ to be a weak learner, then it is clear that $f(x)$ presents a weighted vote of the weak learners, just as in AdaBoost. So now instead of dealing with the somewhat vague concept of boosting, all we have to do is figure out how to fit an additive expansion $f(x)$ to some training data, denoted by $\\{(y_i, x_i)\\}_1^N$. 

A first attempt to do this is to minimise some loss function $L(y_i, f(x_i))$
for each training example, such that the parameters are:

$$\arg\min_{\{\beta_m, \gamma_m \}_1^M} \sum_{i=1}^N L \left( y_i, \sum_{m=1}^M \beta_m b(x_i;\gamma_m)\right)$$

That looks to be quite a monstrous summation, but it is only the cumbersome notation: if you carefully look at the expression, you will see that it is really just choosing $\beta_m$ and $\gamma_m$ values that will minimise the loss of $f(x)= \sum_{m=1}^M \beta_m b(x_i;\gamma_m)$ over all training examples. 

While the above optimisation may not be conceptually difficult, it is *computationally* difficult. Fortunately, additive expansions invite a **forward stagewise** approximation. Instead of trying to optimise the parameters all at once, we do it in stages, slowly building up an approximately optimal model. At each iteration $m$ we find
an optimal $\beta_m$ and $\gamma_m$, and add the new term to the existing predictor function $f_{m-1}(x)$. 


**The Forward Stagewise Additive Modelling (FSAM) algorithm**
> **Set** $~f_0(x) = 0$
> 
> **For** $~m=1,\ldots, M$
> - **Compute**:
> 
> $$(\beta_m, \gamma_m) = \arg \min_{\beta, \gamma} \sum_{i=1}^N L(y_i, f_{m-1}(x_i)+\beta b(x_i;\gamma))$$
> 
> - **Set** $~f_m(x)=f_{m-1}(x)+\beta_m b(x;\gamma_m)$
> 
> **Return** $~f_M(x)$


### Gradient tree boosting  

Unfortunately, the difficulty with the FSAM algorithm is the actual optimisation step:

$$(\beta_m, \gamma_m) = \arg \min_{\beta, \gamma} \sum_{i=1}^N L(y_i, f_{m-1}(x_i)+\beta b(x_i;\gamma))$$

For all but the simplest loss functions, this presents a prohibitively difficult optimisation problem. One solution is gradient boosting, as described by Friedman in his 2001 Paper [*Greedy Function Approximation: a Gradient Boosting Machine*](https://statweb.stanford.edu/~jhf/ftp/trebst.pdf).

It is worth mentioning at this point that although we can use a large variety of models for $b(x;\gamma)$, a decision tree is a common choice. We will adopt the notation conventions from [*Elements of Statistical Learning*](https://web.stanford.edu/~hastie/Papers/ESLII.pdf). A tree with $J$ **terminal nodes** (or 'leaves') is represented as follows:

$$T(x; \Theta) = \sum_{j=1}^J \gamma_j I(x \in R_j), \qquad \Theta = \{R_j, \gamma_j \}_1^J$$

Such a tree partitions the feature space into $J$ disjoint regions/leaves, each having an associated constant $\gamma_j$ which acts as a weight on that leaf. $I$ is the *indicator function*, returning 1 if its argument is true, and 0 if false. $\Theta$ encodes both the leaf weights and the 'structure' of the tree. 

Thus the optimisation step in the FSAM algorithm becomes: 

$$ \Theta_m = \arg \min_{\Theta} \sum_{i=1}^N L(y_i, f_{m-1}(x_i) + T(x_i; \Theta))$$

Note that we no longer need to consider the constants $\beta_m$, as they are effectively absorbed into $\gamma_{jm}$.

One method of optimising the above is to borrow the idea of classical gradient descent, a wonderfully intuitive method which realises that the negative gradient of the loss function points in the direction of fastest decreasing loss, so updating parameters in the direction of the gradient must also decrease loss! Of course, in the case of boosting we are optimising in *function space* rather than *parameter space* -- we are trying to find the *optimal tree* to add to $f_{m-1}(x)$. The components of the gradient vector $\mathbf{g}_m \in \mathbb{R}^N$ are thus given by: 

$$g_{im} = \frac{\partial L(y_i, f_{m-1}(x_i))}{\partial f_{m-1}(x_i)}$$

Again, because $-\mathbf{g}_m$ points in the direction of fastest decreasing loss, we can simply fit the tree to the negative gradient instead of trying to optimise with respect to some loss function – in both cases, the tree will be one that decreases loss, but in the former case we have the computational benefits of fitting with squared error: 

$$\Theta_m = \arg \min_{\Theta} \sum_{i=1}^N (-g_{im} - T(x_i;\Theta))^2$$

I think it is definitely worth reading over the last few paragraphs again; it is a beautifully subtle piece of reasoning. The full algorithm is presented below: we start by initialising the model with the best constant weights. Then, in each round, we fit a tree to the negative gradient before choosing optimal leaf weights.

**Gradient tree boosting algorithm**

> **Set** $~f_0(x) = \arg \min_\gamma \sum_{i=1}^N L(y_i, \gamma)$
> 
> **For** $~m=1,\ldots, M$
> - **For** $~i=1, \ldots, N$ 
>     - **Compute**:
> 
> $$g_{im} = -\frac{\partial L(y_i, f_{m-1}(x_i))}{\partial f_{m-1}(x_i)}$$
> 
> - **Fit** tree to targets $-g_{im}$ giving:
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


### Regularisation

A popular method of regularising boosted tree models, proposed in the original paper by Friedman, is *shrinkage*, wherein additional trees are multiplied by a small shrinkage parameter $\nu$ before being added to the existing model. This would change the update step in the gradient tree boosting algorithm to: 

$$f_m(x) = f_{m-1}(x) + \nu \sum_{j=1}^{J_m} \gamma_{jm} I(x\in R_{jm})$$

Note that we cannot absorb $\nu$ into $\gamma_{jm}$: even though the latter are the *best* parameters (having the least loss on the training data), we must shrink their contribution to the final model to reduce overfitting. The meta-parameter $\nu$ is intuitively the same as the learning rate and is often referred to as such: smaller values (in the order of $0.1$) lead to slower learning, but improved generalisation ability.

Another method that has previously been employed with success is
*subsampling*, in which a certain subset of the training data is
randomly selected (without replacement) at each boosting round.
*Stochastic gradient boosting* subsamples by row; not
all available training data is used in a given round. These methods have been found to give computational benefits and improved generalisation.


## XGBoost

[XGBoost](https://github.com/dmlc/xgboost) (‘eXtreme Gradient Boosting’) is as an open-source
gradient boosting software package that won renown for simply being *the* algorithm you used if you wanted to win [Kaggle](https://www.kaggle.com) competitions. 


XGBoost provides an alternative perspective on the optimisation problem in gradient tree boosting (reproduced below):

$$ \Theta_m = \arg \min_{\Theta} \sum_{i=1}^N L(y_i, f_{m-1}(x_i) + T(x_i; \Theta))$$

Instead of making the 'logical leap' of fitting to gradients, we simply find the second order Taylor expansion of the loss function: 

$$L(y_i, f_{m-1}(x_i) + T(x_i; \Theta)) = L(y_i, f_{m-1}(x_i)) + g_{im} T(x_i; \Theta) + \frac 1 2 h_{im}T(x_i; \Theta)^2$$

where 

$$\begin{aligned}
    g_{im} &= \frac{\partial L(y_i, f_{m-1}(x_i))}{\partial f_{m-1}(x_i)} \\
    h_{im} &= \frac{\partial^2 L(y_i, f_{m-1}(x_i))}{\partial (f_{m-1}(x_i))^2}
    \end{aligned}$$

It looks pretty nasty, but it is really just a standard Taylor expansion of the form:

$$f(x+ k) \approx f(x) + kf'(x) + \frac{k^2}{2}f''(x)$$


Following on from this, XGBoost’s *learning objective*, or *cost
function*, in boosting round $m$ is given by 

$$\begin{gathered}
    C_m = \sum_{i=1}^N \left[L(y_i, f_{m-1}(x_i)) + g_{im} T(x_i; \Theta_m) + \frac 1 2 h_{im}T(x_i; \Theta_j)^2 \right] \\
    +~\Omega(T(x; \Theta_m))
\end{gathered}$$

The ultimate goal is to minimise this cost with respect to $\Theta_m$. The first term is simply the sum over all training examples of the loss approximation. The second term is a *complexity penalty* on the $m$th tree. Many different functions could work here, but the following is an intuitive choice which results in an elegant algorithm (we will subsequently drop the $m$ subscripts for clarity): 

$$\Omega(T(x; \Theta)) = \alpha J + \frac 1 2 \lambda \sum_{j=1}^J \gamma_j^2$$

Recall that $J$ is the number of terminal nodes on the tree, each having weight $\gamma_j$. This complexity penalty punishes having more leaves (controlled by $\alpha$), as well as leaves with greater weight (controlled by $\lambda$). If we substitute this back into the cost function, and at the same time remove terms that are constant in $\Theta$, we get:

$$ C = \sum_{i=1}^N \left[g_{i} T(x_i; \Theta) + \frac 1 2 h_{i}T(x_i; \Theta_j)^2 \right] + \alpha J + \frac 1 2 \lambda \sum_{j=1}^J \gamma_j^2$$

At this stage, one must recall the the mathematical defition of a tree:  $T(x; \Theta) = \sum_{j=1}^J \gamma_j I(x \in R_j)$. By substituting this into the above, and regrouping the terms, we can rewrite the cost function:

$$C = \sum_{j=1}^J \biggl[ \underbrace{\biggl( \sum_{x_i \in R_j} g_i \biggr)}_{G_j} \gamma_j + \frac 1 2 \biggl(\underbrace{\sum_{x_i \in R_j} h_i}_{H_j} + \lambda \biggr) \gamma_j^2 \biggr] + \alpha J$$

Essentially, instead of counting the contributions to the cost over each
training example, we count over each leaf on the tree, summing the
weighted gradient statistics for all training examples in that leaf. New variables $G_j = \sum_{x_i \in R_j} g_i$ and
$H_j = \sum_{x_i \in R_j} h_i$ are defined to simplify the notation.

Remembering that we are trying to minimise the cost with respect to the
parameters $\Theta = \\{R_j, \gamma_j \\}_1^J$, we can first find the
optimal weights $\gamma_j$ by noting that the cost function above is a sum of independent quadratics in $\gamma_j$. The
optimal $x$ in a quadratic $ax^2 + bx + c$ is given by $x = -b/2a$. Likewise, the optimal weights are:

$$\gamma_j^* = -\frac{G_j}{H_j+\lambda}$$

Our final cost function is then:

$$C = -\frac 1 2 \sum_{j=1}^J \frac{G_j^2}{H_j + \lambda} + \alpha J$$

We can use this cost function to greedily grow trees. To evaluate a potential split into ‘left’ and ‘right’ nodes, we consider the *gain*, the amount by which a given split would decrease the cost:

$$ \text{gain} = \frac 1 2 \left[ \frac{G_L^2}{H_L + \lambda} + \frac{G_R^2}{H_R + \lambda} - \frac{(G_L+G_R)^2}{H_L + H_R + \lambda} \right] - \alpha$$

Then, we do a linear scan over the training examples for each feature to decide on the split. What is amazing about XGBoost is really just its cleverness: none of the above mathematics is *difficult* – it is mostly algebraic sleight-of-hand. However, these 'tricks' do really improve the computational efficiency, which is partially why XGBoost is such a popular learning algorithm. 


### Regularistion in XGBoost


Apart from the inbuilt complexity penalty, XGBoost offers shrinkage and row subsampling, but also *column subsampling* -- the regularisation method of choice for Random Forests. In column subsampling, each boosting round neglects a certain portion of the features. This increases the overall model’s robustness by reducing the emphasis placed on one or two key variables, giving other variables ‘a chance to speak’, so to speak. 

## Conclusion

This has been quite an unexpectedly detailed post on the development of XGBoost, and I fear that I have failed in my goal of presenting the subject intuitively. While the logic behind the mathematics is not hard to follow, I will agree that the notation is often quite arcane: there are many different indexing values to keep track of. However, it is very rewarding to be able to understand just how clever XGBoost is, even more so because with it, you can achieve amazing performance on many learning tasks. 

I did a fair bit of research into the theory of gradient tree boosting while writing a paper about using XGBoost to classify potentially outperforming stocks (more on that another time, perhaps). For me, the biggest issue was synthesising a clear and consistent notation from all the conflicting sources (if you think the notation in this post is confusing, wait until you compare three different sets of notation). Nevertheless, some resources I found useful are as follows:

- The Elements of Statistical Learning, which is often the gold-standard reference for machine learning theory. I made notes on the relevant chapters, which I have put up [here]({{ site.url }}/notes/el_stat_learn). 
- If ESL is too rigorous for your liking, there is a slightly easier (though obviously less in-depth) version by the same authors, called Introduction to Statistical Learning. I also have [notes]({{ site.url }}/notes/intro_stat_learn) on the Tree-Based methods chapter. 
- The original XGBoost paper, detailing the theory and implementation. My notes on the paper are here: [XGBoost: A Scalable Tree Boosting System]({{ site.imageurl }}../notes/xgboost.pdf)
- The [XGBoost docs](https://xgboost.readthedocs.io/en/latest/model.html) offer a nice high-level overview of the theory, and are definitely a must-read when it comes to implementations and APIs. 
-  There is a great [youtube video](https://www.youtube.com/watch?v=Vly8xGnNiWs) by Tianqi Chen explaining his algorithm. 

 I personally can never go back to the 'standard' Machine Learning algorithms such as SVM, kNN etc, because in my experience they are just crushed by XGBoost on every possible metric. However, even XGBoost may no longer be the state-of-the-art. Competing software packages such as [LightGBM](https://github.com/Microsoft/LightGBM) and [CatBoost](https://github.com/catboost/catboost) seem to give XGBoost a run for its money; after playing with those libraries, *XGBoost* seems slow.  All said, gradient tree boosting is an exciting field within machine learning, and is very much an active topic of research. For my part, I believe that boosting methods are likely to become the de facto machine learning algorithms, and any data science practitioner would do well to keep themselves in the loop. 
 
 





