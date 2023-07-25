---
layout: default
title: How to Mine Gold Without Digging
---

## How to Mine Gold Without Digging

*Guo K, Leung T, Ward B. How to Mine Gold Without Digging. SSRN Electron J. 2018;1–23. [10.2139/ssrn.3172514](https://www.ssrn.com/abstract=3172514)*

- Obviously gold prices and miner prices are correlated in general, but divergence is common.
- Gold miners are sensitive to the ERP as well as individual firm profitability
- Outline of the real options model:
  - mining company will close the mine if gold prices go below a cutoff
  - production rate derived from quadratic increasing marginal costs (each marginal ounce of gold costs more to mine as a percentage of the gold price).
  - Equity is treated as a call option on the firm's assets


### Real options model

- Model the spot gold price as following Geometric Brownian Motion. The firm controls the rate of production $u_t$ and has costs $K(u)$, so the infinitesimal cash flow at time *t* is $u_t (1-K(u_t))S_t dt$.
- The firm then maximises the discounted sum of future cash flows, which is a stochastic control problem.
- Assume the cost function has a fixed cost $\kappa$ and is linear in the production rate, i.e $K(u) = \kappa + \alpha u$. We require $\kappa < 1$ (else firm would exit market) and $\alpha > 0$ (else it would produce an infinite quantity of the commodity).
  - this control problem can be solved using the Hamilton-Jacobi-Bellman equation
  - the miner's asset value and production policy are given by:

$$V(S_t) = \frac{(1-\kappa)^2}{4\alpha\delta} S_t$$

$$u^* = \frac{1-\kappa}{2\alpha}$$

- The asset value of the miner is a constant multiple of the spot price:
  - decrases with increasing production cost
  - fixed cost is more important in determining the value
  - decreases with the convenience yield $\delta$.

- Equity can be valued as a call option on firm assets, with strike equal to the total debt less accumulated coupons (Merton, 1974).
- The equity value of a gold miiner can be replicated by some combination of physical gold, the market portfolio, and the risk-free asset.

- The implied leverage of the miner falls with increasing spot gold price. This is **convex** in the gold price, so a falling gold price can lead to a rapidly increasing implied leverage.


### Factor replication

- Gold miners are more similar to options on physical gold than other equities, which is why the common equity factors don't perform well for gold. 
- 70% of the variance in gold miner returns can be explained by the replicating portfolio 
- The replicating portfolio declines slightly more than the gold miner during periods of negative gold returns, but significantly outperforms during periods of ihgh gold prices. Thus is because miners have other real options, e.g. due to hedging activities, or the option to cut costs.
- A Kalman filter can be used to dynamically estimate the model parameters.
