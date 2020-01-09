---
layout: post
title: "Portfolio optimisation: lessons learnt"
category: quant-finance
---

Over the past few months I have been busy doing a mixture of blockchain consulting and quantitative finance work for a couple of companies in South East Asia. In particular, I have had the opportunity to investigate the interesting problem of portfolio management for cryptoassets – it was not my first experience with portfolio optimisation, having implemented efficient frontier portfolios at a roboadvisor startup, but this time I took the opportunity to do a deep dive into the subject.  
<!--more-->

Though I am not an expert, I do believe I am at the stage where I can do a kind of write-up, recounting some of the things I've learnt along the way. A lot of my experience has been baked into [PyPortfolioOpt](https://github.com/robertmartin8/PyPortfolioOpt), a comprehensive portfolio optimisation package which I recently revamped, but this post will act as an informal guidebook for those interested in pursing the subject.

## 1. Be careful with in-sample vs out-of- testing

For most people, 'portfolio optimisation' basically refers to mean-variance optimisation (MVO) in the style of Markowitz. MVO will tell you the mathematically optimal portfolio allocation, which sounds great, but the caveat is that it requires the (future) expected returns and the covariance matrix as inputs. For those of us who aren't clairvoyant, these quantities are inaccessible, so we must make do with estimates. The quality of these estimates will be discussed in the next section, but for now I would like to highlight a common mistake, which is backtesting with a forward-looking bias.

The standard way (in textbooks or pedagogical materials) to estimate the expected returns is to take the mean annual return over the past few years. However, it is surprisingly easy to accidentally let future data worm its way into the estimate, resulting in vastly overinflated performance. One of the early mistakes I made was to optimise an equity portfolio using data from 2006-2010, then testing it from 2006-2015. The problem here is the overlap – the 2006 test has had access to data up to 2010, so performance in the 2006-2010 portion of the backtest will be very good.

To be fair, this is often quite an obvious mistake to diagnose, because your portfolios will outperform the benchmark by a ridiculous margin and you will realise that something is wrong. But there are also more subtle ways that you can include future data (e.g survivorship bias), in which case it may not be so clear. 


## 2. Forget about expected returns 

Despite their strong theoretical guarantees, efficient frontier portfolios often have poor real-life performance owing to estimation errors in the inputs.  

Anyone who has ever tried to pick stocks will know how absurd it is to expect that a stock's mean return over the past few years will be a good indicator of its future returns. Such simple relationships have almost certainly been arbitraged away – I therefore contend that mean historical returns are almost pure noise. The problem with this is that MVO has no way of distinguishing noise and signal, so often what happens is that the optimiser highlights the noise in the input. This is why there is a 'running joke' in the portfolio optimisation literature that a mean-variance optimiser is really just an "error maximiser"[^michaud].

In practice, I have found that standard MVO can perform at least in line with the benchmark for equities, but for other asset classes (particularly those which are primarily driven by speculation, like cryptocurrency), the mean historical return is a useless estimator of future return. However, in the next section we will see a simple way round this. 

## 3. You can do better than 1/N

A significant body of research suggests that, in light of the aforementioned failures of MVO, we should instead use 1/N portfolios[^degrappa] – it is noted that they often beat efficient frontier optimisation significantly in out-of-sample testing.

However, an interesting paper by [Kritzman et al. (2010)]({{ site.url }}/notes/papers/defense_optimisation) finds that it is not the case that there is anything special about 1/N diversification: it is just that the expected returns are an incredibly poor estimator and any optimisation scheme which relies on them will likely go astray. 

The easiest way to avoid this problem is to not provide the expected returns to the optimiser, and just optimise on the sample covariance matrix instead. Effectively we are saying that although previous returns won't predict future returns, previous risks might predict future risks. This is intuitively a lot more reasonable – the sample covariance matrix really seems like it should contain a lot of information. Empirical results support this, showing that minimum variance portfolios outperform both standard MVO and 1/N diversification.

In my own work I have found that the standard minimum variance portfolio is a very good starting point, from which you can try a lot of new things:

- Shrinkage estimators on the covariance matrix
- [Exponential weighting]({{ site.url }}/2018/08/15/exponential-covariance/)
- Different historical windows
- Additional cost terms in the objective function (e.g small-weights penalty)


## 4. Don't ignore rebalance costs

When it comes to investment strategy or algorithmic trading, most people leave the "realities" like commission, slippage, and latency to the last stage. However, because of the general high turnover nature of efficient frontier portfolios, this could be quite a dangerous mistake.

In the context of portfolio management, the two main realities that must be accounted for in all backtests are commission and slippage. Latency is not very important because of the longer time horizons involved. Commissions are not hard to analyse: just multiply your broker's percentage commission by the turnover. Slippage, on the other, hand is a little bit more subtle (though it is only really an issue in less liquid markets). There are a number of variables that affect slippage, but the important ones are transaction volume and urgency (refer to models such as that of [Almgren](https://www.courant.nyu.edu/~almgren/papers/optliq.pdf) for more).

Specifically, in the case of cryptoasset portfolios, one major problem is the difference in liquidity between bitcoin and a small altcoin like OmiseGo. A portfolio that shifts positions too much among the altcoins may look like it performs well in backtests, but when traded in real life the 0.5% slippage will be a killer.

Once all transaction costs have been included, you may find that it can be difficult to outperform a buy-and-hold market-cap weighted benchmark. Incidentally, this is another failure with 1/N portfolios: for reasonably volatile assets, there is high turnover at each rebalance and thus a large transaction cost. 


## 5. Overfitting

One thing that I respect a lot about machine learning educators is that they are fundamentally honest about one of the major issues in the subject: overfitting. Essentially, it is possible to encourage most classifiers to wiggle their decision boundary to accommodate all of the training examples at the cost of generalisation ability. This is done by adjusting the hyperparameters (which do things like specify learning rates) and seeing which ones perform better. Hyperparameter tuning is not a bad thing, but it must be done responsibly lest we simply choose the best parameters that fit the particular train/test setup.

When it comes to standard portfolio optimisation, one doesn't really have to worry about this because there aren't any hyperparameters. However, once you start adding different terms to the cost function (each scaled by some parameter), things get a little bit more complicated. For example, let us consider the simple example of minimum variance optimsiation with an L2 regularisation term. 

$$\underset{w}{\text{minimise}} ~ \left\{w^T \Sigma w + \gamma w^T w \right\}$$

The performance of this portfolio varies greatly depending on the choice of $\gamma$. A natural instinct is to run the optimisation for 10 different values of $\gamma$, but if this is not done carefully, we are probably fitting to the noise within the train and test sets.

Remember that ceteris paribus, a simpler model should be preferred. Black-Litterman, or optimisation with an exotic objective with multiple parameters may be seem to improve performacne on the backtest, but the proof will always be in the out-of-sample pudding. Approach portfolio optimisation like you would any financial machine learning task: with a healthy dose of skepticism. 
    
## Conclusion 

I still have a lot to learn about portfolio optimisation, particularly with respect to optimisation of higher moments (skew and kurtosis) and things like copula, but the lessons highlighted in this post definitely still apply. If you found this interesting, check out:

<div> 
<a href="https://github.com/robertmartin8/PyPortfolioOpt"> <h5> PyPortfolioOpt &nbsp; <a class="github-button" href="https://github.com/robertmartin8/PyPortfolioOpt" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star robertmartin8/PyPortfolioOpt on GitHub">Star</a></h5></a>
</div>

<!-- Place this tag in your head or just before your close body tag. -->
<script async defer src="https://buttons.github.io/buttons.js"></script>

## References

[^michaud]: Michaud, R. (1989). “The Markowitz Optimization Enigma: Is Optimization Optimal?” Financial Analysts Journal 45(1), 31–42.
[^degrappa]: Demiguel, Victor & Garlappi, Lorenzo & Uppal, Raman. (2009). Optimal Versus Naive Diversification: How Inefficient is the 1/N Portfolio Strategy?. Review of Financial Studies. 22. 10.1093/rfs/hhm075. 
