---
layout: default
title: Honey, I Shrunk the Sample Covariance Matrix
---

## Honey, I Shrunk the Sample Covariance Matrix

*Ledoit, O., & Wolf, M. (2004). Honey, I Shrunk the Sample Covariance Matrix. The Journal of Portfolio Management, 30(4), 110–119. [https://doi.org/10.3905/jpm.2004.110](https://doi.org/10.3905/jpm.2004.110)*

Mean-variance optimisation (MVO) proposed by Markowitz provides theoretical guarantees, and can be coded to run very efficiently, but it needs good inputs. In particular, it requires a good **risk model**, that is, a good estimator of covariance. 

The sample covariance is the default choice, but often has coefficients with extreme errors which are particularly dangerous in MVO because the optimiser is likely to make large allocations based on these coefficients. One possible improvement is to move extreme values towards the centre, in a process called **shrinkage**.

### Formal description of the portfolio optimisation problem

A manager's portfolio can be thought of as a position in the benchmark (with a weight vector $w_B$) and an additional long/short position in certain stocks – the **active weights** $x$ (also a vector). Clearly, because the portfolio weights ($w_P$) sum to one, we have $w_P = w_B + x$.

The expected stock excess returns are given by the mean portfolio returns less the returns from the benchmark.

$$\alpha = \mu - w_B^T\mu$$

The optimisation problem can then be laid out as follows (where $\Sigma$ denotes the covariance matrix and $\mathbf{1}$ is a conforming vector of ones):

$$
\begin{equation*}
\begin{aligned}
& \underset{x}{\text{minimize}} & & x^T\Sigma x \\
& \text{subject to} & & x^T\alpha \geq g \\
&&& x^T\mathbf{1} = 0 \\
&&& x \geq -w_B \\
&&& x \leq c\mathbf{1} - w_B \\
\end{aligned}
\end{equation*}
$$

The constraints on this optimisation are respectively interpreted as follows:

- The active returns must be more than some target gain $g$, so $x^T\alpha \geq g$
- The active weights must sum to zero (in order for $w_P$ and $w_B$ to both sum to one).
- The portfolio is long only, so $w_P \geq 0 \implies x \geq -w_B$
- The maximum position size is *c* (e.g. we can't have more than 10% of a given stock), so $w_P \leq c\mathbf{1}$.

### Shrinkage estimation of the sample covariance matrix

- The sample covariance is easy to compute and is an unbiased estimator, but there tends to be a lot of estimation error, particularly when there are fewer price datapoints than there are stocks (i.e $N \geq T$). 
- On the other hand, there exist highly structured estimators such as Sharpe's one factor model ([Sharpe 1963](https://www.researchgate.net/publication/227357145_A_Simplified_Model_for_Portfolio_Analysis)), which may be highly biased but have little estimation error.
- The principle behind shrinkage is to compromise between some highly structured estimator $F$ (also known as the **shrinkage target**) and another unstructured estimator. 
- In the case of covariance, the unstructured estimator is clearly the sample covariance matrix $S$. 
- The compromise between these two estimators is determined by the **shrinkage coefficient** $\delta$. The final estimator then takes the form:

$$\delta F + (1-\delta)S, \qquad 0 \leq \delta \leq 1$$

### The shrinkage target

- The shrinkage target should involve only a small number of free parameters (highly structured), but also provide some insight as to the character of the quantity being estimated.
- This paper uses the **constant correlation model**, which sets all pairwse correlations to be equal to the mean of all sample correlations. If $s_{ij}^2$ denotes the sample covariance between two stocks, the shrinkage target matrix $F$ is then given by:

$$f_{ii} = s_{ii} \qquad f_{ij}= \bar{r}\sqrt{s_{ii}s_{jj}}$$


### The shrinkage coefficient

The shrinkage coefficient depends on an estimator $\hat{\kappa}$ where:

$$\hat{\kappa} = \frac{\hat{\pi} - \hat{\rho}}{\hat{\gamma}}$$

Let $T$ denote the number of price datapoints and $y_{it}$ be the returns of the $i$th security at time *t*.

$\hat{\pi}$ estimates the sum of asymptotic variances of the entries of the sample covariance matrix scaled by $\sqrt{T}$:

$$\hat{\pi} = \sum_i^N \sum_j^N \hat{\pi}_{ij} ~~ \text{with} ~~ \hat{\pi}_{ij} = \frac 1 T \sum_{t=1}^T \{(y_{it} - \bar{y}_i)(y_{jt} - \bar{y}_j) -s_{ij}\}^2$$

$\hat{\rho}$ estimates the sum of asymptotic covariances of the entries of the shrinkage target with the entries of the sample covariance matrix, scaled by $\sqrt{T}$

$$\hat{\rho} = \sum_{i=1}^N \hat{\pi}_{ij} + \sum_{i=1}^N \sum_{j=1,j \neq i}^N \frac{\bar{r}}{2} \left( \sqrt{\frac{s_{jj}}{s_{ii}}}\hat{\theta}_{ii,ij} + \sqrt{\frac{s_{ii}}{s_{jj}}}\hat{\theta}_{jj,ij} \right)$$ 

where 

$$\hat{\theta}_{ii,ij} = \frac 1 T \sum_{t=1}^T \{(y_{it} - \bar{y}_i)^2 -s_{ii}\}\{(y_{it} - \bar{y}_i)(y_{jt} - \bar{y}_j) -s_{ij}\}$$

$\hat{\gamma}$ estimates the misspecification of the shrinkage target:

$$\hat{\gamma} = \sum_{i=1}^N\sum_{j=1}^N (f_{ij} - s_{ij})^2$$

The optimal shrinkage constant is given by $\frac{\hat{\kappa}}{T}$, but in principle because this value can be greater than 1 or less than 0, we truncate it with:

$$\hat{\delta} = \max \left \{ 0, \min \left \{ \frac{\hat{\kappa}}{T},1 \right \} \right \}$$

### Empirical evaluation

- Multiple benchmarks of different sizes are generated, where the index weight is determined by market cap.
- To mimic a skilled active manager, random noise is added to the realised excess returns (i.e using hindsight), normalised such that the ***ex ante* information ratio** (IR) is about 1.5.
- Using efficient frontier with constant correlation shrinkage, the ex post IR is computed over 50 trials, and compared with other risk models:
    - the sample covariance matrix
    - shrinkage with Sharpe's single-factor target
    - multi-factor models
- Results show that shrinkage beats sample covariance in all scenarios. 
- Shrinkage with constant correlation beets single-factor shrinkage for $N \leq 225$. 
- Research suggests that adding a constraint on portfolio variance, i.e $\sigma_P^2 = w_P^T \Sigma w_P$, improves overall efficiency.

