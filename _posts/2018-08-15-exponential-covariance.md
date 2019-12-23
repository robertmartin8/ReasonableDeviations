---
layout: post
title: Exponential Covariance
category: quant-finance
---

For the past few months, I have been doing a lot of research into portfolio optimisation, whose main task can be summarised as follows: is there a way of combining a set of risky assets to produce superior risk-adjusted returns compared to a market-cap weighted benchmark?
The answer of Markowitz (1952) is in the affirmative, with some major caveats. Given the expected returns and the covariance matrix (which encodes asset volatilities and correlations), one can find the combination of asset weights which maximises the Sharpe ratio. But because we don't know the expected returns or future covariance matrix a priori, we commonly replace these with the mean historical return and sample covariance matrix. The problem is that these are very noisy estimators (especially the mean return), so much so that a significant body of research suggests that a naive diversification strategy (giving each asset equal weight) outperforms most weighting schemes. However, work by [Kritzman, Page and Turkington (2010)]({{ site.url }}/notes/papers/defense_optimisation) affirms the intuition that there must be some information in the sample covariance matrix; accordingly, they observe that minimum variance portfolios can beat 1/N diversification. <!--more-->


In my own research, I have found this to be true. Minimum variance optimisation and its variants (no pun intended) can significantly outperform both 1/N diversification and a market-cap weighted benchmark. The nice thing about minimum variance optimisation is that success largely depends on how well you can estimate the covariance matrix, which is easier than estimating future returns. The most common methods are

- **Sample covariance** – standard, unbiased and efficient, but it is known to have high estimation error which is particularly dangerous in the context of a quadratic optimiser.
- **Shrinkage estimators** – pioneered by [Ledoit and Wolf]({{ site.url }}/notes/papers/ledoit_wolf_covariance), shrinkage estimators attempt to reduce the estimation error by blending the sample covariance matrix with a highly structured estimator. 
- **Robust covariance estimates** – estimators that are robust to recording errors, such as Rousseeuw's [Minimum Covariance Determinant ](http://scikit-learn.org/stable/modules/covariance.html#minimum-covariance-determinant).


I have been experimenting with a new alternative, which I call the **exponential covariance matrix** (to be specific, the exponentially-weighted sample covariance matrix). In this post, I will give a brief outline of the motivation and conceptual aspects of the exponential covariance. However, because I am currently using this professionally, I will be intentionally vague regarding implementation details.

## Motivation

In many technical indicators, we see the use of an exponential moving average (EMA) rather than the simple moving average (SMA). The EMA captures the intuition that recent prices are (exponentially) more relevant than previous prices. If we let $p_0$ denote today's price, $p_1$ denote yesterday's price, $p_n$ denote the price *n* days ago, the exponentially weighted mean is given by:

$$\alpha \left[ p_0 + (1-\alpha) p_1 + (1-\alpha)^2 p_2 + \ldots + (1-\alpha)^n p_n + \ldots\right]$$

$\alpha$ parameterises the decay rate ($0 < \|\alpha\| < 1$): by observation we note that higher $\alpha$ gives more weight to recent results, and lower $\alpha$ causes the exponential mean to tend to the arithmetic mean. Additionally, because $1/\alpha = 1 + (1-\alpha) + (1-\alpha)^2 + \ldots$, we note that 'weights' sum to one. 

In practice, we do not compute the infinite sum above. Rather, observing that the weights rapidly become negligible, we limit the calculation to some window. This window is not to be confused with the *span* of the EMA, which is another way of specifying the decay rate – a good explanation can be found on the [pandas documentation](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.ewm.html).

The EMA is useful because it 'reacts' to recent data much better than the SMA owing to the exponential weighting scheme, while still preserving the memory of the timeseries. 

## Covariance

Covariance, like correlation, measures how two random variables *X* and *Y* move together. Let us think about how we would define this metric. For variables that have high covariance, we would expect that when $x$ is high, so is $y$. When $y$ is low, $x$ should be too. To capture this idea, we can proceed as follows.

For each pair $(x_i, y_i)$ in the population, we measure how far away $x_i$ and $y_i$ are from their respective means, then multiply these distances together. If these differences have the same sign, i.e $x_i$ is greater than $\bar{x}$ and $y_i$ is greater than $\bar{y}$, there is a positive contribution to the covariance. If $x_i$ is less than $\bar{x}$ but $y_i$ is greater than $\bar{y}$, there is a negative contribution. We can then sum over all observations, and divide by the number of observations to get some kind of 'average co-variation'. In fact, this is the exact definition of the **population covariance**:

$$Cov(x,y) = \frac{1}{N} \sum_{i=1}^N (x_i - \bar{x})(y_i - \bar{y})$$

Please note that in practice you would always use the **sample covariance** instead, which has a factor of $1/(N-1)$ rather than $1/N$, but this is not something we will worry about here.


## Covariance of asset returns

One would think that the easiest approach is to take two price series (e.g stock prices for AAPL and GOOG), then compute the daily percentage change or log returns, before feeding these into a covariance calculation. This does work, and is the standard approach. But in my view, this is carelessly throwing away a good deal of information, because:

**Covariance does not preserve the order of observations.**

You will get the same covariance whether you provide $(x_2, y_2), (x_{17}, y_{17}), (x_8, y_8), \ldots$ or $(x_1, y_1), (x_2, y_2), (x_3, y_3), \ldots$. 

However, in the case of time series, the order of the returns is of fundamental importance. Thus we need some way of incorporating the sequential nature of the data into the definition of covariance. Fortunately, it is simple to apply our intuition of the EMA to come up with a similar metric for covariance.

We will rewrite our previous definition as follows: 

$$Cov(x,y) = \frac{1}{N} \left[ (x_1 - \bar{x})(y_1 - \bar{y}) +  (x_2 - \bar{x})(y_2 - \bar{y}) + \ldots +  (x_N - \bar{x})(y_N - \bar{y}) \right]$$

Rather than letting $(x_i, y_i)$ be any observations from the dataset, let us preserve the order by saying that $(x_i, y_i)$ denotes the returns of asset *X* and *Y* $i$ days ago. Thus $(x_1 - \bar{x})(y_1 - \bar{y})$ specifically refers to the co-variation of the returns yesterday.

The next step should now be clear. We simply give each co-variation term an exponential weight as follows:

$$Cov(x,y) = \frac{\alpha}{N} \left[ (x_1 - \bar{x})(y_1 - \bar{y}) +  (1-\alpha)(x_2 - \bar{x})(y_2 - \bar{y}) + \ldots +  (1-\alpha)^N(x_N - \bar{x})(y_N - \bar{y}) \right]$$

Or more simply:

$$Cov(x,y) = \frac{\alpha}{N} \sum_{i=1}^N (1-\alpha)^{i-1}(x_i - \bar{x})(y_i - \bar{y})$$

And we are done! This simple procedure is all that is required to incorporate the temporal nature of asset returns into the covariance matrix.

## Conclusion

This post has presented a modification of the covariance matrix especially suited to time series like asset returns. It is simple to extend this to an exponential covariance matrix and use it in portfolio optimisation – it is reasonable to suggest that this matrix will be positive definite if the sample covariance matrix is positive definite. 

I have used the exponential covariance to great effect regarding portfolio optimisation on real assets. Backtested results have affirmed that the exponential covariance matrix strongly outperforms both the sample covariance and shrinkage estimators when applied to minimum variance portfolios. 

At some stage in future, I will consider implementing this in my portfolio optimisation package [PyPortfolioOpt](https://github.com/robertmartin8/PyPortfolioOpt), but for the time being this post will have to suffice. Please [drop me a note]({{ site.url }}/about/) if you'd like to use the ideas herein for any further research or software.

*Update: as of 20/9/18, exponential covariance has been added to PyPortfolioOpt!*
