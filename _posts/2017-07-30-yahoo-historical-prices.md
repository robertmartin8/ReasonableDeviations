---
layout: post
title: Retrieving historical stock prices from Yahoo Finance with no API 
category: quant-finance
---

Yahoo Finance has long been an excellent free financial resource with a wealth of data and a convenient API, allowing open source programming libraries to access stock data. But not any more. As of May 2017, they have [discontinued their API](https://forums.yahoo.net/t5/Yahoo-Finance-help/Is-Yahoo-Finance-API-broken/td-p/250503), probably as a result of Yahoo's pending acquisition by Verizon. This means that excellent tools like `pandas-datareader` are now broken, much to the dismay of many amateur algorithmic traders or analysts. It turns out that there is a rather hackish workaround which allows us to download the data as CSV (i.e spreadsheet) files, which of course can then be read into excel, pandas dataframes etc.
<!--more-->

Just a legal disclaimer. This method involves making a large number of requests on Yahoo Finance, which may 'look like' a DDOS. Clearly, I am not trying to conduct a DDOS – I am merely trying to parse data for educational purposes. I am not responsible for how you use the methods demonstrated in this post. 

***Update as of 10/2/18**: there is now a [library on GitHub](https://github.com/ranaroussi/fix-yahoo-finance) that puts this post to shame, with a direct pandas-datareader interface. I will leave this post up for legacy, but for any serious implementations, I wholeheartedly recommend the aforementioned tool.*

***Update as of 20/5/18**: it seems that `fix-yahoo-finance` is becoming very inconsistent. So I guess this post does have some utility after all!*

***Update as of 9/1/20**: looking back on this post, I don't think my solution is very good. A much better way would be to use `requests` directly and parse the csv bytes, instead of physically downloading files moving them.*

## Overview

Although the API doesn't work, you can still manually download CSV files containing historical price data for a given ticker. To do so, you go to [finance.yahoo.com](https://finance.yahoo.com/), and enter a ticker in the search. I entered 'AAPL'. 

 After navigating to the `Historical Data` tab, you will end up with a screen like this:

<center>
<img src="{{ site.imageurl }}yfinance_screenshot.png" style="width:800px;"/>
</center>

After editing the time period and applying it, you can click the `Download Data` button (which I emphasised in red above), and a CSV file will promptly 
start downloading on your browser. But to manually do this for 3000 stocks, as I would have to do for my MachineLearningStocks project, would be tantamount to water torture. I thus needed a good way 
of automating this process, and luckily I found one. 


## The method

The `Download Data` button is actually a hyperlink, so I decided to have a quick check as to the url of this link. If you right click a link in chrome, there is an option to 'Copy Link Address'. Doing so for AAPL with a time period of Jan 01 2001 – Aug 06 2017, the link is:

```
https://query1.finance.yahoo.com/v7/finance/download/AAPL?
period1=946656000&period2=1501948800
&interval=1d&events=history&crumb=BkT/GAawAXc
```

Actually, the structure of this url is all that one needs to crack the problem of automation. I don't know what time system yahoo finance uses internally, but it's a pretty fair guess that 946656000 corresponds to Jan 01 2001, and likewise that 1501948800 corresponds to Aug 06 2017. 

I tried just changing 'AAPL' to 'GOOG' in the above url and entering it onto chrome, and suddenly `GOOG.csv` had downloaded onto my machine. I did some digging as to what the 'crumb' is, and to my knowledge it is some sort of API token / cookie which will most likely be different for you. 

In any case, I figured that I had basically solved the issue at this point, so the rest was a matter of coding it up in python.

## Implementing the solution in python

To interact with my browser (Google Chrome), I used the `webbrowser` library in python. It is extremely intuitive to use: opening a URl is as simple as

```python
webbrowser.open('https://www.google.com')
```

I wanted to download a list of tickers that I had saved in a text file called `russell3000.txt`. To read this into a python list, we can use an elegant list comprehension:

```python
russell_tickers = [line.split('\n')[0].lower() 
                   for line in open('russell3000.txt')]
```

For each ticker, we will want to open the URL as specified earlier. The time period will be the same in all of them – Jan 01 2001 to Aug 06 2017. The code for this is as follows:

```python
import webbrowser 

for ticker in tickers:
    link = ("https://query1.finance.yahoo.com/v7/finance/download"
            "/{}?period1=946656000&period2=1501948800&interval=1d"
            "&events=history&crumb=BkT/GAawAXc".format(ticker))
    webbrowser.open(link)
```

And that's it. This will download all of the CSV files straight to your downloads folder. 

Or will it? I have found the hard way that web-related code often runs into errors regarding the web queries – often, pages that you expect to exist have been taken down or corrupted etc. When webbrowser tries to open this URL, it will encounter an error, but instead of continuing with the rest of the tickers, it'll just give up. The way around this is to use python's exception handling:

```python
for ticker in tickers:
    try:
        link = ("https://query1.finance.yahoo.com/v7/finance/"
                "download/{}?period1=946656000&"
                "period2=1501948800&interval=1d&events=history"
                "&crumb=BkT/GAawAXc".format(ticker))
        webbrowser.open(link)
    except Exception as e:
        print(ticker, str(e))
```

This way, if there is ever an error, the code will just print the ticker and the error, then carry on. In principle, we are finished. However, it is a bit annoying that all of these CSVs are in the downloads folder, rather than the directory for my project. I wrote a few lines of python to fix this:

```python
import os

# Change these as appropriate
project_path = "/Users/Robert/Project/"
download_path = '/Users/Robert/Downloads/'

# Look at the contents of your download folder
for item in os.listdir(download_path):
        # If the file is a csv, move it to the project folder
        if item[-4:] == '.csv':
            print(download_path + item)
            print(project_path + item)
            os.rename(download_path + item, project_path + item)
```

For AAPL.csv, the code would print:

```
/Users/Robert/Downloads/AAPL.csv
/Users/Robert/Project/AAPL.csv
```

and the file would be moved from the former location to the latter. Note that it will also move any other CSVs, but I didn't need to fix this as I don't normally have CSVs in my downloads. 

## Conclusion

It is always a nice feeling when you can use some python to save you hours of manual labour, and this was certainly the case here. Using this code, one can automate the downloading of a large number of datafiles which contain historical stock prices. This was a very important aspect of my MachineLearningStocks project, as it is meaningless to try to learn from data without having the data. 

As is often the case, the code I've showed here just scratches the surface. You will find that some of the CSVs didn't actually download, or they are empty. Thus, to increase the quality of your training data, some troubleshooting is in order. Some of the improvements I've made:

- troubleshoot non-alphabet characters in the ticker names
- use [Quandl](https://www.quandl.com), and its python API, to download data that Yahoo Finance doesn't have
- use [pandas-datareader](https://pandas-datareader.readthedocs.io) to download data from Google Finance, if both Yahoo and Quandl don't have the data









