---
layout: default
title: Ethereum whitepaper
---

## Next generation smart contract and decentralised application platform

*Buterin, V. (2014). A next-generation smart contract and decentralized application platform. Ethereum, (January), 1–36.*

*The Ethereum whitepaper, available on [GitHub](https://github.com/ethereum/wiki/wiki/White-Paper)**

### Bitcoin

- Satoshi solved distributed consensus by requiring proof of work from participants, as well as implementing a public ledger.
- Formally, a  ledger can be thought of as a set of **state transactions**: 

```APPLY(S, TX) -> S' or ERROR```

- Each transaction converts an old state into a new state or throws an error.
    - the state is the collection of **UTXO** (unspent transaction outputs), each UTXO having a denomination and owner.
    - a transaction maps one or more inputs (with signatures) to one or more outputs

> **for each** input **in** TX:
> - **if** input UTXO **not in** S: ERROR
> - **if** signature $\neq$ UTXO owner: ERROR
> 
> **if** $~\sum$ inputs $<\sum$ outputs: ERROR
> 
> **return** S - (input UTXO) + (output UTXO)

- Miners create blocks, which are valid if
    - previous block exists and is valid
    - timestamp > timestamp(previous)
    - valid proof of work
    - all transactions are valid
- An Eve of with >51% of the network's computational power could extend her own malicious forks
- The blockchain history is stored in a **Merkle tree**
    - a node can compare a block header with a small part of the tree, which reduces required storage size
    - the **simplified payment verification** (SPV) protocol allows for *light nodes* to verify POW on block headers and only download a small part of the tree. 
- To build a consensus protocol, you can either build a new network or build it on top of the bitcoin blockchain:
    - the former is difficult to implement and most applications will be too small to justify it
    - the latter is not scalable, as you cannot have 'light nodes'
- The Bitcoin scripting language that can:
    - create 'safety deposit boxes' that requires additional key(s) to open, via multisig
    - implement merchant escrow
    - support cross-cryptocurrency exchange
- However, it has important limitations:
    - not Turing-complete
    - lack of state: UTXOs are spent or unspent, which limits possibilities.
    - Blockchain-blindness: a UTXO cannot see the nonce and prevHash, which could be good sources of randomness. 

### Ethereum 

- Ethereum is a blockchain with a built-in Turing complete programming language
- The state in Ethereum is made up of **accounts**, with each account having a 20B address and containing four fields:
    - nonce
    - ether balance (the digitial currency used to pay transaction fees)
    - contract code
    - storage (empty by default)
- Accounts are either externally owned (controlled by private keys), or else they are **contract accounts** (controlled by their contract code)
- The Ethereum equivalent of a Bitcoin transaction is a **message**:
    - can be created externally or by a contract
    - can explicitly contain data
    - if the recipient is a contract account, they can return a response -> **messages can be used as functions**
- 'Transaction' in Ethereum refers to the signed data package that stores the message, the recipient, the signature, the quantity of ether, data, and two constants `STARTGAS` and `GASPRICE`. 
- **Gas** is Ethereum's solution to the Halting problem. You must pay to the miner a certain amount of ether per computation, measured in 'gas'
    - The limit of your payment is `STARTGAS`. 
    - If a transaction runs out of gas, state changes revert except for gas fees. 
    - Spare gas is returned to the sender 
    - this prevents infinite loops
- Contracts in Ethereum are created with a different transaction format.
- Contracts are **first class citizens**, capable of doing anything that an external individual can. 

`APPLY(S, TX) -> S'` works as follows:

> 1. Check if TX is well formed with a valid signature
> 2. Subtract `STARTGAS * GASPRICE` from sender and increment sender's nonce
> 3. Initialise `GAS = STARTGAS`and subtract a certain amount of gas per byte to pay for the size of the TX
> 4. Transfer the TX value to the receiving account. If it is a contract, run the contract and decrement `GAS` accordingly.
> 5. Miner collects all gas used, the rest is returned to the sender. 

### Code execution in Ethereum

- The basis of Ethereum is the **Ethereum Virtual Machine (EVM) code** 
- Code is an infinite loop (incrementing a counter) until STOP or RETURN is seen.
- Each byte represents an operation. These operations can access:
    - the **stack** (32B)
    - **memory**, an infinitely expandable byte array (both this and the stack reset after computation ends)
    - the contract's **storage**, a key/value store where any item can be 32B
    - value, sender, data, block-header
- The code can also return a byte array, which can represent data or more operations.

### The Ethereum Blockchain

- In addition to the transactions, blocks also contain:
    - the most recent state (not inefficient because only a small part of the state changes between transactions)
    - the block number and difficulty

The block validation algorithm:
> 
> 1. Check if the previous block exists and is valid
> 2. Check the timestamp, block number, difficulty, TX root, gas limit
> 3. Check the POW
> 4. Set `S[0] := STATE_ROOT` of the previous block (i.e no need to store blockchain history)
> 5. `APPLY(S[i], TX[i])` **for** `i=0,1,...,n-1`.
> 6. Set `S_FINAL:=S[n]` (plus a miner reward)
> 6. Check if `S_FINAL` is the same as the system's `STATE_ROOT`

### Applications of Ethereum

- Token systems
- Financial derivatives, which require a data feed contract that can be pinged when needed
- 'Decentralised dropbox': split the file and have your contract pay hosts as long as they can prove that they have the file
- **Decentralised Autonomous Organisations/Corporations/Communities** (DAO/DAC):
    - e.g only a 67% majority of members can move funds / modify code
    - there can be dividend-receiving shareholders and tradable shares
    - voting and liquid vote-delegation
- **Decentralised data feeds**: N parties input a datum, and anyone between the 25th-75th percentile gets a reward.
- Cloud computing: pay others to compute, with spotchecks built in.
- P2P gambling




