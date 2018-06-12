---
layout: page
title: Projects
---

## MLStocks

The goal of MLStocks is to discover relationships between a stock's fundamental data and its annual performance. This, in theory, should allow us to predict whether a stock will outperform the market (S&P500). 

Machine learning is only a small part of this project. The most time-consuming part regarded scraping the data (rather than buying it) and cleaning it into a suitable form. A brief overview of the steps involved are as follows:

1. Scrape historical fundamental data from a non-standard resource on the internet, an old-fashioned website with no convenient API.
2. Automate the download of historical stock prices from Yahoo, Google, and Quandl. 
3. Calculate the forward annual returns for each stock and the S&P500 for each time snapshot.
5. Combine and clean all of this data into a readable csv, using pandas. Deal with missing data using a variety of tricks. 
6. Back test a large number of different machine learning models: K-NN, Naive Bayes, Random Forest, SVC, and XGBoost. At the same time, it is very important to be aware of data snooping and to limit its effects. 
7. Parameter tune the most promising models using scikit-learn's gridsearch. 
8. Manually tune certain parameters to reduce overfitting. 
8. Repeatedly train the models on all of the data, and generate predictions. 
9. Combine the predictions using classical techniques, such as efficient frontier and the Kelly criterion. 

I have allocated a large percentage of my live portfolio based on these predictions -- I was fortunate enough to invest in NFLX at 144 and AMZN at 902 because of these predictions (though of course these examples have been cherry-picked).

MLStocks is probably my longest running project: its first iteration began in 2015, and I have been slowly improving it ever since. In the next phase of the project, I am looking to see how I can combine it with classical quant methods, like volatility modelling. I am also keen on building a statistically rigorous framework for validating performance so that I can be confident that any out-of-sample outperformance is indicative of live trading prospects. Once I sort out my Interactive Brokers, I'd also like to start taking some short positions. 

Although for obvious reasons much of the codebase is proprietary, I released a much simplified educational version of this project [on GitHub](https://github.com/robertmartin8/MachineLearningStocks), (I have also written a [post]({{ site.baseurl }}{% post_url 2018-03-10-mlstocks-edu %}) about it), which seems to be reasonably popular. 

### Related blog posts

1. [Automating the download of historical stock price data from yahoo finance]({{ site.baseurl }}{% post_url 2017-07-30-yahoo-historical-prices %})

2. [DIY MachineLearningStocks]({{ site.baseurl }}{% post_url 2018-03-10-mlstocks-edu %})
<hr>


## PyPortfolioOpt

[PyPortfolioOpt](https://github.com/robertmartin8/PyPortfolioOpt) implements financial portfolio optimisation functionality in python.  The great thing about python is that there is ostensibly a package for everything (with which you can just `import` and get going), but I have generally found that this is *not* the case for quant-finance functionality. The few existing implementations out there were unintuitive and poorly documented - this project was born out of my need for a comprehensive plug-and-play portfolio optimisation library (that can also be modified by advanced users). 

PyPortfolioOpt includes classical methods such as mean-variance optimisation, more recent improvements such as covariance shrinkage, and novel experimental features such as L2 regularisation on the weights. The roadmap is currently full of new things that I am looking to implement, including Black-Litterman selection, Monte Carlo methods, and better backtesting. Everything that is implemented has been extensively documented; there is a [dedicated documentation](https://pyportfolioopt.readthedocs.io/en/latest/) site hosted on ReadTheDocs, which both summarises the relevant theory, and provides an in-depth reference to the offered functionality.

This was the first of my projects to be released on [PyPI](https://pypi.org/project/PyPortfolioOpt/), so it was a good opportunity for me to get some experience in python package distribution and continuous integration testing on [Travis](https://travis-ci.org/robertmartin8/PyPortfolioOpt). 
