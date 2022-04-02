---
title: "Bitcoin"
tags:
- crypto
- wip
disableToc: false
---
# Introduction
In the very first block of Bitcoin that was mined, Satoshi posted a message referencing a story in the UK newspaper The Times. The message read: "**The Times 03/Jan/2009 Chancellor on brink of second bailout for banks**." Satoshi created Bitcoin in hopes to address systemic problems in the financial system.

There are problems with Bitcoin, some of which may be systemic. Will Bitcoin serve it's purpose of helping humanity and the world to be better off? It's a difficult question to answer, but with the United States Federal government indebted to the tune of [$30 trillion](https://usdebtclock.org/) dollars, one becomes curious about this thing called Bitcoin, a peer to peer decentralized financial system.

## How are transactions verified in Bitcoin?
Ownership of bitcoin is established through _digital keys_, _bitcoin addresses_, and _digital signatures_. The digital keys are not actually stored in the network, but are instead created and stored by users in a file called a _wallet_. The digital keys in a user’s wallet are completely independent of the bitcoin protocol and can be generated and managed by the user’s wallet software without reference to the blockchain or access to the Internet. Keys enable many of the interesting properties of bitcoin, including de-centralized trust and control, ownership attestation, and the cryptographic-proof security model.

Every Bitcoin address contains a public and private key, generated using the SHA-256 encryption algorithm, which is thought of to be impossible to reverse-engineer. The public key is used to ensure that you are the owner of an address that can receive funds. You need to have the private key for an address in order to spend from it.

SHA-256 is mathematical function that is practically irreversible, meaning that it is easy to calculate in one direction, but infeasible to calculate in the opposite direction (albeit, not impossible). Based on these mathematical functions, cryptography enables the creation of digital secrets and digital signatures that cannot be forged. Bitcoin uses elliptic curve multiplication as the basis for its public key cryptography.

In bitcoin, we use public key cryptography to create a key pair that controls access to bitcoins. The key pair consists of a private key and—derived from it—a unique public key. The public key is used to receive bitcoins, and the private key is used to sign transactions to spend those bitcoins.

There is a mathematical relationship between the public and the private key that allows the private key to be used to generate signatures on messages. This signature can be validated against the public key without revealing the private key.
## Incentives of Bitcoin 
- Miners are the engine of the Bitcoin network. For Bitcoin to continue to operate successfully into the future, blocks of transactions around ~1 MB in size are mined every 10 minutes. Mining as a process, is complex but I'll try to boil it down to one sentence. Mining is a process that consists of individuals around the world using computers to expend real electricity to run an encryption hash algorithm. Putting aside the debate about whether or not this process justifies it's energy usage, maybe you're wondering _why_ the hell Bitcoin requires so much energy in the first place?

- Incentives are what truly govern the Bitcoin network. If Bitcoin were no longer more valuable than the cost of energy associated with mining Bitcoin, it would cease to be mined. Blocks would stop being mined, transactions on the network would no longer process.

- If energy costs soared to all time highs due to inflation, and the majority of Bitcoin miners become unprofitable, wouldn't they stop mining? Bitcoin could break down.

- Why do the blocks have to be ~1 MB in size? Why can't we make each block 1 GB? As the block size and block throughput are increased, the size of the blockchain increases exponentially. This makes it more difficult for the average individual to run a Bitcoin nodes that stores a copy of the ledger. By running a node, you also hold a vote in the consensus rules for the version of the Bitcoin network that you run.

- The non-anonymity of the Bitcoin maintainers is a weak spot and easy target. The maintainers run Bitcoin Core, the main software client for a Bitcoin node. It's hosted on GitHub [here](https://github.com/bitcoin/bitcoin). This shouldn't be conflated with having _control_ over Bitcoin itself. Many people out there have mirrors and copies of the source code in the event that the main code repository is compromised. It's mostly low hanging fruit. Each node has individual say over whether or not to update to a new version of Bitcoin Core.

## Bitcoin Energy Usage
- While Bitcoin consumes a substantial amount of energy in it's own right, in relative terms, it is small. In 2021, the entire world used 154,620 TWh of energy. Bitcoin mining consumed 188 TWh, equal to 0.12% of global energy production.
- Bitcoin is powered by a [higher mix of renewable energy](https://bitcoinminingcouncil.com/wp-content/uploads/2021/10/2021.10.19-Q3-BMC-Presentation-Materials-Final.pdf) than any major country or industry. 
- 10,463 TWh total energy is used to generate & distribute electricity in the US. 6,800 TWh of that energy is wasted or lost. Bitcoin miners operate on small margins, and chase the cheapest source of energy globally, and this is often otherwise wasted energy.
- 65% of all energy used to generate & distribute electricity in the US is lost or wasted.
- Bitcoin does use a lot of electricity. But it is relative, for example, in 2021 Bitcoin used less energy than both the computer gaming industry and combined residential Christmas light energy consumption. One might argue that if this amount of electricity allows people living under authoritarian regimes to flee with their wealth or for people living under high inflation to preserve their wealth, this energy usage may be warranted.

## Mining Pools
How do Bitcoin mining pools work?

Are centralized mining pools a risk to Bitcoin?

What is Stratum V2?



## Bitcoin Privacy
- Bitcoin the ledger is not private at all. That's the intention. Without the global distributed ledger, nobody would be able to verify transactions.
- Coinjoin is a privacy tool that prevents this and there are different types of implementations, each with their own take on the same basic idea. Two or more users pool their UTXOs together into a collaborative transaction that is formed in a unique way. The way the transaction is constructed makes it very difficult for surveillance firms to know exactly which transaction output belongs to which of the input owners.
- This is where the separation part comes in. A proper coinjoin implementation will completely break all deterministic links with the coins 'pre coinjoin' past. At best anyone looking at the transaction can come up with a number of possible scenarios as to who owns which piece of bitcoin but they can never be 100% sure. Now imagine you carry out multiple rounds of coinjoin one after the other, the transaction graph quickly becomes very confusing and impossible to track.
- https://bitcoin-only.com/#privacy
- https://bitcoiner.guide/privacy/separate/

## Critiques of Bitcoin
### Bitcoin Transaction Fees
- Bitcoin transaction volume may not be enough to support the network's security budget in the long term. In 2024, the block subsidy will halve from 6.25 BTC to 3.125 BTC. It will take a long time for the subsidy to be zero, but in 2028 it will be 1.56 BTC. In 2032 it will be 0.78 BTC, then .3906 BTC in 2036. The majority of potential money for the entire economy of miner's can earn will someday have to come from transaction fees. If this model works, Bitcoin transactions transactions will be astronomically expensive to make on-chain. If other blockchains keep the demand for Bitcoin blockspace low enough, Bitcoin fees will remain low, and miners will have become unprofitable, and stop mining. This would cause the security budget to go down, making Bitcoin susceptible to a 51% attack.

### [51% Attack](51-attack.md)

## Additional Resources and Links
- [Clark Moody's Dashboard](https://bitcoin.clarkmoody.com/dashboard/)
- [Bitcoin vs. S&P 500 returns](https://strategy.com/)