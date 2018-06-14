---
layout: default
title: Quantopian Lecture Series
---

# Quantopian Lecture Series

This lecture series can be found on the Quantopian [website](https://www.quantopian.com/lectures), and is free for anyone to follow (though you may have to sign up). The lectures are in the form of jupyter notebooks, but most are accompanied by youtube videos. I think it's a great introduction to quant finance – it puts a lot of emphasis on common pitfalls that must be avoided.



## Contents

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:0 orderedList:0 -->

- [Mean and variance](#mean-and-variance)
- [Statistical moments](#statistical-moments)
- [Correlation](#correlation)
- [Instability of parameters](#instability-of-parameters)
- [Linear regression](#linear-regression)
- [Hypothesis testing](#hypothesis-testing)
- [p-hacking](#p-hacking)
- [Estimating the covariance matrix](#estimating-the-covariance-matrix)
- [Volume, slippage and liquidity](#volume-slippage-and-liquidity)
- [Market impact models](#market-impact-models)
- [The Capital Asset Pricing Model (CAPM)](#the-capital-asset-pricing-model-capm)
- [Factor Models](#factor-models)
- [Principal Component Analysis (PCA)](#principal-component-analysis-pca)
- [Long-Short Equity strategies](#long-short-equity-strategies)
- [Hedging Beta and Sector Exposure](#hedging-beta-and-sector-exposure)
- [Value-at-Risk](#value-at-risk)
- [Integration, Cointegration and Stationarity](#integration-cointegration-and-stationarity)
- [Pairs trading](#pairs-trading)
- [Auto-regressive models](#auto-regressive-models)
- [ARCH and GARCH Models](#arch-and-garch-models)
- [Kalman Filters](#kalman-filters)
- [Futures](#futures)

<!-- /TOC -->


## Mean and variance

- The geometric mean is useful with multiplicative data, but breaks down for negatives.
- The **harmonic mean** is the reciprocal of the arithmetic mean of reciprocals of the data.
- Point estimates can be deceiving, a lot of information is lost when so much information is condensed into one number.
- Variance and std are used more than mean absolute deviation because of differentiability.
- **Chebyshev's Inequality** states that the proportion of samples within k std of the mean is at least $1 - 1/k^2$. This can be derived from **Markov's inequality** which states that $P(X \geq a) \leq E(X) / a$.
- **Semivariance** only looks at the variance of observations below the mean. **Semideviation** is the square root of semivariance. The **target semivariance** chooses the cutoff value to be some target value, instead of the mean.


## Statistical moments

- **Skewness** is a measure of asymmetry. In general, if the mean is higher than the median, skew is positive.

$$Sk = E \left[ \left( \frac{X - \mu}{\sigma}\right)^3 \right]$$

- **Kurtosis** is a measure of peakedness.
- All normal distributions have a kurtosis of 3 by definition, so we often talk about **excess kurtosis**.
    - $K_E > 0 \implies$ leptokurtic (very peaky)
    - $K_E = 0 \implies$ mesokurtic (like the normal distribution)
    - $K_E < 0 \implies$ platykurtic (flat)

$$K_E \approx \frac 1 n \frac{\sum (x_i - \mu)^4}{\sigma^4} -3$$

- The **Jarque-Bera test** tests normality by comparing the skewness and kurtosis of a sample to the normal distribution (the null hyp. is normality)

## Correlation

- Correlation normalises covariance:

$$\rho = \frac{Cov(X,Y)}{\sigma_X \sigma_Y}$$

- Useful when thinking about portfolio diversification
- The rolling correlation can be examined to check for stability.
- The Spearman Rank correlation can help with nonlinearity, because it ranks samples.
- Standard correlation measures work badly when there is a time lag. Autocorrelation techniques should be used.

## Instability of parameters

- In finance, estimates are highly dynamic.
- It is important to consider robustness to movements in every parameter. Algorithms should include the changing variances of input parameters.
- Often best to include (dynamic) confidence intervals.

## Linear regression

- Models take the form $Y = \alpha + \beta_1 X_1 + \ldots + \beta_n X_n$. 
- Ideally, all of the beta terms cancel out to leave pure **alpha** behind.
- You should check for regime changes, in which the initial regression model becomes invalid.
- Linear regression has many requirements, which should be checked. In particular, residuals should be tested for normality and homoskedasticity.
- The **Breusch-Godfrey** test can be used to test for autocorrelation, and the **Newey West method** can be used to correct for it. 

## Hypothesis testing

1. State $H_0$ and $H_A$
2. Identify an appropriate test statistic (and check the assumptions)
3. Compute the critical value from the significance level $\alpha$.
4. Compute the test statistic and compare with the critical value.

- The t-distribution is more robust to small samples than is the normal distribution, and works with sample mean/variance.
- For single variance tests, we can use the $\chi^2$ test.
- To compare two variances, we can use the **F-test** (if the degrees of freedom are the same).

## p-hacking

- If you run enough experiments, you will be able to get the p value arbitrarily low.
- The best method for avoiding this is to run fewer tests, or to make sure that subsequent tests are on new data.
- The **Bonferroni correction** makes it harder to reject $H_0$, by setting the significance level as $\alpha/m$ where *m* is the number of tests you have done. Although this greatly reduces false positives, it may be too stringent.

## Estimating the covariance matrix

- The covariance matrix is very important for position sizing, but it is also highly unstable to estimate. The sample covariance often has undesirable errors.
- One solution is **shrinkage**, reducing the extreme values of the sample covariance matrix (shrinking it towards some target *F*):

$$\hat{S} = (1-\delta) S + \delta F$$

- Different methods can be used to estimate the optimal shrinkage constant $\delta$, though a widely-used option is **Ledoit Wolf shrinkage**.


## Volume, slippage and liquidity

- **Dollar volume** may be more useful than the quantity of an asset traded, because the price itself is dynamic.
- The **volume-weighted average price (VWAP)** is the dollar volume divided by the total number of shares traded. Orders below the VWAP are unlikely to affect the price.
- Normally, there is higher equity volume right after market open and before market close.
- **Slippage** is when your orders affect the market price, and is especially important for high volume strategies as your entry/exit prices will be slightly worse.
- **liquidity** is the ability to execute large trades quickly without slippage.
- Volume is often used as a proxy for liquidity, but if the market is one-sided, or a large shareholder is trading shares via other routes, it may not be a good measure.
- The more capital you have, the more you have to worry about your orders moving the market. Alternatively, having too little capital will increase the effect of commissions and position errors (the difference between continuous and discrete weights).

## Market impact models

- When executing an order, there is a tradeoff between timing risk (risk of price drift between the first and the last block), and the market impact.
- As the order size increases, so does the time taken for complete execution.
- The volatility of a stock peaks at market open then decreases until market close.
- Volatility can either be measured close-to-close historically, which looks at the variance of log returns over some period, or estimated from OHLC candles using the **Garman-Klass Yang-Zhang** estimator, which is more efficient but has more bias if there is drift.
- Spreads are higher when trading assets with:
    - lower market cap
    - higher volatility
    - lower dollar volume
- There are many different methods to quantify market impact, but a ppular one is the **Almgren model**
    - *X* is the number of shares you want to trade over time *T*.
    - There are $\Phi$ shares outstanding with an average daily volume *V* and a daily volatility of $\sigma$
    - $\gamma$ and $\eta$ are market coefficients that must be estimated
    - there are two terms, corresponding to the permanent and temporary impacts.

$$\text{cost} = 0.5 \gamma \sigma \frac X V \left(\frac \Phi V \right)^{1/4} + \eta \sigma \left| \frac{X}{VT}\right|^{3/5} $$

- It should be noted that the Almgren model was developed before the SEC regulated HFT.
- Other models include the **Kissell** model or the JP Morgan model. However, generally models take the form:

$$\text{impact} \propto \sigma_m \cdot (\text{participation rate})^\beta$$

- For smaller trades, a useful approximation is:

$$\text{cost} = 0.1  \left| \frac{X}{VT}\right|^2$$

## The Capital Asset Pricing Model (CAPM)

- Risk is either **idiosyncratic** (firm-specific) or **systematic** (market-wide).
- The risk premium of an asset must be independent of idiosyncratic risk, otherwise it could be diversified away and there would be arbitrage opportunities.
- The CAPM states that the expected return of an asset is equal to the risk free rate plus some risk premium:

$$E(R_i) = R_F + \beta(E(R_M) - R_F)$$

- The CAPM assumes that investors can trade without friction, borrow and lend at the risk free rate, and that they would only buy portfolios located on the **efficient frontier**.
- The **Capital Allocations Line (CAL)** represents different combinations of the portfolio and a risk free asset (plotted on a graph of return vs risk), The y-intercept is $R_F$, and the gradient is the portfolio's Sharpe ratio.
- The **Secuiryt Market Line (SML)** plots $E(R_i)$ against $\beta$: the difference between a security's return and the CAPM expectation is called **alpha**. Securities above the SML can be considered undervalued.
- **Arbitrage Pricing Theory (APT)** generalises the CAPM by acknowledging other risk factors $b_k$ and premia associated with those factors $\lambda_k$

$$E(R_i) = R_F + b_1\lambda_1 + \ldots + b_k \lambda_k$$

- If you somehow know what $E(R_i)$ is, you could compare it with either the CAPM or APT to secure arbitrage.
- To reduce the risk factors, there are a number of hedging strategies:
    - if $Y = \alpha + BX_{SPY}$, consider shorting SPY.
    - pairs trading matches long positions with shorts in correlated stocks to reduce sector exposure.
    - a long-short equity strategy goes long on the top few and short on the bottom few stocks based on some ranking.

## Factor Models

- A factor model is a linear combination of factors to model some stream of returns (could be a security or strategy)
- The strength of a factor can be observed by generating a long-short portfolio based solely on that factor.
- Factor models let you see where your returns are most exposed and how the exposures change over time.
- WHen using models in APT, you should try to use fewer factors to reduce overfitting. But when using facotr models to model risk exposures, you should use as many factors as possible.
- The **active risk** of a strategy is the standard deviation of the (strategy returns - market returns).
- A factor's marginal contribution to active risk (**FMCAR**) is given by

$$FMCAR_j = \frac{b_j \sum_{i=1}^k b_i Cov(F_j, F_i)}{(\text{active risk})^2}$$


## Principal Component Analysis (PCA)

- PCA constructs new features that explain maximal variance: selecting the top *k* principle components thus reduces dimensionality.
- PCA works by calculating the eigenvalues and eigenvectors of the n x n covariance matrix. These eigenvectors (ordered by eigenvalue) correspond to the new components. This procedure is guaranteed to be possible because the covariance matrix is symmetric.
- Running PCA on a portfolio will generate components (i.e asset combinations) that explain most of the variance.


## Long-Short Equity strategies

- Ranks some universe of stocks, then longs the top n% and shorts the bottom n%.
- It is a bet on the ranking system, and strategies are more likely to be market neutral.
- Friction costs are important because of the number of positions.
- There may be a particular time horizon, and the rebalancing frequency must be decided appropriately. 


## Hedging Beta and Sector Exposure

- The **information ratio (IR)** of a strategy is a function of *skill* and *breadth*: $IR = IC \sqrt{BR}$.
- IC is the **information coefficient** (i.e Spearman rank coefficient), which measures the skill.
- While IC may be hard to raise, increasing the breadth is a matter of making many independent bets, which can be done by **beta hedging**.
- A 'bet' is the forecast of the residual of a security's return (asset return minus cost of hedge)
- With beta hedging, the residual is given by $r_i - \beta r_i$.
- In order to add a sector hedge, you must first estimate market beta residuals then calculate sector beta on top of those residuals.
- Overall, breadth can be modeled as:

$$BR = \frac{N}{1+ \rho (N-1)}$$

- In practice, it is important to think aobut market/sector exposure throughout the whole algorithm design process.
- In a single-factor model where $r_i = \alpha_i + \beta_i r_M + \epsilon_i$, we have $Var(r_i) = \beta_i^2 \sigma_M^2 + \sigma_\epsilon^2$, i.e systematic and idiosyncratic risk.
- In a general factor model, we instead have **common factor risk** and **stock-specific risk**. 
- If the returns of a portfolio of *n* assets (or algos) with weighs *w* is given by: $r = \alpha + Bf + \epsilon$, where *f* is a vector of factor returns and *B* is a matrix of factor betas, the portfolio variance is given by:

$$\sigma_\pi^2 = w^TBFB^Tw + w^T D w$$

- The first term is the common risk, the second term is the specific risk, where *D* is a diagonal matrix of variances.
- **Common risk cannot be diversified away**: many individually low common risks can add up.


## Value-at-Risk

- Value-at-Risk (**VaR**) is a measure of worst-case loss. The VaR for *coverage* $\alpha$ is the maximum you can lose with probability $1-\alpha$.
- The historical VaR is just the $100(1-\alpha)$ percentile of returns multiplied by the investment amount.
- If we know that the underlying distribution is normal:

$$VaR_\alpha = \mu - \sigma \Phi^{-1}(\alpha)$$

- It is important to use a lookback window that results in a stable VaR.
- The **expected shortfall**, or **conditional VaR**
, is the average loss of losses so severe that they only occur $(1-\alpha)\%$ of the time, i.e the mean VaR for all coverages between $(1-\alpha)$ and 1:


$$ES_\alpha = \frac{1}{1-\alpha} \int_0^{1-\alpha} VaR_\gamma(X) d\gamma$$

## Integration, Cointegration and Stationarity

- A timeseries is stationary when the parameters of the data-generating process do not change over time.
- This assumption underlies many statistical analyses, so is important to check. The **Dickey Fuller** test can be used, with the null hypothesis that data is non-stationary.
- A stationary timeseries is said to have **order of integration** equal to zero, i.e it is an I(0) timeseries. However, I(0) does not imply stationarity.
- An I(n+1) timeseries is the discrete integral (cumulative sum) of an I(n) process.
- Conversely, an I(n+1) timeseries can be reduced to I(n) by taking the discrete derivative.
- Thus you can test that a series is I(1) by differencing then using Dickey-Fuller.
- It may be the case that a linear combination of I(1) timeseries is I(0). In this case, the set of timeseries are said to be **cointegrated.**
- Cointegrated timeseries usually suggest a real underlying relationship, and form the basis of pairs trading strategies.
- If $X_t$ and $Y_t$ are cointegrated, $Y_t - \beta X_t = \epsilon_t$ should be I(0).
    - thus the residuals in OLS regression should be I(0).
    - as per the Dickey Fuller test, check that the change in the residual is not correlated with the lagged value of the residual. 
    - however, because we are also estimating $\beta$, we have to use the **Augmented Dickey Fuller test**, which makes it harder to reject the null hypothesis of non-stationarity.

## Pairs trading

- Cointegration intuitively means that the spread is mean-reverting.
- Pairs trading is a class of strategies that involves finding cointegrated securities (with a deep economic connection) and trading the spread.
- When estimating $\beta$, it is important to avoid the lookahead bias – always use rolling statistics.
- It may be useful to standardise the spread, then positions can be taken when the spread moves $x\sigma$ away from the mean.
- Once you find an economic connection, you should generate as many pairs as possible.
- Useful statistics are:
    - the **half0life of mean reversion**, which gives some indication of the time it will take for a given spread to revert to the mean spread.
    - the **Hurst exponent**, which describes whether a series is mean reverting.

## Auto-regressive models

- An AR(1) model has only one lag term, i.e:

$$x_t = b_0 + b_1 x_{t-1} + \epsilon_t$$

- An AR(p) model has *p* lag terms:

$$x_t = b_0 + b_1 x_{t-1} + \cdots + b_p x_{t-p} + \epsilon_t$$

- Autoregressive models require a **covariance stationary** timeseries:
    - mean is constant and infinite
    - autocovariance ($Cov(y_t, y_{t-1})$)is constant and finite for a fixed number of periods in the past or future
    - this is a weaker condition than stationarity.
- AR models tend to have more extreme values (fat tails)
- Estimates of variance, p values and confidence intervals will be *very wrong* unless you correct for autocorrelation with a **Newey-West estimator**.
- The *k*-th order autocorrelation is given by:

$$\rho_k = \frac{Cov(x_t, x_{t-k})}{\sigma_x^2}$$

- Fitting an AR model will normally yield too many lags. To choose the correct number, we can consider the **Akaike Information Criterion** (AIC) or the **Bayes Information Criterion** (BIC).
    - compute AIC and BIC for all models
    - compute each model's **relative likelihood** with $l = \exp{((IC_{min} - IC_i) / 2)}$
- We can also plot the autocorrelation function (ACF) or partial ACF and eyeball it to see which lags are the most informative.

## ARCH and GARCH Models

- GARCH models are used when volatility is autoregerssive; ARCH models are a special case in which volatility is a function of time only.
- GARCH(1,1) models are normally characterised by:

$$\sigma_t^2 = a_0 + a_1x_{t-1}^2 + b_1 \sigma_{t-1}^2$$

- These terms are sometimes interpreted as the ambient volatility, adjustment for shocks, and decay rate respectively.
- To test for ARCH conditions, we fit:

$$x_t^2 = a_0 + a_1 x_{t-1}^2 + \ldots + a_p x_{t-p}^2$$

- This estimates $\hat{\theta} = \langle a_0, a_1, \ldots, a_n \rangle$ and the covariance matrix $\hat{\Omega}$. The test statistic is then $F = \hat{\theta} \hat{\Omega} \hat{\theta}^T$ with $F \sim \chi_p^2$
- One can fit a GARCH model by estimating parameters using maximum likelihood estimation, or with the **Generalised Method of Moments**.
- After a GARCH model is fit, Monte Carlo sampling can be used to get an idea of future volatilities – this is not meant to forecast exact values, but provide a possible range.


## Kalman Filters   

- Kalman Filters generate a constantly updating estimate of a distribution's underlying parameters.
- The Kalman filter can be used as a smooth moving average, without having to decide a window length.
- They can be used to model non-linear transitions or non-Gaussian errors.

## Futures

- Futures for a given asset tend to have standard terms.
- In practice, a position is closed by taking an opposite position in the same contract, at which point the exchange settles the cash for you.
- The difference between the spot price and future price is known as the **basis**.
    - a positive basis is a **backwardation** (e.g if there are storage costs)
    - a negative basis is a **contango** situation.
- Closer to expiry, the basis tends towards zero (i.e spot prices and future prices converge)
- Similar to pairs trading on equity, it is possible to trade the spread between futures, which is interesting because there are many assets with obvious links e.g soy beans and soy oil.
- Alternatively, we could trade the spread between a future and an equity or another derivative, e.g between interest rate futures and REITs.
