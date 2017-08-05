---
layout: post
title: Retrieving historical stock prices from yahoo finance as of July 2017 
---

Yahoo Finance has long been an excellent free financial resource with a wealth of data and a convenient API. 

But not any more. As of May 2017, they have [discontinued their API](https://forums.yahoo.net/t5/Yahoo-Finance-help/Is-Yahoo-Finance-API-broken/td-p/250503), probably as a result of Yahoo's pending acquisition by Verizon. This means that excellent tools like `pandas-datareader` are now irreparably broken, much to the dismay of many amateur algorithmic traders or analysts. 

Of course, you could move to another free data source like Google Finance, but that personally wasn't a very attractive option for me, since Yahoo's adjusted closes seem to be more accurate and take into account dividends as well as stock splits. 

This is what I needed:
**A simple way to download historical daily adjusted closing prices for every ticker in a list of tickers, from yahoo finance**

It turns out that there is a rather hackish workaround which allows us to download the data as .csv files, which of course can then be read into dataframes etc. 

Just a legal disclaimer. This involves making a large number of requests on yahoo finance, which may 'look like' a DDOS. Clearly, I am not trying to DDOS yahoo finance, I am merely trying to parse data for educational purposes. I am not responsible for how you use the methods demonstrated in this post. 


## Overview

We can parse the wayback


