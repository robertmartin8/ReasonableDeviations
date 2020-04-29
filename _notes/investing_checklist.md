---
layout: default
title: Investing Checklist
---

# Equity Investing Checklist

This page represents a work-in-progress checklist for equity investing. It is continuously being refined based on my reading, as well as my actual experience in researching and investing. The hope is that this provides transparency and accountability (for myself), to better understand my cognitive biases and weak spots.

An important idea underlying this checklist is that time is finite, and there are thousands of public companies that we could analyse. Hence it's ok to be trigger-happy with your vetos, and to stop researching a company if there's a minor red flag. In the checklist below, I write `veto` whenever the point is a sufficient reason to stop researching the company.

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Investment universe](#investment-universe)
- [Preliminary research](#preliminary-research)
    - [Understanding the business](#understanding-the-business)
    - [Quick look at financials](#quick-look-at-financials)
    - [Meta issues](#meta-issues)
- [In-depth research](#in-depth-research)
    - [Resources](#resources)
    - [Key questions](#key-questions)
    - [Story](#story)
- [Modelling and valuation](#modelling-and-valuation)
- [Third-party research](#third-party-research)
- [Pull the trigger](#pull-the-trigger)

<!-- /TOC -->
---

## Investment universe

The first step is to narrow down the investment universe. This is a very vague process, and can be done in many different ways. The quality of these companies really doesn't matter at this point; we are just trying to reduce our universe from \~5000 companies to \~100 companies. Some methods:

- My machine learning screen
- Greenblatt/Lynch screeners
- CapIQ screens
- Random stock ideas
- Sell-side aggregated lists
- 13F filings or insider buying

## Preliminary research

In this section, we aim to understand whether a company is worth doing more research on. Throughout the process, we take brief notes of any questions we have about the company or any preliminary pros/cons, which will be revisited later if we continue researching the company.

### Understanding the business

- How does the company make money? 
  - can just use Yahoo Finance, Atom Finance, or CapIQ.
  - company wikipedia page or website.
- What business is the company in?
  - Geography?
  - Do I understand the basics of the business?
  - What is the top-level trend in the business?
- Where does the company fit into my macro outlook? (`veto` if it clearly doesn't)

### Quick look at financials

- What is the trend in the stock price?
- Quick look at financial ratios, and how it compares with competitors
  - use Atom Finance or CapIQ for comps
  - note anything surprising (e.g very low/high multiples)
- Quick look at the trends in revenue, margins, cash flows.
- Quick look at the balance sheet. (`veto` if much too complicated for me to understand)

### Meta issues

- Do I find the company/sector vaguely interesting? (`veto` if I really have no interest in it, e.g Oil and Gas, there is no point moving further)
- What edge do I have trading this company?
  - is it small enough that major analysts don't cover it? (a la Lynch)
  - do I have special sector knowledge?
  - not necessarily a veto factor because at this stage in my career there aren't a huge number of companies I have an edge on.

## In-depth research

Once I have decided that it is worth investing the time to deeply research a company, it's time to pull up the recent financial statements to try and properly understand the company. 

### Resources

- Read and make notes on the following sections from the 10K, with an emphasis on red/green flags and points that need to be revisited:
    - Introduction and company overview (we should have a rough idea of what will be in here)
    - Skim through the risk factors to see if there are any that aren't boilerplate
    - Management Discussion & Analysis
    - Look over financial statements
    - Read the footnotes (`veto` if company has many accounting complexities, e.g cross-holdings, weird leases, unusual capital structures; it is likely not worth the time given my current ability)
- Read through some recent earnings calls transcripts, in particular the Q&A section, to try and understand what the market is paying attention to.
- Read through the proxy statement to understand the compensation model.
- Read the latest news on the company and third party research
  - Very important to judge the third-party research on the thesis and not the name
  - Use the third-party research to inform data points rather than the overall story

### Key questions

Some of the key questions to answer are as follows (based on Aswath Damodaran's framework):

- **What are the key levers for the sector?** Do I have any deviant views on these? 
  - How are supply and demand changing?
  - What are the tactical (short-term) and strategic (long-term) considerations?
- **What are the key levers for this company?** Do I have any deviant views on these?
  - What non-GAAP metrics are people paying attention to?
  - What questions are people asking in the Q&A section of the earnings calls?

- How will the company's investments drive sales growth or operating margin improvement? What is the cost of this growth in terms of required reinvestment?
- What are the major risks to the company that may inhibit its sales growth or operating margin improvement?
- What is management's strategy, and how successful have they been at executing on their goal? How has the company evolved over time?
- Are management incentives aligned? What is their compensation model? Have they been buying stock?
- What non-operating assets does the company have?
- Will the company remain a going concern? (`veto` if I am not confident that the company will remain solvent)

Regarding strategy and competition:

- Revisit comps in more detail to understand competitive landscape 
- SWOT, Porter's five forces.

### Story

Having answered the questions above, it is time to summarise all of the information thus far into a story, that links the facts together. It is advisable to amass all the facts *then* try to come up with a consistent story, rather than building a story on the go (prone to confirmation bias).

`veto` if I can't come up with a simple story that explains the facts, or if the story is not attractive.

## Modelling and valuation

Valuation is quite a deep area, so the depth you go depends on how much time you are willing to dedicate to it. The bare minimum is do a relative valuation.

### Relative valuation

- Using CapIQ, Bloomberg, or a similar tool, construct a list of comparable companies. There is an important tradeoff between having good quality (i.e very similar) comps and having enough of a sample for the mean/median to be meaningful.
  - important to keep an eye on variables that are not captured by the valuation multiples, for example, leverage and cash flow
  - if you notice any outliers among the compset, investigate why, but consider removing them. 
- We can then compute the average value for each metric (e.g EV/EBITDA, P/E) across multiple values.
- If our company has multiples much lower than the median, it is important to critically think about why this is the case. Some common reasons for a low P/E are: lower growth than comps, more levered, different risk exposures (geography).
- If there is a very good reason that the company is trading at a big discount to comps, and hence you don't feel the comps are a fair sample, then you can just use multiples from the company's own history along with your own forecasts of the denominators to estimate the value.

### DCF valuation

We should already have most of the inputs ready to go, based on the prior section. The key job here is to tie the numbers to the story and to come up with some idea of the intrinsic value of the company.

- Use one of Damodaran's many [spreadsheets](http://pages.stern.nyu.edu/~adamodar/). In most cases, we should use his [model selector](http://www.stern.nyu.edu/~adamodar/pc/model.xls) and the relevant Ginzu spreadsheet.
- Conduct a sensitivity analysis

If we are having trouble with the inputs, we may need to do some work regarding market-implied variables, or inferring DCF inputs from comparable companies. Useful questions to consider: 

- How do my assumptions differ from consensus?
- Is the company trading at an *unjust* discount to peers? 
- What is the market-implied forecasting period (cf Expectations Investing)? Do I think this period of cash flow growth is sustainable?

### Margin of safety and catalysts

- Using either of the above methods (or a combination of the two), we should have a range of valuations. A useful valuation tool is the **football field** chart, which plots the valuation range from a number of sources, and compares it to the current share price. 
- It is a positive sign if there is a high degree of agreement between the different signals -- if not, we should analyse why the market is treating one metric very differently.
- It is a very positive sign if all of the metrics are significantly above the current share price -- this is a **margin of safety**, and serves to give us some leeway to account for **model risk**. Nevertheless, it is important to be realistic and really think about what we know that the market doesn't. 
- Lastly, it is critical to think about **catalysts**. If we are arguing that a stock is completely undervalued, what will make the market re-value it?
  - the most common catalyst is probably an earnings report, since many people pay attention to these, but for small-cap stocks even these might not be enough to cause a significant re-valuation.
  - when dealing with shorts, regardless of how good your valuation is, a catalyst is a necessity. Many legendary investors (Tiger Cubs) got killed in the dot-com bubble because they were shorting incredibly overvalued companies that just went on to become even more overvalued. "The market can stay irrational longer than you can stay solvent".


## Pull the trigger

At this point, if we decide that a company is worth investing in, we prepare a dated memo according to the following template:

- Brief introduction to the company (industry, current share price, current multiples)
- Investment thesis and key levers.
- How my view of the levers differs from consensus
- Risk factors (identify risks using a [pre-mortem analysis](https://en.wikipedia.org/wiki/Pre-mortem))
- How the company fits into my portfolio (in terms of diversification / sector risk)
- Price target and stop loss.

This is really for accountability and self-improvement, so that 6 months from now I can understand whether I was:

- Right for the right reasons (yay!)
- Right for the wrong reasons, i.e lucky.
- Wrong but on the right lines, e.g identified the key lever but called it the wrong way.
- Completely wrong, or black-swanned.

## After the trade

- For posterity, it is good to note down the date and price of your fill (although this is tracked by your broker).
- It is inevitable that we will continue to learn new things about the stock once we own it -- many investors say that this is the best time to learn about your company because you now have skin in the game. It is very possible that you may come across new information that invalidates your thesis, or affects your entry/exit conditions. This is fine, as long as you note it down very clearly. Hence 6 months from now, you will be able to retrospectively diagnose whether you were constructing a narrative to justify your beliefs, or the new information truly did affect your thesis.
- When we eventually close the position, again, we should write down the date and the fill. This is important so that in future we can analyse what happened to all the stocks that we sold -- did we sell too early?
