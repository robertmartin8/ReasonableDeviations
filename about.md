---
layout: page
title: About
---

My name is Robert. I am a soon-to-be undergraduate natural sciences (physical) student at the University of Cambridge.

I like to try to find intuitive quantitative explanations for complex phenomena, even if those intuitive explanations concede a lack of determinism. It is always nice to find order in chaos, but I think we should accept that it may very well be chaos all the way down. I'm a logical person who holds rationalism dear. Physics and mathematics are my fortes, but as of late I have been expanding that to computer science and finance.

I think man's best friend is the computer. The best and worst thing about them is that they do exactly what you tell them to – they can be extremely reliable in theory, if they are coded by a theoretically perfect person.

If you'd like to get in touch, click the link below (even if you haven't got a mail client set up, you can still see the address) 

<script language="JavaScript">
var user1 = "martin.robertandrew";
var host1 = "gmail.com";
document.write("<a href='" + "mailto:" + user1 + "@" + host1 + "'><b>Email me!</b></a>");
</script>

## Programming

Whenever I need a computer to help me, I first turn to python. I have been using python (primarily for scientific purposes) for about {{ 'now' | date: "%Y" | minus: 2014  }} years, and its initial charm has never really faded. With its pseudocode-like syntax and extremely clear objectives (type `import this` to a python console), it lets you get your ideas into a computer with relative ease. Of course you aren't going to get C-like speeds natively, but nowadays there are options like cython or Numba which can alleviate this to a certain extent.

However, I concede that R has a better statistics ecosystem – there is an R implementation for basically any useful theorem/procedure in statistics (though python is rarely far behind). I have used R for a number of data science projects, and I do appreciate its data visualisation capabilities and intuitive data processing.

I have also frequently turned to 'knowledge based computing', a nice euphemism for 'asking Mathematica what the answer is'. I've dabbled in C++ and a little bit of Haskell, and I would *love* to become more proficient at both. Understanding quicksort in Haskell was a marvellous moment:

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

I have built a couple of full-stack projects using python (Flask), and of course CSS/HTML/JS, but if I'm honest I don't get as much of a kick out of this compared to working with pure python.

## Finance

Finance is inherently interesting to me, because it represents a quantification of the beliefs and wills of so many individuals (point particles if you like) – and in my mind, quantification is the first step to prediction and/or understanding. Of course, a nice side effect of prediction is that you can make money from it.

I have spent a lot of time building predictive systems based on machine/deep learning. I'm not naïve: I know that whatever I do is probably being done far better by a hedge fund somewhere. But regardless, I do think it's interesting to test the limits of statistical methods in a system as chaotic as the American stock market.

One day I'd like to be able to use my knowledge in physics and mathematical methods to be able to properly find order in financial markets, or failing that, to quantify the chaos and understand how to mitigate its effects.


## Other specific interests

On top of the very general 'physics and maths', here are some specific interests (in no particular order):
- machine learning on fundamental stock data
- nice tricks to evaluate integrals
- 'pure' statistics (e.g probability generating functions)
- blockchain-based smart contracts, especially with Ethereum. 


## Timeline

**Right now**    
Waiting for October 2018 when I  start at Cambridge. Trying to learn as much as I can about finance and statistics in the meanwhile, because I'll probably be mostly occupied with physics at uni. 

**May 2018**

Finished national service! 

**January 2017**    
Passed out from the Medical Response Force conversion course, which was 7 weeks of very tough training. MRF is a unit of specially trained combat medics who deal with chemical, biological and radiological threats. It is part of the Special Operations Taskforce, so we are expected to be operationally ready at all times. I am now a specialist, as well as a trained military driver.

**July 2016**     
Enlisted into the army. In Singapore, there are two years of compulsory national service (NS) for citizens or permanent residents (that's me). Basic Military Training was until September.

**January 2016**    
Accepted to study the Physical Natural Sciences at Christ's College Cambridge, with two years deferred entry (entry in October 2018)

**November 2015**    
Graduated from high school, having completed my International Baccalaureate with my three higher level subjects being maths, chemistry, and physics, and extended essay in maths – I scored 45 points overall.
