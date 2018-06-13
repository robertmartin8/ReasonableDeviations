---
layout: default
title: Neural Networks to Forecast Stock Market Prices
---

## Using Neural Networks to Forecast Stock Market Prices

*Lawrence, R. (1997). Using Neural Networks to Forecast Stock Market Prices. Retrieved from [http://people.ok.ubc.ca/rlawrenc/research/Papers/nn.pdf](http://people.ok.ubc.ca/rlawrenc/research/Papers/nn.pdf) [Literature Survey]*

### Possible approaches to quantitative investing

- Technical analysis – detects trends based on previous patterns and market psychology. Tends to have a lower predictive accuracy, and time lag.
- Fundamental analysis – typically more involved, but is acknowledged to give superior long-term returns
- Time series analysis – either univariate (e.g Box-Jenkins), which involves analysing patterns in **autocorrelations**, or multivariate to discover causal relationships.
- Efficient Market Hypothesis (EMH), i.e buy and hold because you can do no better than random guessing. This is the null hypothesis which any quantitative methods must beat.
- Chaos theoretical approach – the stock market is not purely random: it is a massively complex nonlinear system

Neural networks should be able to add value in every case except for the EMH, because they can capture nonlinearity and extract rules from data. Input data can take many forms, e.g:

- technical indicators such as moving averages or RSI
- fundamentals e.g DCF value
- financial ratios
- sentiment from natural language processing

Performance typically increases with the amount of data, but in the case of time series very old data may just add noise.

# Network organisation

- **Backpropagation** is the most common architecture: 
    - node weights change in proportion to their error contribution and different activations can be used. The sigmoid function is best at learning average behaviour; but tanh is better for deviations. 
    - overfitting can be dealt with by pruning (dropout), or with cross validation
- **Supplementary learning** involves updating individual weights based on the total error. Each output node has an error threshold such that backprop only occurs if the tolerance is exceeded.
- **Moving simulation** involves constantly changing the target, learning, and prediction periods. In each iteration, the train-test-predict window moves forward in time.
- **Genetic algorithms** may be useful for large input dimensionalities or for hyperparameter optimisation (e.g deciding on neural network architecture)
- **Modular neural networks** are often made of smaller backprop networks which act as interchangeable subunits. 
- **Self-organising systems** may be attractive in theory, but require a lot of data, are often difficult to train, and are prone to overfitting.
- **Hybrid systems** pass neural network output through an expert system, which is often rule-based and designed with domain knowledge. 

---

> **Comments**
> 
> - This survey is quite poorly organised, and the ideas are presented in a very illogical order
> - Presents many individual results without linking ideas or adding value.
> - Some of the network architectures discussed (e.g supplementary learning, modular NNs) seem to have disappeared from the literature. They could be profitable to study as a result. 
