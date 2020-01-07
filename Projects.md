---
layout: page
title: Projects
---

## PyPortfolioOpt

[PyPortfolioOpt](https://github.com/robertmartin8/PyPortfolioOpt) implements financial portfolio optimisation functionality in python.  The great thing about python is that there is ostensibly a package for everything (with which you can just `import` and get going), but I have generally found that this is *not* the case for quant finance functionality. The few existing implementations out there were unintuitive and poorly documented - this project was born out of my need for a comprehensive plug-and-play portfolio optimisation library (that can also be modified by advanced users). 

I have written about PyPortfolioOpt extensively elsewhere, so check out the GitHub repo for more.

### Relevant links

- [GitHub repo](https://github.com/robertmartin8/PyPortfolioOpt)
- [Docs](https://pyportfolioopt.readthedocs.io/)
- [Post: Portfolio Optimisation Lessons Learnt]({{ site.baseurl }}{% post_url 2018-09-27-lessons-portfolio-opt %})

<hr>

### MLStocks

The goal of MLStocks is to discover relationships between a stock's fundamental data and its annual performance. This, in theory, should allow us to identify stocks that are more likely 
to outperform the broader market (S&P500). Machine learning is only a small part of this project. The most time-consuming part regarded scraping the data (rather than buying it) and cleaning it into a suitable form. A brief overview of the steps involved are as follows:

1. Scrape historical fundamental data from a non-standard resource on the internet, an old-fashioned website with no convenient API.
2. Automate the download of historical stock prices from Yahoo, Google, and Quandl. 
3. Calculate the forward annual returns for each stock and the S&P500 for each time snapshot.
5. Combine and clean all of this data into a readable csv, using pandas. Deal with missing data using a variety of tricks. 
6. Engineer features that I think are predictive of future returns
7. Develop multiple machine learning models, while remaining very aware of the effects of data snooping. Extra emphasis on a reduction in overfitting, even if it hurts in-sample performance.
8. Aggregate predictions from different models and construct a portfolio.

I have allocated a large percentage of my live portfolio based on these predictions -- I was fortunate enough to invest in NFLX at 144 and AMZN at 902 because of these predictions (though of course these examples have been cherry-picked).

MLStocks is probably my longest running project: its first iteration began in 2015, and I have been slowly improving it ever since. I released an open-source educational version of it, which received positive response. I personally think that MLStocks is best used as an "intelligent" stock screener, with deep fundamental research being done on its outputs. However, I strongly believe that there could be some alpha in long/short portfolios (constructed using a portfolio optimisation algorithm).

### Relevant links 

- [GitHub repo](https://github.com/robertmartin8/machinelearningstocks)
- [Post: DIY MachineLearningStocks]({{ site.baseurl }}{% post_url 2018-03-10-mlstocks-edu %})
- [Post: Automating the download of historical stock price data from yahoo finance]({{ site.baseurl }}{% post_url 2017-07-30-yahoo-historical-prices %})

<hr>

## HyperVault

HyperVault is an enterprise blockchain startup dedicated to providing secure access control to sensitive documents. Think google drive, but for secrets. I built HyperVault with two team-mates over the course of 6 months, during my first year at Cambridge. I worked on both the business (strategising, pitch decks) and tech aspects. The tech was rather interesting -- we used Hyperledger Fabric for the blockchain stuff and NodeJS/expressJS/nunjucks for the web app. I spent a lot of time on the frontend, which is something I normally avoid. However, our functional proof-of-concept was very well received by both technologists and VCs.

### Relevant links

- [LinkedIn](https://www.linkedin.com/company/hypervault/)
- [Post: What we learnt building an enterprise blockchain startup]({{ site.baseurl }}{% post_url 2019-09-01-what-we-learnt-enterprise-blockchain %})

<hr>

## HireYou

HireYou is an interview preparation app specifically targeted at video interviews (run by vendors such as HireVue and Sonru). I have probably done 30+ interviews over the course of two years of applying for internships in financial services and I have found every single one of them to be quite challenging. They are extremely impersonal and allegedly reject candidates for many rather arbitrary reasons, such as not making sufficiently good eye contact *with the web camera* when talking. After my first round of internship applications, I realised that video interviews are quite a specific skill, so I decided to build a preparation app. Fortunately, this coincided with a hackathon, so I was able to build a proof-of-concept with the help of some friends. I then made significant modifications and managed to deploy it live at <https://hireyou.xyz>. Since then, I have been rebuilding the app in Django. 

