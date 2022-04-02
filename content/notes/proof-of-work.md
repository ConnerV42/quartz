---
title: "Proof of Work"
tags:
- crypto
- wip
disableToc: false
---

## Benefits & Issues of Proof of Work
There's a difference between users and miners (also known as block validators). Anyone can use a blockchain to make transactions or execute smart contracts. That's a user. A miner's purpose is to validate blocks of transactions. are incentivized to do so because they can earn a monetary reward. If you have a basic understanding of Bitcoin, you know that a new block is mined roughly every 10 minutes. A block is not unlimited in size in blockchain networks. With Bitcoin, for example, block size is capped at ~1 MB. It has to be a small block for a few reasons. decentralization, node validators, total size of blockchain exponentially grows as block size increases. When a new block is mined, it is added onto the end of the chain, hence the term blockchain.

[Proof of Stake](/notes/proof-of-stake.md) protocols are a class of consensus mechanisms for blockchains that work by selecting validators in proportion to their quantity of holdings in the associated cryptocurrency.

Proof of Stake (PoS) gives mining power based on the percentage of coins held by a miner.

PoW and PoS are the only network consensus mechanisms that I know of that work in practice. Someone could naively suggest a proof of user consensus, where one user equals one node in the network, but that doesn't work either, because nothing is stopping bad actors from provisioning N number of nodes on AWS, and boom, your network just forked because of a 51% attack.

Tying network consensus to real world electricity consumption, like BTC and ETH do, is problematic because of the carbon emissions caused by it. But it does solve the problem of achieving network consensus quite well, and it does it in a decentralized way.