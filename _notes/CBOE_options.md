---
layout: default
title: CBOE Options Courses
---

# CBOE Options courses

The [CBOE courses](http://www.cboe.com/education/online-courses) are frequently cited as a great free resource for people interested in understanding the practicalities of option trading. 

In these notes, I have frequently borrowed images from [Daniels Trading](https://www.danielstrading.com/education/), which has succinct overviews of various strategies (including example scenarios and calculations).

## Language of options

-   An **option class** is the set of all options on a security, usually represented by an options chain. 
    -   within a class, the _type_ can be a put or call
    -   a **series** specifies the options with a given strike and expiry, e.g SPY Apr 300 calls.
-   Every stock is assigned one of three expiration cycles:
    -   Jan cycle: Jan, Apr, Jul, Oct
    -   Feb cycle: Feb, May, Aug, Nov
    -   Mar cycle: Mar, Jun, Sep, Dec
-   All optionable stocks have at least 4 contracts:
    -   the next two calendar months (forward months)
    -   the next 2 months in the cycle
    -   some stocks have two additional LEAPS expiring on the next 2 Januaries
-   If you sell an option and get assigned, you have 2 days to settle. Early assignment is a risk for any short American option. 
-   All equity options are American, most index options are European.

## Introduction to option pricing

-   Options are conceptually very similar to insurance:
    -   puts can protect long positions in the underlying
    -   calls can be thought of as "insurance" on cash positions, making sure that they participate in a price rise
    -   insurance contract are priced based on the value of the underlying, the deductible (i.e strike), the length of the policy, and the risk.
-   Higher interest rates mean higher call prices: the option is more valuable because you are happy to sit in cash and collect money. The opposite is true for puts. 
-   Dividends affect option prices in a manner opposite to that of interest rates.
-   Even when the stock price is equal to the strike price, the call value will be higher than the put. This asymmetry is due to the interest component in the call (you can collect interest on your cash) which is not in the put. 
-   Time decay (**theta**) is nonlinear – it accelerates towards option expiry.
-   **Delta** is the amount by which the value of an option changes for a \\$1 change in the price of the underlying. 
    -   delta is a dynamic quantity and depends on strike. 
    -   OTM calls have deltas between 0.0-0.5, ATM 0.50, and ITM 0.5-1.0.

### Implied volatility (IV)

-   IV is the value of the volatility which results in the option value being equal to the current market price.

## Index options

-   A stock index is a statistic that measures the value of a group of stocks, based on some formula.
-   Indices are typically either cap-weighted (e.g SPX) or price-weighted (e.g DIJA).
-   Index options tend to be European, so would trade slightly lower than an American option because they have less optionality. However, both are automatically exercised on settlement day.
-   Index options are cash-settled, with a \\$100 multiplier over the index value. The settlement value depends on the style: American options are PM settled, using the closing value of the index on the day of exercise, while European options are AM settled, using the Friday opening price.
-   On settlement, the exerciser receives a cash payment (which may or may not be greater than what they paid for the option).
-   Because indices often have large price levels, options sometimes use a different underlying index. e.g the DJX is 1/100 of the DIJA.
-   Index expiries typically follow the March cycle  

-   IV for index options typically moves faster compared to IV for equity options. 
    -   it is important to understand historic ranges for IV, which can be summarised in a statistic like the IV rank
    -   IV risk can be reduced using options spreads

### Basic strategies with index options

-   Bullish but afraid of near-term downside: **index calls and money market fund**.
    -   want to buy an index fund but hedge against near term loss
    -   buy calls on a dollar-for-dollar basis (based on the strikes) and deposit our principal into a money market fund. 
    -   if the index appreciates, our call pays off and the result is that we have the same amount of exposure as if we had waited. If the index falls, we get to buy the index at the cheaper price, having only lost the premium.
-   Seeks upside participation with low capital: **index calls and save**.
    -   buy a long term index call to control a large exposure with a small premium (useful if index has a prohibitively high price)
    -   save your income in the mean while; at the end of the period, you will have enough to buy the underlying

## Option strategies

-   Owning a stock, want to take profit if price rises or buy more if prices fall: **covered straddle**
    -   consists of long stock, cash, and a short straddle. Calls and puts in the straddle 1-for-1 with the long stock. 
    -   short calls are covered by the underlying; short put are covered by the cash.
    -   if the stock price falls/rises, the put/call is assigned
    -   in the mean time, you collect the premium.
-   Want to break even on a paper loss without committing additional capital: **stock repair**
    -   long 1 ATM call, short 2 OTM calls with strike at the breakeven (a **call ratio spread**)
    -   this position appreciates more rapidly with respect to an increasing stock price than just the long stock, but gains are capped at the breakeven

<center>
<img src="{{ site.imageurl }}note_img/cboe/stock_repair.png" style="width:50%;"/>
</center>

-   Aggressive bull: **ITM LEAPS**
    -   rather than buying shares on margin, you can buy ITM LEAPS
    -   these have a lower up-front cost, a substantially lower max risk (no margin calls), but a slightly lower delta.
    -   with LEAPS, you can't vote nor receive dividends, but these are rarely relevant for the aggressive bull.

### Vertical spreads

-   Spreads are positions that involve one long and one short option. For vertical spreads, these options have the same expiry.
-   Spreads reach their maximum theoretical value only very close to expiration. The higher the volatility, the more pronounced this effect is.
-   By having one leg OTM and the other ITM (by the same amounts), we can largely hedge out theta.
-   A **bull call spread** (debit) consists of a long call at a low strike and a short call at a high strike.
    -   max profit is the difference between the strike prices, less the debit paid
    -   max loss is the debit paid
    -   breakeven at the low strike + debit paid
    -   sacrifices unlimited upside for a lower breakeven point
    -   can be used as a neutral-to-bullish strategy by setting the short strike at the current price.
-   A **bear put spread** (debit) consists of a long put at a high strike and a short put at a low strike.
    -   breakeven at high strike less debit paid
    -   same max profit/loss as bull call spreads
-   A **bear call spread** (credit) consists of a short call at a low strike and a long call at a high strike 
    -   this can be seen as protection on a short call position 
    -   max profit is the credit received
    -   max loss is the difference in strikes less the credit received
    -   breakeven is the low strike + credit received
-   A **bull put spread** (credit) consists of a short put at a higher strike and a long put at a lower strike

    -   breakeven at high strike less credit received
    -   same max profit/loss as bear call spread

-   Choosing between a credit and a debit spread:
    -   it may seem that credit spreads should be preferred because they give you a credit 
    -   however, they also carry the risk of early assignment which "brings forward" the loss. 
-   How to choose strikes:

    -   degree of conviction -- The long call/put with the higher/lower strike will be more bullish/bearish, e.g 50/55 bull call spread is more bearish than 40/45. 
    -   strike price intervals -- the wider the strike interval, the more convicted
    -   set the short option equal to the target price

-   Most brokers support direct spread orders. The alternative is to leg into the trades, establishing the long option position first then later writing the other leg.

### Backspreads and ratio spreads

-   **Backspreads** are variants of vertical credit spreads that involve buying more of the high strike calls, or low strike puts (i.e buying more of the "hedge").
-   **Ratio spreads** (a.k.a _frontspreads_) are variants of vertical debit spreads, e.g a call ratio spread is a bull call spread with more short calls than long while a put ratio spread is a bear put spread with more short put spreads. Ratio spreads include one more more naked short options by definition.
-   Backspreads and ratio spreads are referred to in terms of the smallest ratios, e.g a 1-2 bull call spread.
-   A **call backspread** can be used to express a mostly bullish long-volatility view. 
    -   it has a higher breakeven and lower profit potential than a long call, but will profit if the underlying falls significantly.
    -   maximum loss occurs if the stock closes at the high strike

<center>
<img src="{{ site.imageurl }}note_img/cboe/call_backspread.png" style="width:50%;"/>
</center>

-   A **put backspread** can be used to express a mostly bearish long-volatility view:
    -   unlike call backspreads, they have a limited upside potential because the stock can only go to zero
    -   this strategy should be compared with a long straddle. The choice will depend on the relative strengths of the bearish view and the long-vol view.

<center>
<img src="{{ site.imageurl }}note_img/cboe/put_backspread.png" style="width:50%;"/>
</center>

-   Ratio spreads can be used to express mostly neutral views (with some directionality).
-   A **ratio call spread** has unlimited loss on the upside, a small profit if there is a large decline, and maximal profit if the stock stays where it is (at the short call strike).

<center>
<img src="{{ site.imageurl }}note_img/cboe/call_frontspread.png" style="width:50%;"/>
</center>

-   A **ratio put spread** has a similar profile to the ratio call spread, but downside is limited.

<center>
<img src="{{ site.imageurl }}note_img/cboe/put_frontspread.png" style="width:50%;"/>
</center>

### Time spreads (a.k.a calendar spreads)

-   Because time decay accelerates towards expiration, this is a time at which many traders may want to be short the option (capturing theta). However, writing options carries substantial risk. **Time spreads**, which involve buying a long-term option and selling a short-term option (same strikes), are designed capture theta while having limited risk.
-   For a time spread to be profitable, the underlying should remain in a narrow range within the life of the short-term option.
-   Establishing a time spread requires a debit, which is the maximum loss of the position.
-   The value (at the expiry of the ST option) is depicted below:
    -   curved because there is still time until the LT option expires
    -   asymmetric because ITM calls have greater theta than OTM calls. 
-   If the difference in expiries is long, we have the opportunity to roll the short-term expirations, increasing the overall credit. 
-   ATM time spreads are neutral, but OTM time spreads can be used to create directional positions. 
    -   the spread should be established at the target price, with the ST expiry at the expected tenor
    -   at ST expiry, we can decide if we want to hold the long option or liquidate, though the long option will experience rapid theta decay.
    -   this trade is exposed to changes in IV.
