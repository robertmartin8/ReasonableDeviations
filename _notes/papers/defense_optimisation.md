---
layout: default
title: In Defense of Optimisation
---

## In Defense of Optimisation: The Fallacy of 1/N

*Kritzman, M., Page, S., & Turkington, D. (2010). In defense of optimization: The fallacy of 1/N. Financial Analysts Journal, 66(2), 31–39. https://doi.org/10.2469/faj.v66.n2.6*

- Much research suggests that 1/N portfolio allocation outperforms mean-variance optimisation. The benefits of 1/N allocation are that it:
    - avoids concentrated positions
    - capitalises on mean reversion at rebalance
    - overweights small-cap stocks
- Optimisation *should* outperform because it takes into account additional knowledge about risk/returns. Cynics argue that instead, optimisers maximise input errors.
- This paper instead argues that the problem is poor models for expected returns, i.e over-reliance on a trailing 5 year mean.
- Markowitz 1952 is about optimisation *given a set of beliefs*, not about how to generate those beliefs.

### Methodology

- The study used 13 datasets representing different asset classes, some going back to 1926.
- For each asset class, multiple portfolios were generated and optimised for the Sharpe ratio. These portfolios were rebalanced at fixed intervals, then performance was compared to the 1/N and market portfolios.
- Rather than using econometrics, three different models for expected returns were used:
    - Constant expected returns, i.e only generating the minimum variance portfolio.
    - Estimating a risk premium for each asset
    - Rolling mean using all available data
- The covariance matrix was estimated with a monthly rolling 5/10/20-year sample covariance matrix.

### Results

- Optimisation of the covariance matrix alone adds value.
- Any reasonable set of expected returns, even chosen based only on arbitrary judgment, outperforms 1/N after optimisation.
- It doesn't matter how sophisticated the optimisation is if the return model is based on a small rolling sample.
- Past risk can be predictive of future risk, but in an efficient market we should not expect mean returns to add much value because expectations of growth should be priced in.
- Only rarely did the optimal portfolios hit the maximum weight constraint.
- The optimal portfolios had a lower **risk concentration** than the 1/N portfolios. Risk concentration is defined as the portfolio volatility divided by the weighted average of individual asset volatilities.

### Further possible improvements

- In reality, there is a big difference between the latent volatility and the price shifts in response to significant events (e.g 2008).
- Thus the covariance matrix is an unreliable measure of diversification performance in turbulent markets, because it does not not distinguish between these two kinds of volatility.
- This can be improved by splitting the series of returns into two subsamples, one for normal noise and one for event-driven movement, then calculating the covariance matrix for each. The final covariance matrix will be the weighted average, overweighting the turbulent.
- **Full-scale optimisation** can be used instead of mean-variance optimisation to achieve better out-of-sample performance and increased flexibility.
