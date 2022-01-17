---
layout: post
title: Intuiting the gamma function, part 3
category: maths
excerpt: "We now pick up where we left off and fully elucidate the link between the factorial and the gamma function."
---

We ended [Part 2]({{ site.baseurl }}{% post_url 2017-07-16-gamma-intuition-2 %}) with the stunning result that 

$$x! = \int_0^\infty t^x e^{-t}dt$$

Of course, this is not exactly the same as the gamma function, because the gamma function is defined as:

$$ \Gamma (x) = \int_0^\infty t^{x-1}e^{-t} dt $$

A direct observation of the above leads to the conclusion that:

$$\Gamma (x) = (x-1)! \qquad \text{or alternatively} \qquad x! = \Gamma (x+1)$$

This 'shift' is a minor annoyance and is largely a historical artifact, but don't let it distract you from the fact that **the gamma function is a superset of the factorial**. That is quite a bold statement (no pun intended), so it is worth proving. To show it, we must simply demonstrate that the gamma function can satisfy the definition of the factorial which we discussed in [Part 1]({{ site.baseurl }}{% post_url 2017-07-11-gamma-function-intuition %}), reproduced here:

- $1! = 1$
- $n! = n(n-1)!$

In terms of the gamma function, we thus need to show that

- $\Gamma (2) = 1$, the **boundary condition**
- $\Gamma (x+1) = x \Gamma (x)$, the **recursion condition**

## The boundary condition

To evaluate $\Gamma(2)$, we can start by substituting $x=2$ into the definition of the gamma function:

$$\Gamma(2) = \int_0^\infty te^{-t}$$

We can use standard integration by parts (IBP) to evaluate this:

<center>
<img src="{{ site.imageurl }}gamma/ttable.png" style="width:150px;"/>
</center>


$$\begin{align*}

    \therefore \int_0^\infty t e^{-t} dt &= \left[ -te^{-t} - e^{-t} \right]_0^\infty \\
    &= \left[ \lim_{t \rightarrow \infty} \left( -\frac{t}{e^t}\right) \right] + 1
 \end{align*}$$
 
I can tell you very quickly that this limit is equal to zero – look at what happens to the expression within the limit as $t$ grows. The $e^t$ on the denominator clearly grows much faster than the $t$ on the numerator, so as $t \rightarrow \infty$, this expression will tend to zero. In fact this is the intuition behind L'Hopital's rule, which we could apply by differentiating the top and the bottom of the fraction. 

In any case, we can conclude that

$$\Gamma(2) = \int_0^\infty t e^{-t} dt =  \left[ \lim_{t \rightarrow \infty} \left( -\frac{t}{e^t}\right) \right] + 1 = 1 \qquad QED$$
 
## The recursion condition

As a quick reminder, we need to show that:

$$\Gamma(x+1) = \int_0^\infty t^x e^{-t}dt$$

This proof is really rather elegant, so I hope you'll be as impressed with it as I was when I first understood it. 

We will start by evaluating the left hand side (LHS) using IBP, though we will do it without our table method. Choosing an $f$ and a $g'$ then substituting them into the IBP formula: 

$$\begin{aligned}
    f = t^x \implies f' = xt^{x-1}\\
    g' = e^{-t} \implies g = -e^{-t}
\end{aligned}$$

$$\therefore \int_0^\infty t^x e^{-t} dt = \left[ -t^xe^{-t} + x\int e^{-t} t^{x-1} dt\right]_0^\infty$$

To evaluate the RHS, we can split up the expression (noting that *x* can be treated as a constant)

$$\begin{aligned}
    \left[ -t^xe^{-t} + x\int e^{-t} t^{x-1} dt\right]_0^\infty &= \left[ -t^xe^{-t}\right]_0^\infty + x \left[\int e^{-t} t^{x-1} dt\right]_0^\infty\\
    &= \left[ -t^xe^{-t}\right]_0^\infty + x\int_0^\infty e^{-t} t^{x-1} dt\\
\end{aligned}$$

Let us evaluate the first term (in square brackets):

$$\left[ -t^xe^{-t}\right]_0^\infty = \lim_{t \rightarrow \infty} \left( -\frac{t^x}{e^t} \right)$$

I could just apply the same intuition as before to find this limit, but in this case it's not immediately obvious that $e^t$ grows faster than $t^x$. To show this, we just repeated differentiate the top and bottom as per L'Hopital's rule, until we can 'substitute' $t = \infty$ with sensible results (please don't show that statement to any mathematicians):

$$\lim_{t \rightarrow \infty} \left( -\frac{t^x}{e^t} \right) = \lim_{t \rightarrow \infty} \left( -\frac{xt^{x-1}}{e^t} \right) =  \lim_{t \rightarrow \infty} \left( -\frac{x(x-1)t^{x-2}}{e^t} \right) = \ldots = \lim_{t \rightarrow \infty} \left( -\frac{x!}{e^t} \right) = 0$$

To summarise:

$$\begin{aligned}
    \Gamma(x+1) &= \int_0^\infty t^x e^{-t} dt\\
    &= \left[ -t^xe^{-t}\right]_0^\infty + x\int_0^\infty e^{-t} t^{x-1} dt\\
    &= 0 + x\Gamma(x)\\
    &=x\Gamma(x) \qquad \qquad  QED
\end{aligned}$$
  
We have successfully shown that the gamma function satisfies both the boundary condition and the recursion condition of the factorial function. This means that it will *always* give the same result as the traditional factorial (ignoring the shift). For example, we know for sure that $1729! = \Gamma(1730)$ without having to calculate either quantity. 

## Extending the factorial

It is a fact that $\Gamma(x)$ is defined for all complex numbers except the negative integers (and zero), but unfortunately, its values are rarely 'nice' and can often only be evaluated with a computer. However, I'd like to show you one example that *does* turn out nicely: $\Gamma(\frac 1 2)$. 

We start by substituting 1/2 into the definition of the gamma function:

$$\Gamma (\frac 1 2) = \int_0^\infty t^{-1/2}e^{-t}dt$$

To evaluate the RHS, we make a substitution: 

$$u = t^{1/2} \implies \frac{du}{dt} = \frac{1}{2\sqrt{t}} = \frac{1}{2u} $$

$$\begin{aligned}
    \therefore \int_0^\infty t^{-1/2}e^{-t}dt &= \int_0^\infty \frac 1 u e^{-u^2} \cdot 2u~du\\
    &= 2\int_0^\infty e^{-u^2}du
\end{aligned}$$

You may recognise this very special integral, which is called the *Gaussian integral*. Although the indefinite integral has no elementary antiderivative, this *definite* integral has a beautifully simple result (the derivation of which I won't show):

$$\Gamma(\frac 1 2) = 2\int_0^\infty e^{-u^2} du = \sqrt \pi$$

I hope it's clear just how unexpected and *weird* this result is. We are saying that $\left( -\frac 1 2 \right)! = \sqrt \pi$, which is when you should realise that this whole series of posts has been a practical joke...

Not really. 

Anyway, note how we can use this value to find $\frac 1 2 !$ We see that $\frac 1 2 ! = \Gamma(\frac 3 2)$, and we can easily write this in terms of $\Gamma(\frac 1 2)$ using the recursion condition. 

$$\Gamma(\frac 3 2) = \frac 1 2 \Gamma(\frac 1 2) = \frac{\sqrt{\pi}}{2} $$

This is a result with which I like to surprise people at parties, because it's slightly more believable than the result of $(- \frac 1 2)!$. Maybe this is why nobody invites me to parties. 

<br />
## Conclusion

We have reached the end of the series on understanding the intuition behind the gamma function. I hope you now see that the weird integral really does 'contain' the traditional factorial, which you can verify at any time by doing repeated integration by parts. 

Unfortunately, despite my efforts, I can offer no intuition regarding the freaky results like $(1/2)! = \sqrt{\pi}/2$. But fortunately, you don't have to take these results on faith – you can prove them to be true (as we have done). 
