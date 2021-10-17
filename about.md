---
layout: page
title: About
order: 4
---

<center>
<img src="{{ site.imageurl }}linkedin_pic.png" style="border-radius: 50%; width: 40%"/>
</center>

My name is Robert. I recently graduated from the University of Cambridge where I studied Natural Sciences, specialising in Astrophysics.

I am interested in finding intuitive quantitative explanations for complex phenomena, even if those intuitive explanations concede a lack of determinism. It's always nice to find order in complexity, but it may very well be complex all the way down. I'm a logical person who holds rationality dear, though I'm equally interested in the limits of rationality and the need to make decisions under epistemic uncertainty, accounting for "model risk".

There was a time when I wanted to be a physicist, but "messy" systems like financial markets attract my attention nowadays. You can't understand these systems on a blackboard; you have to get your hands dirty and dive into the data. Programming is a key weapon in this endeavour.

## Get in touch

*If you have a question about PyPortfolioOpt, please [raise an issue on GitHub](https://github.com/robertmartin8/PyPortfolioOpt/issues)*

<center>
<img src="{{ site.imageurl }}contact.png" style="width:75%;"/>
</center>

<hr>
<!-- TOC -->
- [Programming](#programming)
- [Finance](#finance)
- [Crypto](#crypto)
- [Books](#books)
- [Timeline](#timeline)

<!-- /TOC -->

## Programming

Whenever I need a computer to help me, I first turn to python. I've been using python for about {{ 'now' | date: "%Y" | minus: 2014  }} years, and its initial charm has never really faded. With its pseudocode-like syntax and clear philosophy (type `import this` into a python console), it lets you get your ideas into a computer with relative ease. Of course you aren't going to get C-like speeds natively, but nowadays there are options like cython or Numba which can alleviate this to a certain extent.

R probably has a better statistics ecosystem – there is an R implementation for basically any useful theorem/procedure in statistics. But I think that its lead over python will narrow over time as more and more people re-implement R packages in python.

I didn't learn Java by choice – it is the language that Cambridge teaches first-year computer science students. It's a fantastic language to frame discussions of OOP and design patterns, but writing Java just doesn't spark joy for me. That being said, for things like mission-critical enterprise codebases, I can certainly see the appeal of Java's verbosity and 'safety'.

Julia is a language that I've become obsessed with recently. The headline pitch is that it offers python-like syntax with C-like speeds. But there are many other reasons why Julia is a joy to code in: a *wonderful* packaging system (blows `pip` out of the water), really cool metaprogramming functionality that allows for neat implementations of autograd etc, excellent integrations with other languages via e.g PyCall or RCall.

## Finance

Financial markets are interesting to me philosophically because they encode the beliefs of many different agents into tradable quantities. Practically, I haven't been able to find an area that has the same mix of programming, world affairs, mathematics, psychology, betting. If we define rationality as the goal of maintaining an accurate set of beliefs about the world and updating these beliefs in accordance with Bayes theorem, then trading on financial markets is a pure expression of rationality -- succesful betting requires calibration and courage.

I've previously worn both discretionary and systematic hats; I'm currently most intrigued by two specific areas areas:

- "Quantitative discretionary": strategies that incorporate a rigorous scientific approach into human decisionmaking. 
- Hypothesis-driven systematic: I love the idea of systematising edge, though I am much more interested in building hypotheses about the world rather than black-box optimisations.

## Crypto

I became fascinated by the crypto space in 2016 -- it has been delightful to watch it morph and grow over the past few years. I spent some time as a blockchain consultant, focussing on the strategic aspects of blockchain for several South-East Asian companies, including a crypto exchange. While in university, I worked on a blockchain startup with some friends (which you can read about [here]({% post_url 2019-09-01-what-we-learnt-enterprise-blockchain %})).

As of late my interest has shifted towards crypto markets (as opposed to blockchain technology). I'm fascinated by the opportunities that arise in a liquid but not-entirely-efficient market, for example the crypto [basis trade](https://binancepremiums.com/), as well as various aspects of DeFi.

## Books

Reading is such an important part of my life. I try not to limit my reading to specific subjects -- instead, I continuously attempt to read about subjects that are new to me, such that I "know what I don't know". This also applies to a lesser degree with fiction, though I no longer bother trying to rationalise reading fiction. I happily concede that "I just enjoy it".

Check out this [twitter thread](https://twitter.com/robertmartin88/status/1442145883047399437) for an explanation of my reading workflow. 

Below are some of my favourite books (or short stories); I suppose there is an overrepresentation of science fiction, but I can't apologise for being fascinated by it!

{% include goodreads.html %}


## Information diet 

This section details some of the newsletters and podcasts I consume on a regular basis. Excluding twitter and reddit ;)

### Podcasts 

I've split up the podcasts I consume loosely based on subject (though there's a lot of overlap). Within each section, the podcasts are in approximate descending order of how often I listen to their episodes etc.

- **Business**
  - *Masters in Business* by Barry Ritholtz -- Ritholtz manages to get top tier guests and I'm consistently impressed by the quality and pacing of the interviews. Plus, in each episode the guests recommend their favourite books/content -- super valuable.
  - *Invest Like the Best* and *Founders Field Guide* by Patrick O'Shaughnessy -- Patrick O'Shag is one of the OG podcasters. There's a tonne of truly excellent content here, though the podcast has definitely pivoted more to VC rather than public market investing.
- **Investing and finance**
  - *Flirting with Models* by Corey Hoffstein -- if you're at all interested in quant finance, this is a must-listen. Corey does an amazing job of guiding the conversation through deep technicals as well as the big-picture view. Guests are surprisingly open in discussing their approaches and techniques -- very rare in quant finance. 
  - *Macro Voices* -- good coverage on macro news and occasional interesting guests, though as I've learnt more about finance I am noticing a fair amount of Gell-Mann amnesia.
  - *Motley Fool Money* -- many people love to hate the Motley Fool, but I enjoy the weekly radio show. There is typically a deep dive of a business, plus a bunch of stock pitching. The hosts are entertaining and I genuinely think there are useful lessons for amateur investors.
  - *Motley Fool Industry Focus* -- as with above, accessible overviews of many different industries. I've gotten a couple of nice investment ideas from these. 
  - *Superinvestors* by Jesse Felder -- some overlap with Macro Voices and episodes are very infrequent, but episodes are usually worthwhile.
- News/world affairs:
  - *All-In* -- the pod, for better or worse, is starting to shape the narrative. They frame themselves as the counter-revolutionaries, fighting the good fight against the "Woke Mob" and cancel culture. Regardless of one's views on the hosts, All-In is hilarious and provides largely sensible takes on the biggest headlines (with a skew to California).
  - *FT News Briefing* -- I tend to listen to this every morning while I'm getting ready for my day.
  - *Economist* (audio) -- I try and listen to the Economist on commutes, but I probably only listen to 20% of the content: the World this Week, Leaders, Letters, Briefing, Business, Finance & Economics.
 - **Misc**:
   - *Huberman Lab* -- a recent find, and something I'm now addicted to (cheers to Hamza for the rec). Huberman Lab is all about developing a better understanding of our neurobiology -- think of it as evidence-based "bio-hacking". The host, Dr Andrew Huberman, is a Stanford neuroscientist who really seems to weigh the evidence behind his claims.
   - *Anatomy of Next* by Mike Solana -- a podcast on futurism and venture capital (cheers to Adi for the rec), hosted by Mike Solana from the Founders Fund. They've got a cool series exploring the nuts and bolts of a journey to Mars
 - **Retired** (pods I used to listen to frequently but now rarely do)
   - *Exchanges at Goldman Sachs* -- good for interview prep but I think Macro Voices has more original takes 
   - *How I Built This* by Guy Raz
   - *We Study Billionaires* -- a decent starting point for young investors but it's gone downhill (lots of ads, a host who became a bitcoin maxi)
   - *The Curious Quant* -- similar to Flirting with Models and well worth a listen, but I don't think it's active anymore.

### Newsletters

- [*Money Stuff*](https://www.bloomberg.com/account/newsletters/money-stuff) by Matt Levine. If you know, you know. 
- [*Byrne Hobart from The Diff*](https://diff.substack.com/) -- thoughtful and offbeat takes on business, startups, and finance.
- [*Front Month*](https://frontmonth.substack.com/) -- substack newsletter about market structure, financial exchanges etc. 
- [*Points of Return*](https://www.bloomberg.com/account/newsletters/points-of-return) by John Authers on Bloomberg opinion -- well written takes on daily financial news, with many thoughtful charts.
- [*Billion Dollar Startup Ideas*](https://www.billiondollarstartupideas.com/) -- a startup idea every day. This newsletter helps keep the creative juices flowing!
- JuliaLang forums -- weekly updates on what's new in Julia.
- Streamlit forums -- weekly updates on what's new in Streamlit (a wonderful python library)
- *eFinancialCareers* -- embarrassing but true. Gotta keep up with the finance gossip :/
- [*Finimize*](https://www.finimize.com/) -- digestible financial news that is very useful when interviewing for jobs, but less useful for me nowadays.
- [*The Felder Report*](https://thefelderreport.com/) -- Jesse Felder often seems like a permabear, which makes his bullish calls all the more interesting.
- [*Grainstone Lee Insights*](https://blog.grainstonelee.com/insight) -- monthly newsletter on the quant jobs market or miscellaneous takes on finance. Surprisingly good. 

## Timeline

- **Sep 2020 -- Present**: Interning at BlueCrest Capital Management (trading)
- **July 2020**: Interning at the D.E. Shaw Group (prop trading)
- **June 2020**: Graduated from Cambridge with a first class degree in Astrophysics. It was a strange three years and I still don't think I've had a chance to properly reflect on it.
- **April 2020**: Back to Cambridge one last time! It's hard to find a balance between socialising and exam revision.
- **December 2020**: COVID-19 round 2 -- I am studying remotely from Singapore. Sadly it won't be like last year where exams are cancelled, so I don't have as much time as I'd like to pursue side projects.
- **October 2020**: Started 3rd year at Cambridge. I swapped from physics to astrophysics, looking for something slightly more computational. 
- **June 2020**: Interning at Point72 Asset Management, doing long/short equity investment. It's tough working remotely, but the learning rate is insanely high.
- **March 2020**: pandemic times! Easter term at Cambridge is "cancelled" (i.e informal exams only). On the bright side, I've got plenty of time to work on my own projects and have been fairly active in blogging.
- **October 2019**: Started my second year at Cambridge, where I'm studying physics and maths. I'm involved with several societies, including: college tennis, Hackbridge (a tech entrepreneurship society), CUFIS (the finance soc). 
- **September 2019**: Interning at the London office of a team within GIC, Singapore's Soveign Wealth Fund, where I get to build cool things at the intersection of finance and tech.
- **July 2019**: Interning at a long-only fund called Albizia. Learning about discretionary investments, meeting company executives, building an alternative data analytics app in Ruby on Rails.
- **June 2019**: Just finished my first year at Cambridge. What an absolute whirlwind! Lots of ups and downs, but managed to come through somewhat unscathed. 
- **October 2018**: Started studying Natural Sciences at Cambridge. I spent the previous months trying to learn as much as I could about finance and statistics in the meanwhile, because I knew that I'd probably be mostly occupied with physics and maths at uni.
- **June 2018**: Splitting my time between a quantitative finance internship and blockchain consulting.
- **May 2018**: Finished national service! ORD loh
- **January 2017**: Completed the Medical Response Force conversion course, which was 8 weeks of very tough training. Military driving (March 2017), Sergeant school (Dec 2017), MRF section leader and Instructor (Jan 2018).
- **December 2017**: Finished combat medic training and started the Medical Response Force conversion course. MRF is a unit of specially trained combat medics who deal with chemical, biological and radiological threats. It is part of the Special Operations Taskforce, so we are expected to be operationally ready at all times.
- **September 2017**: Finished basic training and started combat medic training.
- **July 2016**: Enlisted into the army. In Singapore, there are two years of compulsory national service (NS) for citizens or permanent residents (that's me).
- **January 2016**: Accepted to study Physical Natural Sciences at Christ's College Cambridge, with two years deferred entry (October 2018 matriculation)
- **November 2015**: Graduated from high school, having completed my International Baccalaureate, I studied higher-level maths, chemistry, and physics, with an extended essay in maths, and scored 45 points overall.
