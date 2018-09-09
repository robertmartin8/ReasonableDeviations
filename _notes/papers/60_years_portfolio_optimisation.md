---
layout: default
title: 60 Years of Portfolio Optimisation
---

## 60 years of portfolio optimisation

*Kolm, P. N., Tütüncü, R., & Fabozzi, F. J. (2014). 60 Years of portfolio optimization: Practical challenges and current trends. European Journal of Operational Research, 234(2), 356–371. [https://doi.org/10.1016/j.ejor.2013.10.060](https://doi.org/10.1016/j.ejor.2013.10.060)*


- Markowitz 1952 was the first quantitative treatment of risk/return, via mean-variance optimisation (MVO).
- MVO is difficult in practice: optimal weights are often unstable and not diversified. This is because of estimation errors for the expected returns and variance. 
- If $\Omega$ is the set of permissible portfolios, the classical mean-variance optimisation problem can be framed either in terms of minimising variance or maximising mean return
    
$$
\begin{equation*}
\begin{aligned}
& \underset{\omega \in \Omega}{\text{min}} & & \omega^T \Sigma \omega \\
& \text{subject t<kbd></kbd>o} & & \mu^T\omega \geq R_{min} \\
\end{aligned}
\end{equation*}
$$


- More generally, let $\lambda$ specify the risk-aversion, then the MVO problem is:
- 
$$\underset{\omega \in \Omega}{\text{max}} \mu^T\omega - \lambda \cdot \omega^T \Sigma \omega$$

