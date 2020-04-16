---
layout: post
title: Investigating publicly-disclosed short positions in UK equities
category: quant-finance
---

Hedge funds in the UK are legally obligated to make a disclosure to the Financial Conduct Authority (FCA) whenever their aggregate short position in a given company reaches 0.5% of the issued equity capital. In this post, we investigate a publicly available dataset containing information about large institutional short positions in UK equities and attempt to understand the value that this data contains.
<!--more-->

Usually when I write posts like this, I explain the code as I go along. However, some people have suggested that this interrupts the flow of the article for readers who are more interested in the actual outcomes of the analysis rather than the process. To that end, I have put all of the code (with brief explanaations) in an accompanying Jupyter notebook; this post will then present and discuss the results.

## Short-selling

A short-sale is a relatively simple transaction, wherein you borrow shares of a company, immediately sell them, and hope that their price drops so that you can buy them back at a cheaper price, return them to a lender, and pocket the difference. Rather than buying low and selling high, shorting is equivalent to selling high and buying low. This simplicity notwithstanding, shorting is often associated with more experienced investors. One reason for this may be that short selling has a theoretically unlimited downside – if you shorted Tesla at $300 a share and covered at $800, you would be out of pocket by $500, whereas if you bought at $300 and it went to zero, you would only be out of pocket by the $300 you put in.

Short-selling is often demonised in the media, construed to be one of the many evil things that hedge funds get up to at the cost of greater society. I'm not quite why people feel this way. There are many good reasons why you may want to bet against a company -- for example, the famous short seller Jim Chanos aggressively shorted Enron in the lead up to its collapse in 2001. It is also not possible for short-selling in and of itself to cause a company's equity value to collapse, because of the uptick rule (a stock can only be shorted after it ticks up). I am of the belief that short-selling is an important market mechanism which may help too counteract the "irrational exuberance" that often plagues markets; it is fundamentally no better or worse morally than buying a stock. In both cases, an allocation of capital is being made based on your views of future performance. 

## The significance of the FCA Dataset

As stated in the introduction, the UK's equivalent of the SEC, the Financial Conduct Authority (FCA) mandates that companies disclose short positions exceeding 0.5% of a company's float. 

The reason why this is such a fascinating resource is that hedge funds are notoriously secretive about their positions. Disclosing positions (long or short) may allow competitors to cotton-on to the fund's strategy (alpha decay). In the case of short-selling there is an added dimension: disclosure leaves a fund susceptible to targeted short squeezes, in which other market participants start to buy up a stock, driving up the price and sometimes forcing short-sellers to cover their positions at a loss (further driving up the price).

Nevertheless, because of the FCA, we can get a rare glimpse into the holdings of the smartest players in the industry. Actually, it doesn't stop there. The FCA additionally requires that once companies are above the 0.5% mark, they must disclose any changes in their position of more than 0.1%. Hence we can see how an institution is modifying their position in response to external factors.


## Investigating the dataset 

<center>
<img src="{{ site.imageurl }}short_selling/short_dataset_overview.png" style="width:80%;"/>
</center> 


One initial hypothesis might be that companies with a lot of institutional short interest (i.e many smart players are shorting the stock) should be more likely to experience a decline in the share-price. 







## Extensions 

Some refinements:

- These short positions are not necessarily indicative of a direct view – it could be a hedge, or part of a long/short basket strategy. 



## Conclusion 

In conclusion, we see that the disclosure data is almost certainly *not* a basis for a simple copycat strategy. In fact, we have observed the contrary. Rather than performing poorly, stocks that have been heavily shorted appear to appreciate on average. It is true that we have neglected the performance of the benchmark, but the x% mean outperformance [FIX] is not neglible.

However, I do think that this kind of analysis may provide a useful datapoint for discretionary equity portfolio managers, because it shines light onto the landscape of marginal buyers/sellers. For example, if you see that a certain fund holds a large short position in a small company, you know that at some point in future if the company wants to close their short there will be significant demand for the shares. 
 

Could be interesting to pair this with 13F data 
