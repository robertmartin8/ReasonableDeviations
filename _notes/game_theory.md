---
layout: default
title: Yale Game Theory
---

# Game Theory – Yale (Econ 159)

The course, lectured by Professor Ben Polak, can be found on YouTube or on the [Open Yale Courses](https://oyc.yale.edu/economics/econ-159) page.

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Introduction](#introduction)
- [Iterative deletion](#iterative-deletion)
- [Nash equilibria](#nash-equilibria)
	- [Coordination games](#coordination-games)
	- [The Candidate-Voter model](#the-candidate-voter-model)
	- [The Location model](#the-location-model)
- [Mixed strategies](#mixed-strategies)
- [Evolutionary stability](#evolutionary-stability)
	- [Evolutionary stability in mixed strategies](#evolutionary-stability-in-mixed-strategies)
- [Midterm](#midterm)
- [Backward induction](#backward-induction)
- [Repeated games](#repeated-games)
- [Asymmetric information](#asymmetric-information)

<!-- /TOC -->


## Introduction

A game requires a number of components:

- Players, denoted by $i, j, \ldots$ (there are *N* players)
- Strategies: 
  - $s_i$ denotes a particular strategy of player *i*, within a set $S_i$ of possible strategies. 
  - *s* is a particular play of the game (i.e every player's choice of strategy). This is also known as a **strategy profile**.
- Payoffs, which depend on the strategy profile:
  - $u_i(s) \equiv u_i(s_1, s_2, \ldots, s_N)$
  - initially we will assume that these are all known by every player.
- It can be useful to think about the strategy profile (and payoff) in terms of player *i*'s choice combined with every other player's choice. We use $s_{-i}$ to represent the latter, such that the payoff to player *i* is $u_i(s_i, s_{-i})$
- In simple two-player game, a **payoff matrix** is a complete description of the game, encoding the payoffs that you and your opponent receives for combinations of your actions (row headers) and their actions (column headers). 

Game theory investigates how we can choose between different strategies:

- Strategy $\alpha$ **strictly dominates** strategy $\beta$ if the payoff from $\alpha$ is strictly greater than the payoff from $\beta$ regardless of opponents' choices.
  - i.e $s_i'$ is strictly dominated by $s_i$ if $u_i(s_i, s_{-i}) > u_i(s_i', s_{-i}), \forall s_{-i}$
  - NEVER play a dominated strategy
  - likewise, assume your opponent will never play a dominated strategy.
- In the case of **weak domination**, the payoff from $\alpha$ is at least as high as payoffs from $\beta$, but is strictly higher for at least one of the other player's choices.
  - $u_i(s_i, s_{-i}) \geq u_i(s_i', s_{-i}), \forall s_{-i}$ and $u_i(s_i, s_{-i}) > u_i(s_i', s_{-i})$ for some $s_{-i}$.
- Rational play can lead to Pareto inefficient outcomes. Examples:
  - classic Prisoners' Dilemmas 
  - price competition in a duopoly – each incentivised to undercut even though this will lead to collectively lower margins
  - common resources – incentive to overfish.
- Prisoners' are not just a failure of communication. You would need to change the payoffs, e.g by having contracts, or by playing repeated games. 

An important meta-parameter is how skilled your opponents are at playing games (i.e their rationality)

- It is important to consider your opponents' level of rationality, but also their belief of your rationality, their belief of your belief of their rationality, ad infinitum.
The limiting case, in which you know something, you know that they know something, you know that they know that you know something... is called **common knowledge**
  - different from mutual knowledge – it is possible for both of you to know something, but not know if the other person knows it.
  - this is the key to the "2/3 game". You have to estimate the group's rationality, and the group's view of the group's rationality, etc.

## Iterative deletion

- Since you shouldn't play a dominated strategy (and you know your opponent won't either), you can delete dominated strategies from the strategy set. But now that the game has changed, there may be new dominated strategies. These should be deleted again, and so on. This process is called **iterative deletion**.

- In a simple model of politics, we have a continuum of 10 stances on the political spectrum, from the left to the right.
  - assume voters are uniformly distributed along the spectrum, and they will vote for the nearest candidate (in case of tie, split equally)
  - the objective of the two candidates (players) is to maximise their share of the vote by choosing a position on the spectrum
- Position 1 is strictly dominated by position 2, because regardless of what the opponent does, position 2 captures more vote-share than position 1 would.
- Position 2 is *not* dominated by position 3 (because if opponent plays 1, position 2 is better off), but if we first delete position 1 (since it is strictly dominated), then position 3 *does* dominate position 2.
- We can then apply iterative deletion (and likewise from the 10/9 end by symmetry), such that the remaining strategies are the two centre strategies at 5 and 6. This is the **median voter theorem**.
- We observe the same phenomenon in product placement (similar shops cluster together).

## Nash equilibria

- Some strategies are never the best responses, regardless of the opponent's choices. These **never-best response** (NBR) strategies are not **rationalisable**.
- Hence we can also delete NBR strategies, not just dominated strategies.
- Formally, player strategy $\hat{s}\_i$ is a **best response** to the strategy $s_{-i}$ of other players if $\hat{s}\_i$ solves $\max_{s_i} u_i (s_i, s_{-i})$. Alternatively, we can frame this in terms of expected payoffs and beliefs about the other players' choices.
- Assuming your strategy set is continuous, the best response to an opponent's choice can be found by differentiating the payoff function with respect to your strategy, to find the strategy that maximises payoff.
  - in a two player game, this gives $\hat{s}\_1$ as a function of $s_2$ and $\hat{s}\_2$ as a function of $s_1$. 
  - we can use iterative deletion of NBR strategies. For some games, this converges to a single value.

<center>
<img src="{{ site.imageurl }}note_img/game_theory/gt01.png" style="width:80%;"/>
</center>

- The point of convergence is called a **Nash Equilibrium** (NE) - when each player is playing a best response to each other. 
  - formally, a strategy profile is a NE if, for each *i*, the player's choice $s_i^\*$ is a best response to the other players' choices $s_{-i}^\*$.
  - no individual can do strictly better by deviating (holding others fixed) – **no regrets**.
  - related to the idea of self-fulfilling beliefs. It can be the case that if you believe your opponent will do something and intend to act on that belief, your opponent will now act that way (even if they normally wouldn't).
- No strictly dominated strategy can be part of a NE (by definition). However, weakly dominated strategies may be part of a NE:
  - in the game below, both the bottom-right is part of the NE – no player can do strictly better by deviating.
  - however, strategy *a* dominates strategy *b*

  |   |   ***a***  |   ***b***  |
  |:-:|:----:|:----:|
  | ***a*** |  1, 1 | 0, 0 |
  | ***b*** | 0, 0 | 0, 0 |

### Coordination games

- NEs are typically easy to find via guess-and-check, as long as the strategy sets are small (even if there are many players). In the **Investment Game**, players can either choose to invest \\$10 or not invest. If more than 90% of the class invests, you make \\$5 profit. Otherwise, you lose your investment.
  - there are two NEs: everyone invests or nobody invests. However, the Pareto dominant strategy is for everyone to invest. 
  - play may converge to NEs if games are played repeatedly.
  - games with multiple NEs can be **coordination games**, in which case everybody does better off if you can coordinate (Unlike Prisoner's Dilemma, where there is a strictly dominated strategy).
  - communication can help! NE can be self-enforcing agreements. You don't need to change the payoffs with a contract/regulator to change behaviour.
  - there is scope for leadership to improve outcomes.

- Another example of a coordination game is the **Cournot duopoly**, which models a 2-firm market that is neither perfectly competitive nor perfectly cooperative.
  - each firm produces identical products $q_1, q_2$ with constant marginal costs $cq$.
  - the more the firms produced, the lower the price: $p = a - b(q_1 + q_2)$.
  - each firm aims to maximise their profit $u_1(q_1, q_2) = q_1(p-c)$, i.e revenue - cost.
  - the best-response curves are straight lines between the monopoly quantity and the competitive quantity (quantity at which price equals marginal cost)
- In Bertrand competition, firms set prices rather than quantities (they provide as much quantity as there is demand for).
  - it can be shown that the unique NE is $p_1 = p_2 = c$
  - i.e perfect competition is achieved with only 2 firms.
  - this disagrees with the Cournot model. However, differences are explained if we relax the assumption that products are identical.
- Strategies are called **strategic complements** if they mutually reinforce one another, or **strategic substitutes** if they mutually offset one another. Strategies in the investment game are strategic complements.
  - strategies in the investment game are strategic complements – if other people are investing, you also want to invest
  - the Cournot duopoly shows strategic complements


### The Candidate-Voter model

- Model an even distribution of voters on the unit real line, with voters voting for the closest candidate. 
- However, unlike the previous situation:
  - each voter is a potential candidate. They can choose to run or not to run.
  - number of candidates is endogenous
  - candidates cannot choose their position – everyone can look at track records to know if you are Left/Right. 
  - there is a prize for winning *B* and a cost of running *c*, where $B \geq 2c$. 
  - if you are *x* and the winner is at *y*, you pay a cost equal to $-\|x-y\|$ because you don't agree with the winner's policy.
- If no candidates run, it isn't an NE because someone's best response is to run and win the election.
- If only one candidate, there is a NE for a centrist candidate and odd number of voters. But if there is an even number of voters, someone else would run as a candidate.
- With two candidates, there is a NE if the two candidates are equidistant from the centre.
  - other people will not enter because the more distant candidate will win
  - however, with two candidates at either extreme (beyond 1/6 and 5/6), someone can run in the centre.

### The Location model

- Assume two towns, E and W, which can hold 100 people, and that there are 100 Tall people and 100 Short people.
- We model the people as preferring mixed towns, but failing that, they would prefer their own type.

<center>
<img src="{{ site.imageurl }}note_img/game_theory/gt02.png" style="width:60%;"/>
</center>

- The game is to choose a town, given that everybody chooses simultaneously, and that if there is no room, the excess is randomised.
- There is an unstable NE when both towns are perfectly mixed (since nobody has an incentive to deviate), but if even one person deviated then there would be an incentive to deviate.
- Despite the maximum payoff being for perfectly mixed towns, there are two stable NEs correspond to **segregation**.
- Hence initially random starting conditions can lead to segregation. 

## Mixed strategies

- In the Location model, another stable NE is if everybody chose between cities randomly. 
- This is a **mixed strategy**, i.e a randomisation over the pure strategies. This is a generalisation of a pure strategy, since a pure strategy can be thought of as a mixed strategy with probability 1 for a given choice.
- For example, in rock-paper-scissors, there is no NE in pure strategies, but a mixed-strategy NE is playing each choice with probability 1/3.
- A mixed strategy is denoted by $p_i$, where $P_i(s_i)$ is the probability that $p_i$ assigns to the pure strategy $s_i$.
- The expected payoff of the mixed strategy is the weighted average of expected payoffs of each pure strategy. 
  - thus the payoff of the mixed strategy must lie within the payoffs of pure strategies.
  - hence, if a mixed strategy is a BR, then each of the pure strategies must all be BRs
  - furthermore, all of the pure BRs must have the same expected payoff (else you could drop one out to improve payoff). 
- Hence it is quite easy to find mixed strategy NEs: find your opponent's payoffs in terms of your probabilities, then use the fact that each pure strategy must be a best response. 
  - the implication of this is that many seemingly intuitive strategies do not have the intended consequences.
  - for example, increasing the penalty for tax evasion without changing the auditor's payoffs does not change the amount of tax evasion (though it does reduce the amount of auditing needed).
  - to check a mixed NE, we need only check if we can increase the payoff from pure strategy deviations.
- Mixed strategies can also be interpreted as the proportion of players who would pick different pure strategies in a large population.

## Evolutionary stability

- We can think of genes (phenotypes) as strategies, and fitness as payoffs. 
- In this context we assume that we have a large population and that successful strategies grow (asexual reproduction so no gene redistribution).
- Strictly dominated strategies are not evolutionarily stable (ES).
- In a two player game, if a strategy is not Nash (i.e both players playing *s* is not a NE), then *s* cannot be ES because there is a strictly profitable deviation.
  - conversely, if a strategy is ES, then the strategy is NE.
  - but it is *not* true that any Nash strategy is evolutionary stable. 
  - in the below example, $(b,b)$ is a NE but is not evolutionary stable because you can profitably deviate to $a$.

  |   |   ***a***  |   ***b***  |
  |:-:|:----:|:----:|
  | ***a*** |  1, 1 | 0, 0 |
  | ***b*** | 0, 0 | 0, 0 |

- We conclude that **strict Nash strategies** are evolutionarily stable.
- Formally, in a symmetric 2 player game, the pure strategy $\hat{s}$ is **evolutionarily stable in pure strategies** if there exists an $\bar{\epsilon} > 0$ such that $\forall \epsilon < \bar{\epsilon}$, i.e "for small $\epsilon$":

$$(1-\epsilon) u(\hat{s}, \hat{s}) + \epsilon u(\hat{s}, s') > (1-\epsilon)u(s', \hat{s}) + \epsilon u (s', s')$$

- In other words, for any possible deviant strategy $s'$, the payoff for $\hat{s}$ against the mixed population (including the deviants) is greater than the payoff for $s'$ against the mixed population (including the deviants).
- An alternative definition is that the pure strategy $\hat{s}$ is ES in pure strategies if:
  - $(\hat{s}, \hat{s})$ is a strict NE, i.e $u(\hat{s}, \hat{s}) > u(s', \hat{s}), \forall s'$ 
  - OR $(\hat{s}, \hat{s})$ is a weak NE and $u(\hat{s}, s') > u(s', s')$.
  - the second condition states that for an incumbent to be ES, it must do **strictly** better against the mutant than the mutant does against itself. 
  - this definition makes it easier to verify that a strategy is ES (compared to the previous one).
  - in fact this definition applies directly to the case of mixed strategies, after replacing all *s* with *p*.
- ES strategies may not be Pareto efficient, if there are two strict NEs with different payoffs.

### Evolutionary stability in mixed strategies

- In the symmetric game of chicken (i.e aggression vs non-aggression), the payoff matrix is as follows:

|   |   ***a***  |   ***b***  |
|:-:|:----:|:----:|
| ***a*** |  0, 0 | 2, 1 |
| ***b*** | 1, 2 | 0, 0 |

- There is no symmetric pure-strategy NE, but there is a symmetric mixed-strategy NE with 2/3 aggression and 1/3 non-aggression.
- Mixed NEs cannot be strict by definition, because you are indifferent to the strategies.
- Stable pure populations are called **monomorphic**, while stable mixed populations are **polymorphic**.
- An important game in biology is the **Hawk-Dove game** – this does not refer to different species, but rather aggressive vs non-aggressive strategies.
  - two individuals are competing for some prize $v > 0$
  - if they both fight for it, the prize is split but each species incurs some cost $c>0$
  - if they are both doves, the prize is split equally
  - if one is a hawk and one is a dove, the hawk wins the entire prize.

|   |   ***H***  |   ***D***  |
|:-:|:----:|:----:|
| ***H*** |  (v-c)/2, (v-c)/2 | v, 0 |
| ***D*** | 0, v | v/2, v/2 |

- $(D,D)$ is not a NE so Dove is not an ESS. But $(H, H)$ *is* a NE if $(v-c)/2 \geq 0$, where the strictness depends on whether the $\geq$ is an equality.
  - if $v =c$ (so weak NE), we must check that $u(H,D) > u(D,D)$, which is indeed true. So Hawk is an ESS if $v \geq c$.
  - if $c > v$, neither Hawk nor Dove are ESS, so we must consider symmetric mixed strategies
  - the result is that $\hat{p} = \frac{v}{c}$ of the population will be hawks.
- The ESS agrees with the intuition that as *v* increases or *c* decreases, there are more Hawks (and the converse for Doves).
  - the payoff for being a dove is $(1- \frac v c)(\frac v 2)$. As *c* increases, the payoffs actually increase because the number of fights goes down
  - from this analysis, we can infer the payoff matrix by looking at the proportion of Hawks.

## Midterm

*I sat the midterm closed-book and under timed conditions. In reality I probably would have tried to give a bit more detail – hence the current marking may be a bit generous, because if I got the right numerical answer I assumed that my method was correct and gave myself all the marks.*

[Completed Midterm]({{ site.imageurl }}/note_img/game_theory/econ159_midterm.pdf)


## Backward induction


## Repeated games


## Asymmetric information
