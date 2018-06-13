---
layout: default
title: Induction of Decision Trees
---

## Induction of Decision Trees

*Quinlan, J. R. (1986). Induction of Decision Trees. Machine Learning, 1(1), 81–106. [https://doi.org/10.1023/A:1022643204877](https://doi.org/10.1023/A:1022643204877)*

- Top-Down Induction of Decision Trees (TDIDT) is a family of algorithms which construct trees starting from the root and ending with leaves
- The **induction task**: for a universe of objects with specific discrete attributes, develop a classification rule from a training set to classify the object as a positive (P) or negative (N) instance
- Attributes are **inadequate** if the same attributes can result in different classes. 


### ID3

- A brute force search for the best tree is not computationally feasible. 
- ID3 is iterative: it first trains on a subset of the training data, then some of the incorrectly classified objects in the validation set are added to the training subset. 
- ID3 tends to create relatively simple trees
- To from a tree from a collection *C* of objects, we choose a test *T* with outcomes $\{O_1, O_2, \ldots, O_w\}$ which will partition *C* into $\\{C_1, C_2, \ldots, C_w\\}$. *T* is effectively a choice of which attribute we will use to split. 
- The criterion for splitting is based on **information**.  
- One of the assumptions is that *C* will classify objects either *P* or *N* in the same proportion as the representation of these classes in the training data
- Let *C* contain *p* objects of class *P* and *n* objects of class *N*. The information to generate a 'P' or 'N' message:

$$I(p,n) = -\frac{p}{p+n}\log_2 \frac{p}{p+n} - \frac{n}{p+n} \log_2 \frac{n}{p+n}$$

- If we are splitting on variable *A* which takes values $\\{A_1, A_2, \ldots, A_v\\}$, let each $C_i$ have $p_i$ objects of class *P* and $n_i$ objects of class *N*. The **expected information** is the weighted average:

$$E(A) = \sum_{i=1}^v \frac{p_i + n_i}{p+n} I(p_i, n_i)$$

- The **information gain** by branching on *A* is:

$$\text{gain}(A) = I(p,n) - E(A)$$

- The computational complexity of this process is $O( \vert C \vert \cdot \vert A \vert)$


### Noise

- To deal with noise:
    - the algorithm must be able to work with inadequate attributes
    - there must be some criterion to avoid fitting to noise
- If *A* is an attribute with random values, branching on *A* will still cause an information gain:
    - however, setting a minimum gain may also exclude relevant attributes 
    - better is to perform a $\chi ^2$ test of stochastic independence, such that we won't split on *A* unless irrelevance can be rejected. If *A* is irrelevant, then our number of positive predictions should obey:

$$p_i' = p \left( \frac{p_i+n_i}{p+n} \right)$$

- To deal with inadequate attributes, if we cannot classify an object with our tree, we can just label it with the more numerous class.
- Information gain favours attributes with many values (in the discrete case).We can solve this by requiring that all tests have only 2 outcomes, by choosing a subset of the values to become one branch and the remainders to form the other branch.  
