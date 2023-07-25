---
layout: default
title: Forecasting stock market movement with SVM 
---

## Forecasting stock market movement with SVM 

*Huang, W., Nakamori, Y., & Wang, S. (2005). Forecasting stock market movement direction with support vector machine. Computers and Operations Research, 32, 2513–2522. [https://doi.org/10.1016/j.cor.2004.03.016](https://doi.org/10.1016/j.cor.2004.03.016)*

SVMs are used because:

- regularisation on the decision function
- sparsity of the solution (many coefficients are zero)
- unique and globally optimal solution

### Experiment

- Examining weekly changes of the Nikkei 225
- Features: S&P500, USD/JPY exchange rate
- Parameters: $C=50$, Gaussian kernel
- Outputs a direction $\in \\{-1, 1\\}$, such that:

$$\text{direction}_t = F(S_{t-1}^{SP500}, S_{t-1}^{JPY})$$

where $S_{t-1}$ is the log difference between the values at $t-1$ and $t$

- Dataset: weekly data from Jan 1990 to Dec 2002 (676 observations)
- SVM compared with a naive random walk, LDA, QDA, RNN and a combined model

### Results

- SVM had a hit ratio of 73%, RNN 69%, combined model 75%.

---

> ***Comments***
> 
> - Poorly written, with misleading grammatical errors.
> - The bulk of the paper is standard SVM theory which could be copied from a textbook
> - Very vague experiment design, though this seems to be the industry standard for machine learning finance papers. e.g. what parameters were used for the model comparisons?
> - Small dataset
> - No self-criticism/limitations
> - No comments on actually using such a strategy to trade



