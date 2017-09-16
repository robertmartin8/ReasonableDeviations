---
layout: page
title: About
---

My name is Robert. I am a soon-to-be undergraduate natural sciences (physical) student at the University of Cambridge.

I like to try to find intuitive quantitative explanations for complex phenomena, even if those intuitive explanations concede a lack of determinism. It is always nice to find order in chaos, but I think we should accept that it may very well be chaos all the way down.

I'm a logical person who holds rationalism dear. Physics and mathematics are my fortes, but as of late I have been expanding that to computer science and finance.

I think man's best friend is the computer. Someone once said that the best and worst thing about them is that they do exactly what you tell them – they can be extremely reliable in theory, if they are coded by a theoretically perfect person.

## Programming

Whenever I need a computer to help me, I first turn to python. I have been using python (primarily for scientific purposes) for about 3 years, though it was only a year ago that I started to make use of some core language features like classes. With its pseudocode-like syntax and extremely clear objectives (type `import this` to a python console), it lets you get your ideas into a computer with relative ease.

However, I concede that R has a better statistics ecosystem – there is an R implementation for basically any useful theorem/procedure in statistics. I have used R for a number of data science projects, and I do appreciate its data visualisation capabilities, which are arguably superior to those of python.

I have also frequently turned to 'knowledge based computing', a nice euphemism for 'asking Mathematica what the answer is'. I've dabbled in C++ and a little bit of Haskell, and I would *love* to become more proficient at both. Understanding quicksort in haskell was a marvellous moment:

```haskell
quicksort :: (Ord a) => [a] -> [a]
quicksort [] = []
quicksort [x] = [x]
quicksort (x:xs) =
    let smallersorted = quicksort (filter (<= x) xs)
        biggersorted = quicksort (filter (> x) xs)
    in smallersorted ++ [x] ++ biggersorted
```

One day soon I am going to start learning Java, partially because that's what Cambridge will teach in their first year for computer science, and partially because pain builds character.


## Finance

Finance is inherently interesting to me, because it represents a quantification of the beliefs and wills of so many individuals (point particles if you like) – and in my mind, quantification is the first step to prediction and/or understanding. Of course, a nice side effect of prediction is that you can make money from it.

I have spent a lot of time building predictive systems based on machine/deep learning. I'm not naïve: I know that whatever I do is probably being done far better by a hedge fund somewhere. But regardless, I do think it's interesting to test the limits of statistical methods in a system as chaotic as the American stock market.

One day I wish to be able to use my knowledge in physics and mathematical methods to be able to properly find order in financial markets, or failing that, to quantify the chaos and understand how to mitigate its effects.


## Other specific interests

On top of the very general 'physics and maths', here are some specific interests (in no particular order):
- machine learning on fundamental stock data
- nice tricks to evaluate integrals
- 'pure' statistics (e.g probability generating functions)
- blockchain-based smart contracts, especially Ethereum


## Timeline

**Right now**    
Still waiting for October 2018 when I can start at Cambridge. In the mean while, I'm trying to make the most of my time in NS.

**January 2017**    
Passed out from the Medical Response Force conversion course, which was 7 weeks of very tough training. MRF is a unit of specially trained combat medics who deal with chemical, biological and radiological threats. It is part of the Special Operations Taskforce, so we are expected to be operationally ready at all times. I am now a specialist, as well as a trained military driver.

**July 2016**     
Enlisted into the army. In Singapore, there are two years of compulsory national service (NS) for citizens or permanent residents (that's me). Basic Military Training was until September.

**January 2016**    
Accepted to study the Physical Natural Sciences at Christ's College Cambridge, with two years deferred entry (entry in October 2018)

**November 2015**    
Graduated from high school, having completed my International Baccalaureate with my three higher level subjects being maths, chemistry, and physics, and extended essay in maths – I scored 45 points overall.
