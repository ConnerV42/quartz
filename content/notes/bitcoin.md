---
title: "Bitcoin"
tags:
- crypto
disableToc: false
---

# Introduction
On January 3rd, 2009, the first Bitcoin block was mined by Satoshi Nakamoto, the pseudononymous creator of Bitcoin. Inscribed within the block was a message. It read: "**The Times 03/Jan/2009 Chancellor on brink of second bailout for banks**."

The United States is indebted to the tune of [$30 trillion](https://usdebtclock.org/) dollars. Bitcoin's success since its inception is a fascinating emergent response to the growing instability of the American financial system. This is a living document in which I try to make sense of what Bitcoin is on a technical, economic, environmental and political level.

- [Proof Of Stake](/notes/proof-of-stake.md) 

## Bitcoin Energy Usage
 Bitcoin, as a decentralized network, is resilient to [51% attacks](/notes/51-attack.md) in large part because of it's energy usage through a process called [Proof of Work](/notes/proof-of-work.md). The electricity usage of the Bitcoin network, while jarring, is intentional to the design of the system. A measurement of the total hashing capability of the network is taken bi-weekly. That measurement is used as an input to calculate the _difficulty target_, or the amount of electrical work required to produce a valid Proof of Work block over a 10 minute period. This bi-weekly re-targeting is called the _difficulty adjustment_, and exists to artificially slow the production of Bitcoin blocks across time. This fundamentally limits the transaction throughput capability of the Bitcoin network because each transaction needs to be recorded into a block, and block space is finite, and capped at around 1 MB.
- Bitcoin consumes a substantial amount of energy. In 2021, the entire world used 154,620 TWh of energy. Bitcoin mining consumed 188 TWh, equal to 0.12% of global energy production.
	- Bitcoin is powered by a [higher mix of renewable energy](https://bitcoinminingcouncil.com/wp-content/uploads/2021/10/2021.10.19-Q3-BMC-Presentation-Materials-Final.pdf) than any major country or industry.
	- 10,463 TWh total energy is used to generate & distribute electricity in the US. 6,800 TWh of that energy is wasted or lost. Bitcoin miners operate on small margins, and chase the cheapest source of energy globally, and this is often otherwise wasted energy.
	- 65% of all energy used to generate & distribute electricity in the US is lost or wasted.

## How are transactions verified in Bitcoin?
Ownership of bitcoin is established through _digital keys_, _bitcoin addresses_, and _digital signatures_. The digital keys are not actually stored in the network, but are instead created and stored by users in a file called a _wallet_. The digital keys in a user’s wallet are completely independent of the bitcoin protocol and can be generated and managed by the user’s wallet software without reference to the blockchain or access to the Internet. Keys enable many of the interesting properties of bitcoin, including de-centralized trust and control, ownership attestation, and the cryptographic-proof security model.

Every Bitcoin address contains a public and private key, generated using the SHA-256 encryption algorithm, which is thought of to be impossible to reverse-engineer. The public key is used to ensure that you are the owner of an address that can receive funds. You need to have the private key for an address in order to spend from it.

SHA-256 is mathematical function that is practically irreversible, meaning that it is easy to calculate in one direction, but infeasible to calculate in the opposite direction (albeit, not impossible). Based on these mathematical functions, cryptography enables the creation of digital secrets and digital signatures that cannot be forged. Bitcoin uses elliptic curve multiplication as the basis for its public key cryptography.

In bitcoin, we use public key cryptography to create a key pair that controls access to bitcoins. The key pair consists of a private key and—derived from it—a unique public key. The public key is used to receive bitcoins, and the private key is used to sign transactions to spend those bitcoins.

There is a mathematical relationship between the public and the private key that allows the private key to be used to generate signatures on messages. This signature can be validated against the public key without revealing the private key.

## Incentives of Bitcoin 
- Incentives are what govern the Bitcoin network. If the majority of miners ever found themselves paying more in electricity costs than the value they earn from their Bitcoin, they would simply quit mining. In that scenario, Bitcoin could be left vulnerable to a 51% attack.

- Why is the block size ~1 MB in size? Why haven't block sizes of 1 GB been tried? Well, they have. Just not in Bitcoin specifically. Increasing block size and throughput have the impact of increasing the blockchain increases exponentially.
- A large blockchain size makes it difficult for the average individual to run a Bitcoin node. Bitcoin nodes enforce the consensus rules. The sheer amount of Bitcoin nodes makes it difficult makes changes to the consensus rules - unless everyone agrees. Many Bitcoin nodes store full copies of the blockchain history, which as of this writing is still well under a terabyte.

- The non-anonymity of the Bitcoin maintainers is a weak spot and easy target. The maintainers run Bitcoin Core, the main software client for a Bitcoin node. It's hosted on GitHub [here](https://github.com/bitcoin/bitcoin). This shouldn't be conflated with having _control_ over Bitcoin itself. Many people out there have mirrors and copies of the source code in the event that the main code repository is compromised. It's mostly low hanging fruit. Each node has individual say over whether or not to update to a new version of Bitcoin Core.

## Bitcoin Privacy
- Bitcoin the ledger is not private at all. That's the intention. Without the global distributed ledger, nobody would be able to verify transactions.
- Coinjoin is a privacy tool that prevents this and there are different types of implementations, each with their own take on the same basic idea. Two or more users pool their UTXOs together into a collaborative transaction that is formed in a unique way. The way the transaction is constructed makes it very difficult for surveillance firms to know exactly which transaction output belongs to which of the input owners.
- This is where the separation part comes in. A proper coinjoin implementation will completely break all deterministic links with the coins 'pre coinjoin' past. At best anyone looking at the transaction can come up with a number of possible scenarios as to who owns which piece of bitcoin but they can never be 100% sure. Now imagine you carry out multiple rounds of coinjoin one after the other, the transaction graph quickly becomes very confusing and impossible to track.
- https://bitcoin-only.com/#privacy
- https://bitcoiner.guide/privacy/separate/

## Bitcoin Transaction Fees
- Bitcoin transaction volume may not be enough to support the network's security budget in the long term. In 2024, the block subsidy will halve from 6.25 BTC to 3.125 BTC. It will take a long time for the subsidy to be zero, but in 2028 it will be 1.56 BTC. In 2032 it will be 0.78 BTC, then .3906 BTC in 2036. The majority of potential money for the entire economy of miner's can earn will someday have to come from transaction fees. If this model works, Bitcoin transactions transactions will be astronomically expensive to make on-chain. If other blockchains keep the demand for Bitcoin blockspace low enough, Bitcoin fees will remain low, and miners will have become unprofitable, and stop mining. This would cause the security budget to go down, making Bitcoin susceptible to a 51% attack.

## Updates to Bitcoin
- Bitcoin generally updates through "soft-forks", which means that every future network update must be backwards compatible. Hard forks, on the other hand, always create a chain split, the old chain is often abandoned, which Ethereum has managed to operationalize well. Ethereum has hard forked 10 times (see: [History of Ethereum Hard Forks](https://medium.com/mycrypto/the-history-of-ethereum-hard-forks-6a6dae76d56f)). Soft forks can be thought of as simple "network upgrades", while hard forks literally create 2 networks, the nodes on one network running the new, non-backwards compatible software, while the nodes on the old network are still running the previous, incompatible version of software.

## Additional Resources and Links
- [Clark Moody's Dashboard](https://bitcoin.clarkmoody.com/dashboard/)