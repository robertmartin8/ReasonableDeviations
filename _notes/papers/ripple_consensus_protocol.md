---
layout: default
title: The Ripple Protocol Consensus Algorithm
---

## The Ripple Protocol Consensus Algorithm

*Schwartz, D., Youngs, N., & Britto, A. (2014). The Ripple protocol consensus algorithm. Ripple Labs Inc White Paper, 1–8. Retrieved from
[ripple.com](https://ripple.com/files/ripple_consensus_whitepaper.pdf).*

- Distributed consensus systems allow for fast, low-cost, decentralised transactions. Technical problems are grouped into three categories: correctness, agreement, utility.
- **Correctness**:
    - the system needs to distinguish between correct and fraudulent transactions
    - traditionally, trusted institutions guarantee transactions with cryptographic signatures, but in general distributed systems there are no trusted parties.
- **Agreement**:
    - a single global truth needs to be maintained, i.e. only one set of globally recognised transactions can exist
    - this is necessary to avoid the **Double-Spend Problem**
- **Utility**:
    - this is the most abstract and poorly defined of the three categories. It relates to what the network is like for users.
    - e.g. latency, computing power required for correctness, the technical proficiency required by users

### Consensus

- **Nonfaulty** nodes behave honestly and without error, whereas **faulty nodes** may be honest (e.g. suffering from data corruption) or malicious (*Byzantine* errors). 
- Consensus is defined according to three axioms:
    - **C1.** Every nonfaulty node makes a decision in finite time
    - **C2.** All nonfaulty nodes decide on the same value
    - **C3.** Nonfaulty nodes have multiple possible values (this condition prevents trivial uniform solutions)
- **Fischer, Lynch, Patterson (FLP) impossibility** states that in the *asynchronous case* (individual nodes' decision times are not bounded), termination cannot be guaranteed if there are any faulty nodes. Thus, time-based heuristics must be adopted.
- In the *synchronous case*, it has been proven that no network can tolerate more than $(n-1)/3$ Byzantine faults. However, this can be overcome if there is verifiable authenticity (e.g. digital signatures) on the messages between nodes.
- Previous asynchronous algorithms have achieved Byzantine fault tolerances of $(n-1)/5$ (FaB Paxos), $(n-1)/4$ (phase algorithm), and $(n-1)/3$ (BFT-CUP, with certain added constraints). 

### The Ripple Protocol Consensus Algorithm (RPCA)

- Each server/node *s* maintains a **unique node list (UNL)**, which are the only nodes that *s* queries when determining consensus. The UNL forms a trusted subset of the network, even if some individual members of the UNL are not trusted.
- The RPCA is applied by all nodes every few seconds, reaching consensus assuming there are no forks or system errors. The RPCA proceeds in rounds:

1. Each node publishes all valid transactions it has seen that are not yet part of the ledger, forming a **candidate set**
2. Each node amalgamates the candidate sets of all the nodes in its UNL, then votes on the veracity of each transaction.
3. Transactions with more than a certain percentage of 'no' votes are either discarded or included in the next candidate set.
4. If at least 80% of a node's UNL agrees on a transaction, it is applied to the ledger.
5. The ledger is closed, becoming the new **last-closed ledger**.

- **Strong correctness** is guaranteed for $f \leq (n-1)/5$ Byzantine failures. However, although consensus cannot be reached for $20\% < f < 80\%$, in this case fraudulent transactions will not be confirmed. Thus **weak correctness** is maintained for any $f \leq (4n+1)/5$.
- If the probability of a node colluding maliciously with other nodes is $p_c$, then the probability of correctness $p^*$ is given using the Binomial Distribution:

$$p^* = \sum_{i=0}^{\lceil (n-1)/5 \rceil} \binom{n}{i} p_c^i (1-p_c)^{n-i}$$

- Thus the UNL must be chosen to minimise $p_c$. This is possible because nodes are cryptographically identifiable, so specific nodes can be grouped with other nodes such that they are less likely to form Byzantine collusions. High $p_c$ values can be somewhat compensated for with more nodes in the UNL, e.g. $p_c = 0.15, n=200 \implies p^* = 90\%$. 
- To meet the agreement criterion, some level of interconnectivity is required in the network: the formation of UNL 'cliques' may result in forks, as different parts of the network have different ledgers. 

- To satisfy the utility criterion, it must be shown that consensus is reached in finite time. Because the RPCA is deterministic and has a preset number of rounds, the limiting factor is latency between nodes. This is bounded by removing nodes where latency increases beyond a certain value. Note that the final UNL must satisfy the correctness/agreement bounds in terms of network size and connectivity. 
- A number of other heuristics are used:
    - mandatory 2 second window for nodes to propose candidate sets, to allow for slow connections within latency bounds.
    - during voting, nodes can be flagged and removed for repeated malicious behaviour, such as consistent 'no' votes
    - default UNLs are provided to minimise $p_c$, though nodes are encouraged to choose their own UNL.
    - each node monitors the size of its UNL: a sudden drop may signify a fork.
    - the rounds in the RPCA allow for latency detection to improve overall network speed. 
- The overall low latency, speed, and provable security (given the bounds) of the Ripple network make it ideal for financial transactions. 



