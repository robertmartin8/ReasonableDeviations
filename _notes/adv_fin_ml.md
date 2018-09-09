---
layout: default
title: Advances in Financial Machine Learning
---

# Advances in Financial Machine Learning – Marcos Lopez de Prado


*These notes are not comprehensive: they aim to be an executive summary of the concepts presented in the book, which is very detailed and extensive. Please refer to the textbook itself for implementation details and code.*

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [1. Financial ML as a distinct subject](#1-financial-ml-as-a-distinct-subject)
- [2. Financial Data Structures](#2-financial-data-structures)
	- [2.1 Bars](#21-bars)
		- [2.1.1 Information-driven bars](#211-information-driven-bars)
	- [2.2 Multi-product series](#22-multi-product-series)
	- [2.3 Sampling features](#23-sampling-features)
- [3. Labelling features](#3-labelling-features)
- [4. Sample weights](#4-sample-weights)
- [5. Fractionally differentiated features](#5-fractionally-differentiated-features)
- [6. Ensemble methods](#6-ensemble-methods)
- [7. Cross-validation](#7-cross-validation)
- [8. Feature importance](#8-feature-importance)
- [9. Hyperparameter tuning](#9-hyperparameter-tuning)
- [10. Bet sizing](#10-bet-sizing)
- [11. The dangers of backtesting](#11-the-dangers-of-backtesting)
- [12. Backtesting through CV](#12-backtesting-through-cv)

<!-- /TOC -->

## 1. Financial ML as a distinct subject

*Chapter 1 is available for free online at [SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3104847). I would highly recommend that it be read in its entirety – the below notes do not do justice.*

- Financial academia is a mess: they are either too mathematically pure without practical value, or completely empirical and overfit.
- 'Quantamental' firms that blend ML with discretionary investments are likely to be the ones that perform best, because they can combine human intuition with rigorous mathematical forecasting, and can help to reduce the impact of emotion.
- Financial ML should be considered distinct to other ML.
- Financial ML often fails:
    - firms often treat quants as normal portfolio managers, requiring individual strategies/profits. This encourages overfitting and false positives.
    - individual and easily-discoverable macroscopic alphas are very rare nowadays.
- Discovering microscopic alpha requires a production chain of quants:
    - data curators – experts in microstructure and data protocols
    - feature analysts – discover signals
    - strategists – formulate theories that explain signals
    - backtesting – results shared directly with management to avoid overfitting
    - deployment – specialists in optimised code and HPC.
    - portfolio oversight
- All strategies follow the same life cycle:
    - embargo – test on data after the backtest period
    - paper trading – test on live data feed
    - graduation – manage real money
    - re-allocation – allocation size constantly reallocated
    - decommission

## 2. Financial Data Structures

- Fundamentals are always reported with a a lag; it is important to find out precisely when a datapoint was released.
- Often, fundamental data is backfilled – using this data is a backtesting error.
- Because fundamental data is regularised and accessible, it is unlikely to provide much value on its own. 
- However, fundamentals can often be used as a preliminary screen for other signals.
- Market data tends to be more unstructured, and ranges in complexity from OHLC candles to the FIX messages that can reconstruct order books.
- This data can be used to identify different market participants. For example, human traders often buy in large rounded quantities. 
- Because FIX data is not trivial to process there is likely a greater signal.
- Analytics refer to secondary signals that have usually been purchased from a provider. Though valuable, they are costly and opaque.
- Alternative data is unique primary data that may be difficult to process, but is also the most promising (e.g satellite images).

### 2.1 Bars

- Trade/order observations arrive at irregular frequencies, and must be processed into bars before analysis can be done.
- Bars tend to include a similar set of statistics: OHLC, timestamp, volume, VWAP.
- **Time bars** sample at fixed time intervals.
    - Although popular, they should not be used because markets do not process information at a constant rate. 
    - Time bars have poor statistical properties, like serial correlation, heteroskedasticity, and non-normality. 
- **Tick bars** sample after every *x* transactions:
    - returns behave much more like i.i.d Gaussian
    - however, there may be outliers, like when exchanges carry out auctions at open and close in which case large composite trades are reported as one tick.
- **Volume bars** sample every time *x* units of the asset are exchanged
    - improvement over tick bars because ticks can be different sizes
    - allows for a better analysis of price-volume relationships
- **Dollar bars** sample every time \$x worth of an asset are exchanged
    - takes into account asset price movement. 
    - more robust to changes in the number of outstanding shares.
    - we can make further improvements by adjusting on the free-floating market cap. 

#### 2.1.1 Information-driven bars

- **Tick imbalance bars** (TIBs) are produced more frequently when there is informed trading (asymmetric information).
- For a sequence of ticks $\\{(p_t, v_t)\\}\_{t=1,\ldots,T}$ , we generate $\\{ b_t \\}_{t=1,\ldots,T}$ with $b \in \\{-1,1\\}$ as the sign of the price change between ticks. 
- A bar is formed from *T* ticks such that the **tick imbalance** $\theta_T = \sum_{t=1}^T b_t$ exceeds the expected value of the tick imbalance at the beginning of the bar. 

$$E_0(\theta_T) = E(T)E(b_t) = E(T)[2P(b_t=1)-1]$$

- $E(T)$ and $P(b_t=1)$ can be estimated from exponential moving averages of prior bar values.
- The TIB is then the subset of ticks such that:

$$\| \theta_T\| \geq \| E(\theta_T)\|$$

- We can extend the concept of TIBs to **volume/dollar imbalance bars**, which replace the sign with the signed (dollar) volume, such that:

$$\theta_T = \sum_{t=1}^T b_t v_t$$

- Because large (informed) traders will often split up their trades, it is useful to analyse the sequence of buys in the overall volume, and to sample when it exceeds our expectations. We define the length of the current run as:

$$\theta_T = \max \left( \sum_{t | b_t = 1}^T b_t, - \sum_{t | b_t = -1}^T b_t
\right)$$

- We then sample whenever the run exceeds the expected value of the run lengths.
- Again, this can be extended to volume/dollar runs bars.

### 2.2 Multi-product series

- Often, we may be trading baskets of products or spreads of futures, in which case we run into a number of issues related to the the roll and inversion of the spread.
- A solution is the **ETF trick**, in which we produce a timeseries that reflects the value of \$1 invested.
- This lets us deal with:
    - spread inversion
    - the fact that some assets in a basket may not be tradable at all times
    - dividends
    - the bid-ask spread and rebalance costs

### 2.3 Sampling features

- Sampling is an often-neglected step which attempts to only include informative training examples to improve accuracy.
- Simple downsampling (either via linspace or uniform distribution) is sometimes necessary, but is likely to discard information.
- Event-based sampling attempts to sample after a significant event has occurred. The ML algo may have better performance in these restricted scenarios.
- The CUSUM filter is used to identify a series of upside divergences:

$$S_t = max(0, S_{t-1} + y_t - E_{t-1}(y_t))$$

- This can be extended to a symmetric CUSUM filter, which looks for negative divergences.

## 3. Labelling features

- Labelling is necessary in supervised learning.
- The naive approach is to encode the labels based on the returns, i.e:

$$y =\left\{\begin{aligned}
&-1,& r < - \tau\\
&0,& |r| \leq \tau \\
&1,& r > \tau
\end{aligned}\right.$$

- The returns themselves are given by $r_t = \rho_{t+h}/\rho_t -1$
    - *h* is fixed for time bars
    - we should actually allow for dynamic horizons based on the volatility, and use volume/dollar bars instead. 

- The path followed by prices is often neglected, but is important because of stop losses. 
- The **Triple-Barrier method** involves imagining three barriers for every point: a stop loss, a profit take, and an expiration. 
    - if the stop loss is hit, label -1.
    - if the profit taker is hit, label 1
    - if it hits expiration, label sign(return) or 0.
- Meta-labelling can be used to learn the size of the bets after you know the side.
    - start with a high recall model, i.e one that identifies a high fraction of the existing positives
    - correct for low precision with meta labelling, i.e building a secondary model which is a binary classifier that determines whether we should bet or not.

## 4. Sample weights

- Labels in finance are not IID, e.g because different labels may look at the same set of returns (so they are hardly independent).
- It is important to calculate label uniqueness. This can be done as follows
    - count the number of concurrent labels at each time
    - find the average uniqueness of the label over its lifespan (requires lookahead on the training set)
    - we can then bag with `max_samples` set at the average uniqueness

- An alternative is **sequential bootstrapping**, which continually modifies the probability distribution so that overlaps/repetition are increasingly unlikely. 
- Weight should be a function of uniqueness and the size of the returns. 
- We can apply a time-decay to weight, though "time" does not have to be chronological – e.g it can be cumulative uniqueness.


## 5. Fractionally differentiated features

-  Integer differentiation removes 'memory' from price series:
    - there is a tradeoff between stationarity and memory
    - stationarity is an implicit requirement of most predictive algorithms
- Let *B* be the **backshift operator** such that $B^K = X_{t-k}$. We can define a non integer backshift as follows:

    $$(1-B)^2 X_t = X_t -2X_{t-1} +X_{t-2}$$
    
    $$(1-B)^d = \sum_{k=0}^\infty \binom{d}{k} (-B)^k = 1 - dB + \frac{d(d-1)}{2}B^2 + \ldots $$
    
- This can be thought of as a weighted sum of previous data (at different periods). In practice, we use a window that looks back until the binomial coefficient drops below a certain threshold. 
- We can then find the minimum *d* that achieves stationarity, so we won't throw away excess information with a differencing.
- It often makes sense to start with a cumulative sum, to enforce memory, before achieving stationarity via fractional differentiation. 

## 6. Ensemble methods

- There are three main considerations in any machine learning algorithm: **bias** (underfitting), **variance** (overfitting), and noise.
- Bootstrap aggregation (bagging) fits *N* estimators on different training sets, sampled with replacement. The simple average of forecasts is the ensemble forecast. 
    - reduces variance *if samples are suitably different*
    - bagged accuracy exceeds average accuracy. 
- Random forests (RFs) will still overfit if samples are non IID:
    - this can be combatted by early stopping, or reducing the number of features available to each tree.
    - alternatively, we can reimplement random forest to bag decision trees with `max_samples` equal to the average uniqueness.
    - RF on a PCA rotation tends to be faster without reducing accuracy
- Boosting addresses underfitting, but in practice this is less dangerous than overfitting.
- Bagging can be parallelised, so a cluster of computers can rapidly train bagged classifiers on large datasets.

## 7. Cross-validation

- K-fold CV vastly overinflates results because of the lookahead bias.
- **Purged k-fold CV** solves this by removing training set observations whose labels overlap with test set labels: if a test set label $Y_j$ depends on information $\Phi_j$, training set labels that depend on $\Phi_j$ should be removed.
- Alternatively, a small embargo can be added after a test set before the next training fold. 

## 8. Feature importance 

**Marcos' First Law: Backtesting is not a research tool. Feature importance is.**

- Sometimes estimated importance is reduced because of other features (multicollinearity). This can be addressed with PCA.
- For tree methods, it is easy to use **mean decrease impurity (MDI)**:
    - effects can be isolated by setting `max_features=1`. Features with no gain should be excluded. 
    - this penalises collinear features. 
    - MDI can only be used for tree-based classifiers
- **Mean decrease accuracy (MDA)** is an out-of-sample estimate that permutes different features:
    - can be used for any classifier
    - slow, but highly informative
    - collinearity is problematic
- **Single feature importance (SFI)** computes out-of-sample importance of individual features, but ignores joint predictive power. 
- PCA is a very important sanity check: PCA eigenvalues can be compared with MDI/SFI importance via a weighted **Kendall $\tau$** correlation. 
- To research feature importance, we can compute it for different securities in a universe then research features that are important across many securities. 
- Alternatively, datasets can be stacked and the classifier will learn the importance over the entire universe. However, this requires a HPC cluster.


## 9. Hyperparameter tuning

- Tuning must be done with proper CV, e.g gridsearch with purged K-fold CV + bagging
- F1 scores should be used
- For optimisation over many parameters, random search CV can be used:
    - fixed computational budget
    - we can sample parameters from an adaptive distribution (something like meta-learning)
- Most ML parameters are positive and "non-linear", in that the difference between 0.01 and 1 may be the same as the difference between 1 and 100. Thus, we should sample from a log uniform distribution instead.

$$\ln x \sim U(\ln a, \ln b)$$

- Negative log loss is generally superior to accuracy because it incorporates confidence in predictions. We use negative log loss so that it follows with the intuition that a higher score is better than a lower score.

## 10. Bet sizing 

*This is a hard chapter to summarise because it is terse and highly mathematical.*

## 11. The dangers of backtesting 

A backtest is *not* an experiment. It is a sanity check for behaviour under realistic conditions. There are many errors one can make, but these are the 7 deadly sins:

1. Survivorship bias 
2. Lookahead
3. Storytelling (explaining a historical event post-hoc)
4. Data snooping
5. Ignoring transaction costs (e.g slippage)
6. Succeeding based on outliers, which may not ever happen again
7. Shorting without considering the costs and consequences.

- Even if a backtest is flawless, it is still probably wrong because of selection bias and overfitting.
- *Never* use a backtest report to modify your strategy. 
- Methods of reducing the flaws in backtests:
    - methods should apply to entire asset classes, otherwise they are likely to be a selection bias
    - use bagging 
    - do not backtest until research is complete 
    - record every backtest and deflate performance accordingly 
    - where possible, simulate scenarios rather than using history. 

**Marcos' Second Law: Backtesting while researching is like drink driving. Do not research under the influence of a backtest**

## 12. Backtesting through CV

- Walk forward testing is most common. Though it is hard to implement, it is easily interpretable.
    - however, it is very easy to overfit because only 1 history is tested.
    - thus it may not be a good indicator of future performance.
    - limits the training set 
- CV backtesting is not historically accurate, but may better infer future results:
    - test K alternative histories
    - uses most available data for training
    - great care must be taken to avoid leakage.
- **Combinatorial purged CV** allows multiple paths to be tested.
    - observations are partitioned into *N* ordered groups, of which *k* are tested. Thus there will be $\binom{N}{k}$ splits.
    - the test sets for different splits are then combined into ordered paths 
    - use purging and and embargoes between train and test sets.
    - statistics like the Sharpe ratio can then be analysed over multiple paths
    - leads to lower variance

<!-- ## 13. Backtesting on synthetic data -->