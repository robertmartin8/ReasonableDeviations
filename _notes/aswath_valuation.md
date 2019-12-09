---
layout: default
title: Valuation
---

# Valuation – Aswath Damodaran

This course can be found in full on [youtube](https://www.youtube.com/playlist?list=PLUkh9m2BorqnKWu0g5ZUps_CbQ-JGtbI9). It consists of 25 videos, each around 15 minutes in length. 

## Contents

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Contents](#contents)
- [Introduction](#introduction)
- [Intrinsic Valuation](#intrinsic-valuation)
	- [The risk-free rate and discounting](#the-risk-free-rate-and-discounting)
	- [Equity risk premia](#equity-risk-premia)
	- [Relative risk measures](#relative-risk-measures)
	- [Cost of debt and capital](#cost-of-debt-and-capital)
	- [Estimating cash flows](#estimating-cash-flows)
	- [Estimating growth](#estimating-growth)
	- [Terminal values](#terminal-values)
	- [Value enhancement](#value-enhancement)
	- [The value of equity](#the-value-of-equity)
	- [Synergy, control, and complexity](#synergy-control-and-complexity)
	- [Distress, dilution, and illiquidity](#distress-dilution-and-illiquidity)
- [Relative Valuation](#relative-valuation)
	- [PE Ratios](#pe-ratios)
	- [EV/EBITDA](#evebitda)
	- [Price/book ratio](#pricebook-ratio)
	- [Revenue multiples](#revenue-multiples)
- [Option-based valuation](#option-based-valuation)
	- [The option to delay](#the-option-to-delay)
	- [The option to expand and abandon](#the-option-to-expand-and-abandon)
	- [Distressed equity](#distressed-equity)
- [Applications](#applications)
	- [Asset-based valuation](#asset-based-valuation)
	- [Valuation of private companies](#valuation-of-private-companies)

<!-- /TOC -->


## Introduction

Fundamental propositions:

- Valuation is simple, we choose to make it complex.
- A good valuation has a story behind it
- Valuations go wrong due to bias, uncertainty and complexity.

There are three main approaches to valuation:

- **Intrinsic valuation**  values an asset as the present value (PV) of expected cashflows. **Discounted Cash Flow** (DCF) valuation is one of the main techniques used, and requires the cash flows, a discount rate and a life span. Intrinsic valuation needs a long time horizon to be profitable.
- **Relative valuation** looks at similar assets (using multiples like P/B, EV/EBITDA) and controls for differences. This assumes that the market prices assets correctly on average.
- Option pricing in the context of valuation can be used when assets have option-like characteristics, e.g a contingent payoff.


## Intrinsic Valuation

- DCF is the main tool for performing intrinsic valuation.
- Intrinsic valuation should only be used for assets that generate a cash flow.
- The most common way to do a DCF valuation is to use the expected cash flows then adjust for risk in the discount rate:

$$\text{value} = \frac{E(C_1)}{1+r} + \frac{E(C_2)}{(1+r)^2} + \ldots + \frac{E(C_n)}{(1+r)^n}$$

- Alternatively, we can convert each cashflow into its **certainty equivalent**, then the discount rate is the risk-free rate.
- For an asset to have value, there must be positive cash flows at some time in the future.
- Assets with early cash flows will generally be worth more
- Assets can be divided into **current assets** and **growth assets**.
- Liabilities are either **debt** or **equity** – these are the only ways in which a firm can raise money.
- One can either value the whole business or conduct an **equity valuation**, which only considers the cash flows an equity investor would receive – in which case the discount rate is the rate of return an investor requires
    - the **dividend discount model** is a special case of equity valuation, but does not account for the fact that companies do not pay as much as they can.
    - the discount rate that equity investors demand is the **cost of equity**
    - the weighted average of the cost of equity and the cost of debt is the **cost of capital** which can be used to discount cash flows to the firm to value the whole business
    - thus equity can be valued by valuing the whole business and subtracting debt. This should give the same result as doing a DCF of equity cashflows.
    - never mix cash flows with the wrong discount rate.

### The risk-free rate and discounting

- Valuation can either be in real or nominal terms (whether cash flows are adjusted for inflation). The discount rate will be different in each case.
- The cost of equity depends on risk and risk premia
    - risk is often estimated with statistical measures using stock price data. 
    - We must consider the risk to a **marginal investor**, who sets the stock prices by actively trading large holdings.
    - most models assume that the marginal investor is diversified, in which case the risk of an asset is the risk it adds to a diversified portfolio.
- The CAPM considers a *market risk premium*, using a beta relative to the market.
- The Arbitrage Pricing Model (APM) and multifactor models allow for multiple sources of market risk. Typically, multifactor models consider macroeconomic factors while the APM looks at statistical features.
- All of these risk/return models require the **risk-free rate**.
- A risk-free asset has no variance around the expected return: there is no default risk and no reinvestment risk. 
    - the time horizon matters
    - $R_f$ depends on the currency
- Although it may make most sense to use the longest issue T-bond, it is often sensible to use the 10y issue because other kinds of data may not be available for the 30y.
- To find $R_f$ for different currencies, you can use that government's 10y bonds, but you must adjust for default risk, which can be done with a **default spread**. This can be found by looking at the CDS markets or by looking at the country's debt rating.

### Equity risk premia

- The **equity risk premium** (ERP) is the excess return an average investor requires to invest in equity instead of a risk-free asset.
- It should change over time because macroeconomic risk varies.
- The **historical risk premium** is the excess return of stocks over risk-free assets historically:
    - sensitive to the time window and issue of the risk-free asset
    - tends to be a noisy estimate with high std
    - suffers from the survivorship bias
- A forward-looking risk premium can be computed as follows:
    - treat SPY as a fixed income security where the cashflows are dividends and buybacks
    - future dividends can be estimated by growing the previous year's value at the analyst estimates of SPY growth
    - eventually, set earnings growth equal to the risk-free rate to compute a terminal value
    - the internal rate of return less the risk-free rate is the **implied risk premium**
- To estimate a **country risk premium** for an emerging market, scale that country's default spread by the relative volatility of their index to the volatility of their "risk-free asset", then add it to the the US risk premium. 
- Different approaches to finding the corporate equity risk premia are:
    - $E(R) = R_f + CRP + \beta(US ~ERP)$, i.e eery company is equally exposed to country risk
    - $E(R) = R_f + \beta(US ~ERP + CRP)$
    - $E(R) = R_f + \beta(US ~ERP) + \lambda (CRP)$, i.e treat country risk as a separate factor. $\lambda$ can be estimated by looking at how much of the revenue comes from that country.

### Relative risk measures

- The ERP gives the excess return for an average risky stock, but it is necessary to know the premium for a specific stock.
- There are multiply ways of doing this:
    - MPT betas using regression analysis
    - Accounting risk: volatility of earnings
    - Intrinsic risk: composite measures and qualitative analysis
    - Price-based: implied beta
- A regression beta has a large standard error, and is highly dependent on the exact setup of the regression.
- A **bottom-up beta** can be computed by:
    - making a list of all the businesses the firm operates in
    - averaging the regression betas of other firms in those businesses, then delevering using the average debt/equity of the business.
    - Find the revenue-weighted average of the unlevered betas.
    - re-lever using the firm's debt/equity.

### Cost of debt and capital

- Debt must satisfy 3 criteria:
    - contractual commitments
    - tax deductible
    - failure to meet the commitment can cost you control of the business.
- This includes interest-paying loans, bonds, leases.
- The **cost of debt** is the rate at which you can borrow long term, reflecting your default risk and the prevalent interest rates.
- Cost of debt can be estimated by using the yield to maturity on a long term bond issued by that company, or estimating the default spread based on the credit rating.
    - if there is no official rating, we can generate a synthetic rating based on the **interest coverage ratio** (EBIT / interest expenses), which can then be put into a lookup table.
    - if the company is in a risky market, the country's default spread (or a portion of it) must be added:

$$\text{cost of debt} = R_f + k \times (\text{country default spread}) + \text{company default spread}$$

- Once we have the cost of debt, we must combine it with the cost of equity to get the **cost of capital**
- The weights should be based on market values, because this is what it would cost you to buy the company regardless of the true value.
- There is a tax benefit on debt, taxed at the **marginal tax rate**.
- The market value of debt can be estimated using the cost of debt and the average maturity, treating all debt as one fixed-income security.
- Hybrids (e.g convertibles) can be split into equity/debt.
- Preferred stock should be kept separate.

### Estimating cash flows

- We are trying to estimate the **free cash flow to the firm** (FCFF), from which we can find the cash flows to equity. The steps are:
    - estimate operating earnings
    - estimate reinvestment
    - consider debt
- Trailing twelve month (TTM) earnings should be used instead of the last annual reports.
- Earnings should then be normalised for current market conditions.
- Operating earnings should be cleansed of some items:
    - leases should go under debt, which can be done by calculating the PV of the lease obligations
    - though treating leases as cap ex will lower the cost of capital and increase operating income, it can decrease the ROIC because the lease becomes an asset.
    - R&D should be moved, but this adds complexity because it has to be capitalised for previous years, then amortised/appreciated
        - find the average amortisable life span, e.g 5 years.
        - calculate expenses for each amortisable years
        - write off R&D
- For the first few years, you can use the effective tax rate, but in the end it should be the marginal rate.
- Reinvestments should be defined broadly, including R&D and acquisitions.
- FCFF -> FCFE involves subtracting debt repayments.

### Estimating growth

- Historical growth rates are sensitive to the computation methods, and may not be a good predictor of future growth.
- Management estimates are biased; analyst estimates are too focused on short term EPS.
- Earnings growth is a function of reinvestment and its quality. Metrics for these quantities differ on whether they are being measured for equity or for the firm:
    - quantity for equity: **retention ratio** (1 - dividends/net income).
    - quantity for the firm: reinvestment rate
    - quality for equity: **ROE**, net income / book val of equity
    - quality for the firm: **ROIC**, EBIT(1-t)/book val of capital
- ROE and ROIC are highly dependent on accounting decisions. They should be adjusted for things like goodwill.
- Firms can also grow by improving ROIC without reinvestment. This is **efficiency growth**, and is a valid growth component but one that is transitory.
- If net income is negative, you must look at revenue growth and operating margins. The **sales to capital** ratio quantifies the reinvestment needed to increase revenue; the industry average can be used here.

### Terminal values

There needs to be some closure for cash flows beyond a certain time. There are three approaches

1. Consider liquidation value
2. Estimate the **stable growth rate** then sum the infinite series
3. Use a multiple like P/E. This *should not* be used. 

Most often, the stable growth method is best. The rules for stable growth are as follows:

- Stable growth cannot exceed the growth rate of the economy, i.e it is capped at the risk-free rate.
- Don't wait too long to reach stable growth
- For most companies, ROC should equal the cost of capital unless there is a lasting competitive advantage.
- Beta tends to one in perpetuity. 

### Value enhancement

1. Increase cash flows from existing assets
2. Increase growth rate or enhance the value of growth
3. Lower discount rate by changing debt/equity mix
4. Delay the terminal value by building a competitive advantage. 

Research has shown that:

- The best way to increase value is to release a new product or expand a market
- Acquisition is the worst way because of the premium to intrinsic value. 

### The value of equity

There are some additional value factors that should be considered

- Cash and marketable securities:
    - cash can trade at a discount or premium based on the company, depending on how it has deployed cash
    - cash in emerging markets should trade at a premium
- Cross holdings:
    - hard to value because of different accounting.
        - For minority passive holdings, only dividend is shown. 
        - For minority active holdings, equity income is shown
        - For majority active holdings, financial statements are consolidated.
    - we can use the market value of the holdings as a compromise
    - alternatively, we can use the P/B ratio
- Remaining assets, e.g un-utilised assets.
- Remaining debt, e.g the expected cost of lawsuits

### Synergy, control, and complexity

- Synergy is often added as a premium to reflect that two firms together may have more value than the sum.
    - **Operating synergy**:
        - strategic advantages of economies of scale
        - cost savings
        - new investments
    - **Financial synergy**:
        - tax benefits (e.g higher depreciation)
        - more borrowing power
        - diversification if they are private
    - Calculating the value of synergy requires 3 valuations: acquirer only, target only, then acquirer + target together.
    - It often takes time for synergy to materialise.

- The value of control depends on the acquirer thinking that they can add value with their management.
    - to include this in valuation, revalue the company with slightly better numbers (corresponding to better management), then multiply by the probability of new management taking over
    - this method can be used to value voting vs non-voting shares

- Complexity is a negative value factor, all things held equal.
    - a simple proxy measure is the length of the 10-K
    - then you can add a discount to the intrinsic value.

### Distress, dilution, and illiquidity

- DCF is not good for truncation risk (i.e bankruptcy). We can look at the bond market for an indication of the likelihood of bankruptcy.
- The value of options should be subtracted to account for dilution, and future options issuance should be an operating expense.
- You can apply an illiquidity discount based on the expected cost of transacting in the asset.

## Relative Valuation

- Assets are valued by looking at similar assets. This requires:
    - finding comparable assets
    - standardising prices (e.g using a multiple)
    - controlling for differences
- Popular because:
    - much easier to sell, and you can choose a story
    - assumptions are implicit – harder to question an analysis
    - if you're wrong, other people are too
- Multiples tend to be some measure of what you are paying divided by what you are getting. The numerator can be market cap, enterprise value (EV), while the denominator tends to be revenue, earnings, cash, or book value. 

**1. Define the multiple**

- The numerator and denominator must talk about the same thing, i.e either the equity or the firm. You can't have price/EBITDA.
- When comparing companies, the multiple must be defined exactly the same way.

**2. Describe the multiple with a distribution**

- The distributions for multiples tend to be skewed to the right, because they are truncated on the left – the median is a better measure of central tendency than the mean.
- May make sense to cap the multiples
- Stats will change over time

**3. Analyse**

How do changes in a firm's fundamentals affect the multiple?

<center>
<img src="{{ site.imageurl }}relative_valuation.png" style="width:100%;"/>
<figcaption>Driving variables for multiples</figcaption>
</center>

**4. Apply**

- A comparable firm has similar cash flows, growth, and risk. It does not have to be in the same sector.
- It is important to control for differences

### PE Ratios

- Earnings (EPS) comes in many variants:
    - time variants: current, trailing, forward
    - diluted, partially diluted, no dilution
    - before/after extraordinary items
- The median PE will be much less than the mean because of asymmetry.
- PE is only defined for EPS > 0, eliminating young growth companies.
- PE distributions are largely similar in different markets.
- Relationship between PE and fundamentals (ceteris parabus)
    - higher growth = higher PE
    - higher risk = lower PE
    - higher ROE = higher PE
- When screening for low PE stocks, look for:
    - high expected growth
    - low risk
    - high ROE

### EV/EBITDA

- A better multiple would be EV/FCFF, but this is harder to calculate.
- EV is the sum of the market value of equity and the market value of debt less the cash.
    - cash is subtracted out for consistency, because it is not part of EBITDA
    - this means that the market value of cross holdings should also be removed
- There are many rules of thumb, e.g EV/EBITDA < 6 is good, but it is important to consider the distribution.
- Relationship between EV/EBITDA and fundamentals
    - higher tax = lower EV/EBITDA
    - higher growth = higher EV/EBITDA
    - higher risk = lower EV/EBITDA
    - higher reinvestment requirements = lower EV/EBITDA

### Price/book ratio

- PB = market val of equity / book val of equity
- PB can be extended to add debt or subtract cash, but it must be consistent on the numerator and denominator
- Using a dividend discount model, we see that 

$$P/B = \frac{\text{ROE} \times \text{payout ratio}}{1-g}$$

- ROE is the driving variable: higher ROE = higher PB.
- Growth amplifies the effect of ROE: higher growth = higher PB.
- Relative valuation with PB is a good filter prior to intrinsic valuations.

### Revenue multiples

- Not as dependent on accounting practices as earnings multiples
- Price/sales is actually inconsistent, because the numerator is the market value of equity while sales is for the firm. Thus is acceptable when PS is used for firms with little debt.
- PS varies a lot by sector, so we should not rely on rules of thumb. 
- PS is highly dependent on profit margins:
    - higher margin = higher PS
    - lower margin = lower ROE = lower growth
- EV/sales
    - after-tax operating margin is now the companion variable.
    - higher margin = higher EV/sales
- Buying companies with high revenue multiples is a bet that margins will remain high
- Brand names can be valued by re-valuing a company with a lower margin (use the margin of a competitor with no brand value). The difference between this new valuation and the original valuation is the value of the brand.

## Option-based valuation

- DCF will underestimate value if assets have option-like characteristics. Options entail learning and adaptive behaviour, increasing expected value. 
- To identify a real option, there must be:
    - an underlying asset
    - contingent payoff
    - limited life
- For an option to have significant economic value, there should be exclusivity
- The assumptions of the options-pricing models should be checked before applying them. For example, assets and their options need to be tradable, and most assume borrowing/lending at the risk-free rate.
- The Black-Scholes model may not be appropriate because of assumptions of 'continuity', constant variance, and exercise date. But the Binomial model adds unnecessary complexity.
- After checking the assumptions, we need to map the scenario onto the option pricing model.

### The option to delay

- Just because a project has a negative NPV today doesn't mean that the rights to the project aren't valuable.
- Patents are an example, they will only be commercialised if it becomes economically viable to do so:
    - value of underlying = PV of inflows from taking the project now
    - variance of underlying = variance of cash flows of similar assets
    - exercise price = cost of investing into the project
    - expiration = life of the patent
    - dividend yield = cost of delay
- For natural resource options:
    - value of underlying = expert estimates of quantity multiplied by price
    - variance of underlying = volatility of resource price
    - exercise price = costs of investment in historically similar wells/mines
    - expiration = relinquishment period or time to exhaust the current inventory
    - dividend yield = cost of delay, i.e production revenue

### The option to expand and abandon

- Sometimes a bad investment (NPV < 0) can be viable if it helps you expand into a new market.
- Considerations:
    - do the subsequent investments have to be tied to the first? Can someone else do the market testing?
    - how much exclusivity will you have?
    - what are the competitive advantages in expansion? What stops someone else from expanding after you have 'paid for' the market research?
- The **option to abandon** is like a put option: it adds a lower bound to your losses:
    - cost flexibility and the number of long-term contracts both affect the value of such an option
    - companies that build in options to abandon should be more valuable for investors

### Distressed equity

- Distressed equity is similar to an option: because of limited liability, if a firm can't pay off debt, the most you will lose is your initial investment (much like the price of an option):
    - value of underlying = market value of assets
    - variance of underlying = weighted volatilities of stocks and bonds
    - exercise price = consolidate all debt into one fixed-income security and value it
    - expiration = weighted maturity of debt

## Applications

### Asset-based valuation

- Valuing a business based on the assets on the balance sheet, rather than cash flows. May be required for:
    - liquidation
    - "fair value" accounting
    - acquisition considerations (sum of the parts valuation)
- Valuing each asset requires either intrinsic, relative, or accounting value.
- This is easiest to do when assets are separable and cash flows are independent. There also needs to be a market to sell the assets.
- Liquidation invites relative valuation because it matters what the assets would sell for in the market. There may have to be a **liquidation discount** depending on urgency.
- Accounting has yet to decide whether "fair value" is intrinsic or relative.
- Sum of the parts can be as simple as valuing each asset according to the median EV/EBITDA of the business
    - a more advanced analysis can be done with regression to predict the EV/EBITDA based on other firm characteristics, rather than just assuming it will have the median EV/EBITDA.
    - the PV of expenses for the holding company should be subtracted from the sum of the parts.

### Valuation of private companies

- In private companies, there is no market value, and accounting standards will vary greatly.
- Private to private transactions are hardest to value:
    - buyers are unlikely to be diversified, so they take on both common and firm-specific risk. They are then exposed to a total beta, which can be estimated by scaling up the market beta according to how much of the total risk comes from the market. 
    - to estimate the cost of capital, we can use the industry average debt/equity ratio, remembering to lever the beta with the same value.
    - the owner may be a **key person**, so the company should be revalued with a  lower operating income to reflect the risk that the key person may leave.
    - there may need to be an illiquidity discount
- Private to public:
    - buyers more likely to be diversified, so conventional valuation can be used. Though the public company often has more bargaining power.
    - different tax considerations
    - no illiquidity discount, because the public company can sell off assets
- Private to IPO:
    - control implications
    - must consider how IPO proceeds will be allocated
    - value of options/warrants should be deducted
    - investment banks may guarantee a price, which entails a discount
- Private to VC:
    - compromise between total beta and market beta, because VCs are slightly more diversified.
    - cost of capital will be dynamic, changing as a company goes from private to VC to IPO. 