---
layout: default
title: Princeton Bitcoin
---

# Bitcoin and Cryptocurrencies – Princeton

*The course can be found [here](https://www.coursera.org/learn/cryptocurrency). I thought that it was really a rather good introduction to the subject.*

## Contents
<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:0 orderedList:0 -->

- [1. Bitcoin Theory](#1-bitcoin-theory)
	- [Hash functions](#hash-functions)
	- [Digital signatures](#digital-signatures)
	- [A centralised cryptocurrency](#a-centralised-cryptocurrency)
	- [Decentralisation](#decentralisation)
	- [Incentives](#incentives)
- [2. Bitcoin mechanics](#2-bitcoin-mechanics)
	- [Bitcoin scripting language](#bitcoin-scripting-language)
	- [Some special bitcoin tricks](#some-special-bitcoin-tricks)
	- [Limitations to bitcoin](#limitations-to-bitcoin)
	- [Key splitting](#key-splitting)
- [3. Bitcoin user interaction](#3-bitcoin-user-interaction)
	- [Storing bitcoins](#storing-bitcoins)
	- [Bitcoin exchanges](#bitcoin-exchanges)
	- [The bitcoin exchange market](#the-bitcoin-exchange-market)
	- [Transaction fees](#transaction-fees)
- [4. Bitcoin Mining](#4-bitcoin-mining)
	- [Rough overview to the mining process](#rough-overview-to-the-mining-process)
	- [Different mining hardware](#different-mining-hardware)
	- [Energy consumption](#energy-consumption)
	- [Mining pools](#mining-pools)
	- [Mining incentives and strategies](#mining-incentives-and-strategies)
- [5. Bitcoin community and politics](#5-bitcoin-community-and-politics)
	- [Consensus](#consensus)
	- [The bitcoin core and the Bitcoin Foundation](#the-bitcoin-core-and-the-bitcoin-foundation)
	- [The roots of bitcoin](#the-roots-of-bitcoin)
	- [Bitcoin and governments](#bitcoin-and-governments)
- [6. The Bitcoin platform: other applications of the bitcoin blockchain](#6-the-bitcoin-platform-other-applications-of-the-bitcoin-blockchain)
	- [Secure timestamping](#secure-timestamping)
	- [Overlay coins](#overlay-coins)
	- [Bitcoins as ‘smart property’](#bitcoins-as-smart-property)
	- [Prediction markets](#prediction-markets)
- [7. Altcoins](#7-altcoins)
	- [Alternative mining puzzles](#alternative-mining-puzzles)
	- [Proof of stake](#proof-of-stake)
	- [Some altcoins](#some-altcoins)
	- [Metric for comparing altcoins](#metric-for-comparing-altcoins)
	- [Interaction between bitcoin and altcoins](#interaction-between-bitcoin-and-altcoins)
- [The future of the blockchain and Bitcoin](#the-future-of-the-blockchain-and-bitcoin)
	- [The blockchain as a vehicle for decentralisation](#the-blockchain-as-a-vehicle-for-decentralisation)
	- [Ways that you can use the blockchain](#ways-that-you-can-use-the-blockchain)
	- [What can be decentralised?](#what-can-be-decentralised)
	- [Is decentralisation a good idea?](#is-decentralisation-a-good-idea)

<!-- /TOC -->

<br/>
## 1. Bitcoin Theory

### Hash functions

- A **hash function** takes any string as an input, and maps it to a finite output, e.g 256 bytes. Essential properties:
    - Collision free – nobody can find *x* and *y* such that $H(x) = H(y)$$. This property has never been proven, but is probabilistic. 
    - Hiding property – cannot recover *x* from $H(x)$. In practice, we concatenate a random string *r* to *x* before hashing.
- Bitcoin uses the SHA-256 hashing algorithm. 
- If we hash a piece of information, we now have a unique way of referring to that piece of information. **Hash pointers** are hashes that are used to reference pieces of known information. 
- A **blockchain** is a linked list of hash pointers. Each block contains the hash of the previous block’s information. It is tamper evident, because any changes will propagate until we see that the leading hash changes. 

### Digital signatures

- An individual has a **secret key**, i.e a random number that only they know, and a **public key** that anyone can see. 
- You must sign a message (or *H*(msg)) with your secret key, but anyone can verify with only your public key that the signature is valid.
- Signatures are specific to particular messages.
- Bitcoin uses the elliptic curve digital signing algorithm (ECDSA), which is vulnerable to quantum computers. 
- If we sign a hash pointer to a block on the blockchain, we effectively sign the whole chain. 
- The public key (or the hash of it) is equivalent to a bitcoin address. 

### A centralised cryptocurrency

1. Scrooge creates a new coin, writing a unique coin ID and signing. This is put into a block. 
2. To pay a coin, Scrooge creates a new block, which contains:
    1.  a 'pay to Alice’ statement (which he signs)
    2. a hash of the previous block (the creation block). 
3. Alice can now do the same thing to transfer the coin to other people, signing a transfer statement and including the hash of the previous block. What stops Alice from paying the same coin to many people (**double spending**)?
4. Scrooge publishes and signs the blockchain, so anyone can see that their coins haven’t been double spent. 

The problem is that this is centralised on Scrooge.


### Decentralisation

- Decentralised cryptocurrencies need to implement a form of **distributed consensus**. 
- Bitcoin uses an **implicit consensus** scheme:
    - In each round, a random node in the network is selected to propose the next block in the blockchain.
    - Other nodes then:
        - accept, by extending that block
        - reject, by extending from a previous block
- A malicious node cannot:
    - steal bitcoins without private keys
    - deny service to a particular user, because if he does not add a transaction to his block, someone else will. 
- A malicious user Eve can receive goods from a vendor Bob, and sign a valid payment to him. However, if it is Eve’s turn to propose a block, she can simply propose a payment to another address (e.g. owned by herself) If this block gets accepted, then Eve has received her goods without paying Bob. The payment to Bob is in an **orphaned block**. 
    - To prevent this, Bob must wait for ‘confirmations’ – extensions of the chain in which Eve pays Bob.
    - The double spend probability decreases exponentially with the number of confirmations. In practice, people wait for six.        

### Incentives

1. The block reward:
- The creator of a block can include a coin creation transaction, to send a bitcoin to an address of his choice. 
- But this block will only be valid if other nodes accept it – providing an incentive to behave honestly. 
- The block reward is a fixed value (now 25BTC) which halves every 4 years. 
- This is the only way new bitcoins are created, and there is a finite total supply. 

2. **Transaction fees:**
- The creator of a transaction can make the output slightly less than the input, the difference being interpreted as a transaction fee. 

In order to stop everyone competing for these rewards, and to reduce the likelihood of malicious **sybil nodes**, bitcoin uses a **proof of work** (POW) scheme, in which the next block creator is chosen based on computing power. Bitcoin uses **hash puzzles**:
- We have to find a string nonce such that the hash of [nonce appended to the previous block] is very small. This is essentially trying to guess which number will win a lottery. 
- The difficulty of the hash puzzles increases over time. 
- This can only be done by brute forcing (more than 10^20 hashes per block on average). 

The probability of winning the next block is equal to the fraction of global hash power that you have. Attacks on bitcoin are therefore infeasible unless you have more than half of the global hash power. Such an individual is called a **51% attacker**. 
- The attacker cannot steal coins because of the underlying cryptography
- Although he can suppress certain transactions from the blockchain, honest nodes will see this. 
- Rules of the system like the block reward cannot be changed. 
- However, confidence in bitcoin would likely be destroyed. 

<br/>
## 2. Bitcoin mechanics

### Bitcoin scripting language

Bitcoin uses a scripting language:
- simple, but supports cryptographic functions
- stack based
- limits on time/memory
- no looping

e.g ``<sig><pubkey> OP_DUP OP_HASH <pubkeyHash> OP_EQUALVERIFY OP_CHECKSIG``

1. Pushes signautre and public key onto stack
2. Duplicates public key
3. Hashes public key
4. Push input public key hash onto the stack
5. Check if the hashes are equal and consumes 
6. Check that the pub key signed the transaction. 

- A script is valid if it does not cause an error
- `OP_CHECKMULTISIG` is a function for joint signatures, such that for *N* public keys, at least *T* must sign for a transaction to be valid. There is a known bug that this pops one extra block off the stack, so we put in a dummy node.
- Most nodes whitelist a certain number of scripts and reject everything else. 
- A **proof of burn** script can never be valid (intentional error built in), but it provides an opportunity for people to write arbitrary data into the blockchain.
- It is normally the sender’s job to specify the script. But a famous development (not in the original implementation) is **pay to script hash**: the vendor can provide the hash of the correct script on their website instead of their bitcoin address. 

### Some special bitcoin tricks

**Escrow transactions**

Scenario: A wants to buy goods from B, but neither wants to be the first sender. 

Solution:
- A sends the coins to escrow, with a statement 
`pay x to 2-of-3 Alice, Bob, Judy (multisig)`
- Now, Bob sends the goods. If A and B are both happy, they will both sign coin transfer A -> B.
- If A is not happy, neither A nor B will sign to release the coins from escrow to the other party. Judge Judy will arbitrate, and either sign A -> A or A -> B

**Green addresses**

Scenario: B is offline, but needs to receive payment from A

Solution:
- A will pay a bank and the bank will sign a payment to B, with a guarantee that it will not be double spent. This is not a cryptographic guarantee, it is based on trust for the bank. 

**Efficient micro-payments**

Scenario: A wants to pay B per minute without incurring transaction fees. 

Solution:
- Pay 100 to Bob/Alice multisign (with a Lock time, such that coins get returned to alice after a certain amount of time). 
- After every minute of using a service, Alice signs
 
 time       | sent to B | sent to A
------------ |------------ | -------------
t0|01 -> B | 99 -> A 
t1|02 -> B | 98 -> A 
t2|03 -> B | 97 -> A 
|...     | ...     
t43|42 -> B | 58 -> A 


- When Alice stops using it, she will just stop writing these statements. Bob will sign the latest transaction, and this is the only one that makes it to the blockchain. 

### Limitations to bitcoin

- 10 minute average creation time of a block, and people normally wait 6 blocks.
- 20k signatures / block
- 100m satoshi per bitcoin
- 21m total bitcoins
- 50, 25, 12.5 … mining reward. 
- 7 transactions per second 
- Only 1 signature algorithm 
- None of this can be changed without a disruptive hard fork. 

### Key splitting


Let *P* be a large prime number (not necessarily secret). Let *S* be a secret key, $S \in [0,P)$. We want to split this key into *N* pieces such that *K* pieces are sufficient to reconstruct *S*. Let *R* be a secret random number such that $R \in [0,P)$. 

e.g $K=2$. 
We will imagine *S* to be a point on the *y*-axis. Finding the coordinates of this point is equivalent to finding *S*. Now we imagine a line with gradient *R*, running through *P*. Any two points on this line are enough to find *S*, thus $K=2$. *N* can be anything, because we can supply arbitrarily many points on this line as ‘pieces’ of the key. 

$K = 3$ 
Basically, we need a function that requires 3 points to specify the intercept. This is simply a quadratic. 

$K > 3$
Just use higher order polynomials. 

And you can supply as many sets of $(R_1, R_2, \ldots, R_k)$ as you like.

<br/>
## 3. Bitcoin user interaction

### Storing bitcoins

* To spend a bitcoin, you need some info from the blockchain e.g coin ID and value (publicly available) and your secret key to sign a transfer. So storing bitcoins is equivalent to storing your secret key. 
* The simplest way is to store the key as a file locally. This is convenient, but if your device is lost/wiped/compromised your bitcoins are gone. 
* An alternative is a **bitcoin wallet**. These often have nice UIs, and give your coins different addresses to benefit anonymity. They can turn your public key into a text string or QR code so it is easy for people to send you money They are convenient, and can work on multiple devices. The only issue is security concerns. 
* The **hot/cold storage** technique is often used (just like how we keep some money in our wallet but most in our bank). 
    * hot storage is the online part, cold storage is offline. Each will have the public key of the other, so you can make transfers. 
    * cold storage is typically info stored on an offline device, or a paper wallet. 

### Bitcoin exchanges

* Accept deposits of bitcoin or other fiat currency
* Then, you can make and receive BTC payments.
* You can change BTC for currency, but this *does not go onto the blockchain* – the bank is simply changing its obligations to you. 
* These exchanges have the same inherent risks as banks, but are much more dangerous because of poor regulation: many exchanges go insolvent. 
* A bitcoin exchange can prove it has a certain amount of money by publishing a payment-to-self and signing it. It can then prove the amount of the deposits by making a merkle tree variant wherein each hash pointer includes the value in its subtree. Dividing the two values can prove that the exchange has a certain **fractional reserve**. 

### The bitcoin exchange market

* The market must match buyers and sellers
* It is large and liquid enough to reach a consensus price, determined by supply and demand. 
* The supply of bitcoin may or may not include the demand deposits within exchanges, depending on your analysis. 
* Demand comes from people wishing to mediate fiat currency transactions, and investors.

A simple transaction demand model (ignoring investors)
- *T* = total transaction value mediated by bitcoin ($/sec)
- *D* = duration that BTC is needed for in a transaction (sec)
- *S* = liquid supply of BTC
- *P* = exchange rate ($/BTC) 

In one second $T/P$$ bitcoins needed, $S/D$$ made available.
Therefore the price in equilibrium should be $TD/S$. 

### Transaction fees

* Because your transaction costs money for the network to process, there is a ‘voluntary’ transaction fee, which you can make by setting the output of a transaction to be less than the input. 
* The larger your transaction fee, the faster your transaction will be processed because the nodes will be competing for it. 
* The priority of a transaction is proportional to the age of the block and the value of the block, and inverse proportional to the transaction size.
* As of September 2017, the median transaction fee is 9040 satoshis ($0.33)

<br/>
## 4. Bitcoin Mining

### Rough overview to the mining process

1. Become a node in the network, and validate all proposed transactions.
2. Listen for new blocks
3. Assemble a new valid block
4. Find the nonce to make your block valid
5. Hope everyone accepts your block
6. Pocket the 25btc block reward 

The hard part is finding the nonce to make your block valid. Every two weeks, the difficulty is adjusted based on the time it took to mine the previous 2016 blocks (to maintain a constant 10 minute/block time). On average, it takes 9 minutes. Towards the end of the two week cycle, it takes 8 minutes.  

### Different mining hardware

**GPUs**

Highly parallelised with high-throughput, can be optimised for SHA256. 
* 200MHz hashrate compared to 20MHz in cpu
* Disadvantages:
    * GPUs have other features that are wasted on bitcoin miners
    * poor cooling
    * large power draw
    * not meant to be used in conjunction

**Field Programmable Gate Area (FPGA)**

* Customisable chips so certain operations can be built into the hardware
* Much higher performance than GPUs (1GHz), better cooling.
* Disadvantages:
    * higher power draw
    * few sufficiently talented hobbyists
    * more expensive (very incremental cost advantage).

**Application Specific Integrated Circuits (ASICs)**

* Chips designed from scratch to mine bitcoin
* Very bad shipping times because the companies are funded from preorders
* Most boards become obsolete within 6 months, which is why shipping times are so bad. 
* There are now professional mining centres, located in places with cheap power, good network, and cooler climates. 

### Energy consumption

* **Landauer’s principle** states that any non-reversible computation must consume a minimum amount of energy. Each bit change requires $kT \ln2$ joules. SHA256 is not reversible, so energy consumption is inevitable.
* Energy aspects:
    * the embodied energy, which is the energy required to extract the materials to make the ASICs
    * electricity costs – decrease with scale
    * cooling – costs more with increases scale
* A bottom up analysis of bitcoin’s energy consumption (as of 22/6/17):
    * total network hashrate = 5000 THz
    * most efficient miner: 0.25 W/GHz
    * therefore 1250MW consumption
* A top down analysis (as of 22/6/17):
    * each block worth 25 * 2500 = 62500USD
    * one block every 10 minutes, therefore $104 per second
    * electricity costs $0.05 per kWh = $0.0139/MJ
    * assuming all miners just break even, this is 7488 MW
*  Thus in the grand scale of things bitcoin doesn’t consume that much energy. Also, it’s no different to traditional currency. 

### Mining pools

* Owning a mining rig does not guarantee that you will find a block – they follow a poisson distribution. 
* Mining pools are when many participants all attempt to mine a block with the same coinable recipient – sending any block rewards to a pool manager
* The pool manger distributes revenues bases on computational contribution (minus a cut). 
* Miners can prove probabilistically their computational contribution with **mining shares** – the idea is to demonstrate that you’ve produced “near-valid blocks” – with quite a few leading zeroes, but not enough. The rate at which you submit these near valid blocks should give the pool manager an indication of your work. 
* Some believe that this pooling should be built into bitcoin. 
* Advantages:
    * lower variance for individuals
    * more miners using updated validation software – easier hard forks?
* Cons:
    * leads to centralisation, some pools may end up monopolising the hash power
    * discourages miners from running full nodes

### Mining incentives and strategies

* Miners have several strategic decisions, that have defaults but may not be optimal depending on $\alpha$, the fraction of global hash power that you own
*  Which transactions should a block include?
    * by default, the one with the highest fees
    * if you have $\alpha>0.5$, you can make fork attacks by making a payment in exchange for goods, then building on a previous block where you send yourself the money instead. This attack is detectable, and the community can refuse to accept the longer chain. 
    * forking attacks via bribery have yet to be analysed – it would make sense for individual miners short term, but would destabilise bitcoin long term. 
* When do you announce blocks? 
    * by default, as soon as you find them
    * **block withholding attacks** involve not announcing the block you’ve found right away. You try to find two blocks, but withhold them. The rest of the network wastes hash power trying to find a block, but as soon as they publish it, you put your two blocks down (longest chain). 
    * if you only have one block by the time another is discovered, you must quickly push yours onto the network and hope it is accepted first, either by having good network connectivity, or by bribing others. 
    * with a 50% chance of winning network ‘races’, block withholding attacks are improved strategies for any $α > 0.25$ 

<br/>
## 5. Bitcoin community and politics

### Consensus

* Consensus on the rules:
    * what makes a transaction valid
    * what makes a block valid
    * protocols and formats
* Consensus on history:
    * agree on the contents of the blockchain – which transactions have occured and who owns what coins
* The consensus that coins are valuable

### The bitcoin core and the Bitcoin Foundation

* The bitcoin core is open source under the MIT license 
* Bitcoin improvement proposals (BIPs) are the main method for progress
* The lead developers can edit the defaults, but anyone can fork the software. In a centralised currency, you would have to exit if you disagree. But in bitcoin, the community remains power. 
* Any major forks in bitcoin are equivalent to new currencies being created.
* The Bitcoin Foundation pays the core developers out of its assets
* It is an entity that discusses with governments 
* There have been controversies:
    * board members who have become liabilities
    * people who believe bitcoin should go beyond governments
    * does the Bitcoin Foundation have any right to paint itself as the voice of bitcoin 

### The roots of bitcoin

* The main precursor is the cypherpunk movement, who believed in libertarianism and strong internet privacy using technology. One of the associated problems with this is digital money. 
* Satoshi Nakamoto wrote the bitcoin white paper in 2008, along with the bitcoin source code. 
    * Satoshi ‘speaks’ through public keys, which can be used to identify him
    * He owns a lot of bitcoins from early mining
    * He has been pretty much silent since 2010, except to say that he wasn’t this guy. 

### Bitcoin and governments

* Bitcoin contravenes **capital control** (untraceable cash flows), allowing people to move wealth without the government’s knowledge or control e.g China has tried to make it difficult to exchange 
* Bitcoin can facilitate illegal activities when combined with Tor. 
* However, it is very difficult to separate the virtual with the real world and stay anonymous for a long time – the creator of the Silk Road had 174000 BTC, but as soon as he tried to convert these to cash he’d be caught.
* Governments are very serious about anti money laundering – they search for structure in bitcoin transactions. 
* When markets fail, regulation can help to address the failure.
*  e.g the lemons market:
    * if consumers can’t tell the difference between a high quality (HQ) and low quality (LQ) good, they won’t pay extra for the HQ because they can’t trust the seller
    * then sellers won’t sell the HQ because it costs them more to manufacture 
    * this can be fixed with regulations, such as quality standards, required warranties, and required disclosure, all with enforcement. 
* The NYDFS proposed a BitLicense, in which to do anything with cryptocurrency you need this license. 

<br/>
## 6. The Bitcoin platform: other applications of the bitcoin blockchain

The blockchain is an **append-only log** – we cannot take stuff off the blockchain. This can be useful for various applications. However, in general, there is no way to prevent people from writing arbitrary (even illegal) data to the blockchain. 

### Secure timestamping

* We may want to prove knowledge of *x* (without initially revealing *x*) at some time *t*. 
* The blockchain already has some concept of ordinality in time. 
* We can do this by using hash functions, because $H(x)$$ is a **commitment** to *x*.
* However, you cannot prove clairvoyance because you could just hash every possible outcome and only reveal the correct ones
* If you send 1 satoshi to the hash of your data, you can get some message into the blockchain. However, this is frowned upon by the community as it costs the network. 

### Overlay coins

* Because the bitcoin blockchain allows us to write timestamped data into the blockchain, we can build a new currency on top of it. 
* All the relevant currency data can be written on top the blockchain
* Of course, the bitcoin miners aren’t validating any of this – so the currency needs its own rules.  
* e.g Mastercoin:
    * smart property and smart contracts, all on top of the blockchain
    * this versatility exists because the miners don’t have to understand it, they can just validate anything with out it affecting them. It is user-defined.
* However, this can be a lot more inefficient.

### Bitcoins as ‘smart property’

* Every bitcoin carries a history, which can be seen on the blockchain. 
* Therefore, bitcoins aren’t fully **fungible**, that is, perfectly interchangeable. One bitcoin may have more value than another one bitcoin. 
* However, because of these unique histories, we can give certain coins new meaning – e.g, they can become tickets for entry. 
* The metaphor that is often used is adding colour to the coin. 
* This is done by passing coins through a pay to script hash address which colours the coins. 
* **Coloured coins** can therefore be used to represent real world property, which could be traded on the blockchain 

Secure lotteries in bitcoin

* Alice, Bob, Carol each choose a random $x, y, z$. 
* Every party publishes a hash commitment of their random number. 
* Every party publishes their random number
* The winner is decided by finding $H(x \text{ XOR } y \text{ XOR } z) \mod 3$. 
* However, the problem with this is that the last person to submit their random number can first compute the winner, and refuse to participate if it isn’t them.  
* This can be fixed with timed hash commitments. Alice will sign a multisig with two possibilities
    * It pays Bob after a lock time of t. 
    * It sends the money back to Alice if the second signature is her random number x (which everyone can see on the blockchain). 
    * i.e after you sign up for the lottery, you will lose money for not participating. 
* The problems are that this is $O(n^2)$$, and that you must put up both a bet and a bond. 

### Prediction markets

* In bitcoin we can implement a mechanism to bet or hedge on certain results using smart contracts.
* In theory, a prediction market should reveal all of the current public information. 
* To make a decentralised prediction market,  we need:
    * decentralised payment and enforcement
    * decentralised arbitration
    * decentralised order book
* Decentralised payment and enforcement can be dealt with by using escrow transactions.
* We have no real way of improving over the current arbitration scheme – we still need a trusted party to arbitrate
* A centralised order book gives the opportunity for book makers to profit off the spread. We can change this by just giving the whole spread to the miner, which neutralises front-running. 

<br/>
## 7. Altcoins

### Alternative mining puzzles

Requirements for a mining puzzle: 

* cheap to verify
* adjustable difficulty 
* chance of winning proportional to hash power, but even small players can be compensated

**ASIC resistant puzzles**

* We might want ASIC resistant mining puzzles, in order to reduce oligopolies etc. 
* One strategy is to use **memory-hard puzzles**. Computer memory (RAM) has grown exponentially, but not as fast as processor speed. Therefore it levels the playing field.
* e.g **scrypt**:
    * constant time/memory tradeoff, meaning that it can be computed with a smaller amount of memory at the cost of time. 
    * scrypt is used in Litecoin. 
    * scrypt starts by filling a large block of memory with random value, where each memory entry is the hash of the previous one. Then, we read the values in a pseudorandom order, XORing and hashing as we go. The output is the result after N iterations. 
    * the problem is that checking also requires N steps and N memory. 
* e.g **cuckoo hash cycles**:
    * we have N nodes, and draw a line between two nodes cased on hashing an input X plus some i. We then ask if there is a cycle of size K, which is memory-hard. 

**Proof of useful work**

* Any work would have to be useful to the public and not the individual, otherwise it would equally promote attempted subversion. 
* It would be good if all the computational effort could be used for something that benefitted the public, like solving the problem of protein folding, but it is difficult to make these conform to the puzzle requirements. 

### Proof of stake

* Instead of mining with hardware, you mine by using the money that you would have spent on mining directly on the system. ‘Mining’ is sending money to a special address, then the system will distribute a reward based on the amount sent. 
* It is a form of **virtual mining**. 
* Lower overall costs, and no harm to the environment. 
* Stakeholders should have an interest in the stability of the currency. 
* POS reduces the risk of a 51% attacker, because you can’t just create hash power with fiat currency, you must first buy into the crypto. 
* Variations of virtual mining:
    * a proof of stake where the stake grows as a coin is unused – incentive to hold currency
    * proof of burn: when mining, you send coins to an unspendable address. 
    * proof of deposit: you ‘deposit’ a coin (temporarily unable to spend it). 
    * proof of activity: you have to be online to receive
 
### Some altcoins

* Namecoin, a decentralised replacement for the domain name system:
    * to maintain a domain, you send a Namecoin to the network. 
    * a transaction transfers ownership of a domain and namecoins 
* Litecoin, one of the largest alternatives (‘silver to bitcoin’s gold’). 
    * memory hard mining puzzle
    * 4x faster block rate compared to BTC. 
* Peercoin, which is a hybrid between proof-of-stake and proof-of-work 
    * you can mine by spending stake or by solving puzzles, but only the former will write onto the blockchain. 
    * peercoin admins regularly publish ‘checkpoints’ with their special public key. Good for safety, but not decentralised.
* Dogecoin, which has random block rewards
    * light-hearted culture oriented towards tipping and charity
    * the random block reward implementation was flawed, because the block bonus was a function of the previous block hash. Miners know the next reward and can switch their resources to mining another altcoin if the reward is low. 

### Metric for comparing altcoins

* Market cap (price * number of coins)
    * overestimate, because you can’t sell without moving the price
    * doesn’t account for lost coins
* Exchange volume
    * this can be moved deliberately by people changing coins between accounts
* Total hashpower
* Merchant support

### Interaction between bitcoin and altcoins

* A small mining pool on a large network can use their hash power to demolish an altcoin. 
    * e.g CoiledCoin was killed by a mining pool – they mined blocks that reversed transactions, or blocks that were blank. 
* **Merge mining** is when computations for one coin are also valid attempts for another coin. This is typically done by putting altcoin data into the bitcoin scriptSig field. 
* **Atomic cross chain swaps**:
    * at the start, A and B sign refunds
    * for A to receive B’s coin, she needs to reveal x before time t + 1. 
    * if B learns x before time t + 2, he can take A’s coin. 
    * either both transactions complete, or neither do. 

<br/>
## 8. The future of the blockchain and Bitcoin

### The blockchain as a vehicle for decentralisation

- **Smart property**, e.g car ownership
    * A car is controlled by a cryptographic key pair. It has the public key hard coded, and can be activated by sending a message to it that is signed by the private key. 
    * We can improve this by saying that the public key of the car is not just any public key, but the public key that is the receiving address of some bitcoin transaction. This means that you can sign ownership of the car over to someone else. 
    * **Representation** and **atomicity** are the two major considerations regarding decentralisation – can we represent an arbitrary statement/transaction, and can we ‘couple’ each party to guarantee security. 

### Ways that you can use the blockchain

* Use it directly, e.g with multisigs etc
    * easy to deploy
    * limited representation and atomicity, or at least tedious atomicity.  
* Using a block’s history (coloured coins). 
    * more complex representations possible
    * we get the security of the bitcoin blockchain 
    * limited atomicity
    * some people say that this puts unwanted bagging in the blockchain
* Merge mined side chains
    * avoids polluting the blockchain
* Alternate platforms such as Ethereum. 

### What can be decentralised?

* Purely digital things:
    * storage
    * lotteries
    * paying someone to prove that they know an x such that H(x) = c. 
* Things that can be represented digitally:
    * real world currency
    * stocks
* Complex contracts:
    * e.g interesting financial derivatives

### Is decentralisation a good idea?

* Decentralisation is basically an alternative to existing legal, social, and financial institutions.
* Security is made up of preventive, detective, and corrective controls. 
* Most real world security relies on the latter two: strong law enforcement. 
* With cryptocurrencies, there is a lot of emphasis on the preventive controls. 
* Dispute mediation is arguably better off with a centralised authority like a judge. 

































