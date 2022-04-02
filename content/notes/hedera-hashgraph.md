---
title: "Hedera Hashgraph"
tags:
- crypto
- wip
disableToc: false
---

## HBAR
source: https://medium.com/@EidDany/hedera-hashgraph-vs-bitcoin-a-better-store-of-value-a0393fb2b822

- HBAR is the Cryptocurrency used on the Hedera network. HBAR is a scarce commodity (Only 50 Billion HBAR will ever be minted ), it’s a useful digital good , sometimes referred to as a “Utility Token”, and is used for two purposes, to protect the network and transfer value between the network participants.

- As shown in the latest release of the [Hedera’s Economics paper](https://www.hedera.com/hh-hbar-coin-economics-paper), we can track the following yearly supply schedule:

- The policies behind the HBAR supply schedule after 2025 have not been disclosed in advance by Hedera, and would probably be commensurate with actual TPS adoption figures, along additional security considerations as well. It would be interesting to observe the monetary policies implemented after 2025, and their impact over price valuation.

50 Billion HBAR total supply, 34% to be supplied by 2025, as follows:
- 2020 supply = 4,441,691,370 HBAR, an additional 9% from 2019  
- 2021 supply = 2,492,686,435 HBAR, an additional 5% from 2020  
- 2022 supply = 2,525,088,170 HBAR, an additional 5% from 2021  
- 2023 supply = 2,293,289,865 HBAR, an additional 4% from 2022  
- 2024 supply = 2,118,882,550 HBAR, an additional 5% from 2023  
- 2025 supply = 1,227,196,667 HBAR, an additional 2% from 2024

- Unlike the supply curve above, all Hbar coins have already been pre-minted at the genesis of the network and securely stored in advance in a multi-signature account

### Questions / Hesitations about HBAR

- Who are the owners of the multi-sig account that controls Hedera price distribution? I assume it is the Hedera Governing Foundation.
- So HBAR is premined with a max supply of 50 billion? What enforces the max? Asked another way, is there anything in the network governance structure that prevents an arbitrary change to the HBAR supply in the future?
- The Hedera network was launched in August 2018. At that time, the platform’s total fixed supply of 50 billion hbars was minted and placed into the Hedera Treasury account. The Hedera Treasury is a cryptographically secure, multi-signature account, and hbars can only be transferred out of it when a majority of the Council members sign a transaction; this ensures that management of the network’s native cryptocurrency is decentralized.
---
- Q: If the Hashgraph algorithm is proprietary, that means forking isn't allowed? If forking is technically illegal because of the patent, does that mean that Hedera network nodes are breaking the law if they don't upgrade to a non-backwards compatible version of the network, and a hard fork and a chain split occurs? Would an epic crypto legal battle between 2 Hedera networks ensue? :)
- A: Unlike other blockchain networks, Hedera ensures stability for end-users and application owners with a no-fork guarantee. The Hedera Governing Council achieves this through hashgraph's [open review code license](https://hedera.com/open-source).a
---
- That aspect of Hedera is interesting because in blockchain networks, the concept of "hard-forking" is relatively common and pretty inherent to the tech. Bitcoin has famously hard forked twice, as Bitcoin Cash and Bitcoin SV, and both networks still run today, albeit both are much smaller than Bitcoin by volume and market cap. Bitcoin tends to prefer soft forks, i.e., only release backwards compatible software updates to the network. Ethereum has also hard forked 10 times, they seem to have operationalized it well, people usually just abandon the old chain. But some straggler Ethereum chains still exist, like Ethereum Classic (source: [History of Ethereum Hard Forks](https://medium.com/mycrypto/the-history-of-ethereum-hard-forks-6a6dae76d56f)).
- I also admittedly don't understand the Hedera network very well. I get PoW and PoS networks but I don't understand how the hashgraph works or what it does differently compared to a blockchain. If you have any articles about that I'd definitely love to check them out.
- So, Hedera is a public permissioned network. That means anyone can use it's native currency, run smart contracts, etc. Does it mean that you can stake on it? So Google, IBM, all the Hedera Governing Council members run the consensus nodes that facilitate the networks transactions and manage network state. But you have to be invited to join. As the security, stability, and incentives of the Hedera network mature, the network will relax permissions by opening node operations to more entities and individuals.

Red flags: You can't run a node on Hedera unless you're invited, and most members are large multinational corporations. The idea behind this is to have a slow roll out of decentralization? These 39 invited node operators will all establish massive stakes of HBAR so as to be able to control the network in the future, even when additional nodes join the network. And this is so that  In a PoS network, the first 39 consensus nodes are chosen through invite only. the genesis of the network is important in a proof of stake system. 

Virtual voting and gossip about gossip: https://www.radixdlt.com/post/what-is-virtual-voting

By DAGs become centralized: https://www.radixdlt.com/post/dags-dont-scale-without-centralization

Blockchains are a DLT, but not all DLT's are blockchains. A simple way to view the distinction is that blockchains are linked lists, while DAG's are a tree of linked transactions, and both use different consensus mechanisms.

So Hedera is a DAG, not a blockchain. Like Iota and DAGcoin.

https://codeburst.io/scaling-crypto-dags-82363451e753