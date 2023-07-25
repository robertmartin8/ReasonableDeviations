---
layout: default
title: Introduction to Complexity
---

## Introduction to Complexity

Course from the [Santa Fe Institute](https://www.complexityexplorer.org/).

<!-- TOC -->
- [Complex systems](#complex-systems)
- [Dynamics and Chaos](#dynamics-and-chaos)
- [Fractals](#fractals)
- [Information, Order, and Randomness](#information-order-and-randomness)
- [Genetic algorithms](#genetic-algorithms)
- [Cellular automata](#cellular-automata)
- [Self-organisation in biological systems](#self-organisation-in-biological-systems)
- [Cooperation in social systems](#cooperation-in-social-systems)
- [Networks](#networks)
- [Scaling](#scaling)
<!-- /TOC -->

### Complex systems

- Complex systems are composed of simple components (agents) which interact in nonlinear ways, without central control, which result in **emergent** behaviours.
- Examples of emergent behaviours include hierarchical organisation, information processing, dynamics, evolution and learning.
- Statistical mechanics only works for "disorganised complexity", where the units interact linearly.

### Dynamics and Chaos 

- Dynamical systems theory gives a vocabulary and set of tools for analysing how systems change over time.
- Many systems display "sensitive dependence on initial conditions" -- **chaos**.
- A simple (linear) model of population growth has a constant birth rate $r_b$ at each time, resulting in overall exponential growth. The Logistic model includes the death rate $r_d$, as well as a nonlinear term related to the maximum population $n_{max}$, which models deaths due to overcrowding:

$$n_{t+1} = (r_b - r_d)(n_t - \frac{n_t^2}{n_{max}})$$

- This model can be rewritten as the **logistic map**: $X_{t+1} = R X_t (1- X_t)$. The dynamics of this depend on the initial conditions. 
  - For some values, there is a **fixed point attractor** to which the system asymptotes.
  - For some values, there is a **periodic attractor** and the system oscillates between two different states. As you increase *R*, you will reach a point where the period doubles to 4. 
  - For a sufficiently high *R*, the system is chaotic -- there is no apparent periodicity.
- All **unimodal** (single humped) parabola-like maps display this period-doubling route to chaos, e.g. $x_{t+1} = \lambda \sin(\pi x_t)$. Furthermore, the limiting rate at which bifurcations shrink is a universal constant (Feigenbaum's constant).

### Fractals

- Fractals are objects that are self-similar at different scales.
- Dimension can be defined as the number *D* for which each level is made up of $M^D$ $1/M^D$-sized copies of the previous level, e.g. a cube is made out of 8x cubes of side length 1/2 the original. The dimension encodes the number of copies you get when you reduce the side length by a factor of $M$. (Technically, this is the **Haussdorf dimension**)

$$D = \frac{\log N}{\log M}$$


- In the real world, fractional dimension can be measured using the **box-counting method**. You divide the image into boxes and count the number of boxes that the edge intersects. Then increase box resolution and re-count etc. The dimension is the gradient of an appropriate log-log plot:

$$\log(\text{num boxes}) = D \log (1 / \text{box side length})$$

### Information, Order, and Randomness

- The second law of entropy is a statistical certainty, but not a law in the traditional sense.
- Maxwell's Demon was resolved by Szilard who stated that the process of obtaining information from the gas molecules increases entropy.
- In Shannon's formulation of information, there is a message source (set of possible messages and their probabilities), whose information content *H* is given by: $H(x) = - \sum_x p(x) \lg p(x)$
- In general, the information content is the average number of bits needed to encode a message from a given source, given an optimal encoding.

### Genetic algorithms

- If we are using a genetic algorithm to find an optimal strategy (as opposed to optimal value), we may need to encode the strategies as a bit string.
- Alternatively, we can generate a population of random trees (with syntactic constraints). Crossover then involves exchanging subtrees, while mutation replaces a subtree by a randomly generated subtree.

### Cellular automata 

*I have explored this topic in a [blog post]({{ site.baseurl }}{% post_url 2018-05-25-evolving-cellular-automata %}) and this [summary]({{ site.url }}/notes/papers/evolving_cellular_automata).*

- In an elementary cellular automata (ECA), we have a 1D universe where each cell looks at its left and right neighbours. Hence there are 8 possible states and $2^8 = 256$ possible rulesets, which can be encoded as an integer.
- Stephen Wolfram found four classes of ECA behaviour for different rulesets (each of which applies to almost all initial configurations):
  - Relax to the same fixed configuration
  - Relaxed to a periodic cycle of configurations or a fixed point, depending on the ICs.
  - Behaviour turns chaotic
  - Complex localised structures with apparent patterns
- ECAs can be viewed as dynamical systems with discrete states (rather than continuous like with the logistic map). The analogy to the control parameter *R* is $\lambda$, the fraction of black output states in the rule table.
  - Like $R$, increasing $\lambda$ generally causes the system to cycle from fixed to periodic to chaotic (though it is symmetric because the system is 1s and 0s).
  - $\lambda$ is a better predictor when the neighbourhood size is larger
- Wolfram hypothesised that all class 4 (edge of chaos) CAs can be used for computation.
  - Hard to prove universality in general
  - But they showed that Rule 110 is a universal computer. Moving particles transfer information; particle collisions integrate information from different spatial locations

### Self-organisation in biological systems

- Self-organisation is the production of organised patterns resulting from decentralised interactions. These are common in biology.
- **Flocking** can be explained by the **boids** algorithm, which uses simple rules: collision avoidance, velocity matching, and flock centering.
- **Synchronisation**, e.g. fireflies flashing, cells firing together, is an important adaptive trait. 
- Army ants performing complex tasks (see summary [here]({{ site.url }}/notes/papers/army_ants_collective_intelligence))

### Cooperation in social systems

- The repeated Prisoner's dilemma demonstrates the rationality of cooperation.
- Traditional economics tries to make predictions assuming equilibrium dynamics and analytic models. Complexity economics instead uses agent-based models.
- The **El Farol Problem** (Brian Arthur) relates to a bar that will fit 60 people. 100 people want to go, but only if 60 or fewer go. There is no prior communication, but everyone has historical data for attendance rates. How should people decide whether to go (in other words, how do they predict the next number)? 
  - Each person has *N* different strategies and we have *M* weeks of data. We want to predict the attendance at time *t*, $A(t)$. Each strategy is a linear combination of previous week attendances: $S(t) = w_1 A(t-1) + \ldots + w_M A(t-M) + c$
  - The current best strategy at each time step is the strategy that would have been the best predictor at previous time steps, minimising $\\|S(t) - A(t)\\| + \ldots + \\|S(t-M) - A(t-M) \\|$
  - Initially we start with random weights
  - People decide whether or not to go based on their current best strategy.
- The "El Farol" problem assumes bounded rationality and includes adaptation. Experimental results show that efficiency (i.e consistent 60 attendance) *is* possible.

### Networks

- We might guess that real-world networks are either highly regular, with high average distance between nodes and high average clustering, or random, with low average distance between nodes and low clustering. However, many different networks e.g. power networks, social graphs, biological neural networks, do not show this (Watts and Strogratz).
- Common properties of real-world networks:
  - Small world property ("six degrees of separation") -- short paths between most pairs of vertices.
  - Clustering and community structure
  - Long-tailed degree distribution. Sometimes, *scale-free* (power law), i.e subgraphs have the same degree distribution as the whole graph.
  - Robustness to random node failure, unless one node causes others to fail, resulting in cascading failure (e.g. power networks, banks).
  - Vulnerability to targeted hub attack
- One measure of clustering (with respect to a particular node) is the fraction of pairs of neighbours that are connected to one another
- The distribution of node degrees is not random, there tend to be **hubs**.
- Small-world networks can be generated by starting with a regular network and randomly rewiring some fraction.
- One model that explains the long-tailed degree distribution is **preferential attachment** -- for a new node, its probability of attaching to an existing node is proportional to the degree of that node.

### Scaling

- Scaling is the study of how system properties change with the size of the system.
- In general, many attributes (e.g. area-volume) have power-law scaling.
- An important problem in biology is understanding **metabolic scaling**, i.e how the power output of an organism varies with size.
- A natural hypothesis is that metabolic rate is proportional to body mass, but this is incorrect because heat dissipation is proportional to surface area (grows more slowly than volume) so large organisms would be very hot.
- The *surface hypothesis* is that metabolic rate is proportional to the 2/3 power of mass (so that surface dissipation is constant).
- However, Kleiber's law is an empirical finding that shows that metabolism varies as the 3/4 power of mass, suggesting that evolution has caused some efficiency gains.
- It turns out that many biological processes have 1/4 power scaling, e.g. heart rate inverse to body mass,  blood circulation time vs body mass, life span vs body mass, growth rate inverse to body mass, tree height vs tree mass.
- West, Brown, and Enquist hypothesised that biological rates are limited by the rates at which energy can be distributed at interfaces, not by the surface area, so we need to consider the space-filling properties of e.g. branching bronchi or capillaries.
- Their interpretation is that the surface hypothesis is true if we are referring to 4D volume, because the interfaces are not 2D but rather $2<D<3$.
