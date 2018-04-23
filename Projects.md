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

I have allocated a large percentage of my live portfolio based on these predictions -- I was fortunate enough to invest in NFLX at 144 and AMZN at 902 because of these predictions (though of course I cherry picked those examples).

MLStocks is probably my longest running project: its first iteration began in 2015, and I have been slowly improving it ever since. In the next phase of the project, I am looking to see how I can combine it with classical quant methods, like volatility modelling. I am also keen on building a statistically rigorous framework for validating performance so that I can be confident that any out-of-sample outperformance is indicative of live trading prospects. Once I sort out my Interactive Brokers, I'd also like to start taking some short positions. 

Although for obvious reasons much of my codebase is proprietary, 
I released a much simplified educational version of this project [on GitHub](https://github.com/robertmartin8/MachineLearningStocks), (I have also written a [post]({{ site.baseurl }}{% post_url 2018-03-10-mlstocks-edu %}) about it), which seems to be reasonably popular. 

### Related blog posts

1. [Automating the download of historical stock price data from yahoo finance]({{ site.baseurl }}{% post_url 2017-07-30-yahoo-historical-prices %})

2. [DIY MachineLearningStocks]({{ site.baseurl }}{% post_url 2018-03-10-mlstocks-edu %})
<hr>
