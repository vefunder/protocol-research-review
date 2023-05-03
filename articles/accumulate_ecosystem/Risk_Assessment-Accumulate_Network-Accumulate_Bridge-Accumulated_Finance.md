To-do's (remove before publsihing)
- [x] Read over again & refine sections 
- [x] Resolve comments 
- [x] Review contracts of Accumualted Finance 
- [x] Format images
- [x] Grammar check
- [x] integrate wACME Dashboard
- [x] integrate anton clarifyications  
- [x] Check feedback wormhole gave you and see where applicable to this report 
- [ ] Upload to Github & Request review
- [ ] Answer gudiding questions (after review - in case more digging is needed)



====

# Risk Assessment: Accumulated Network, Accumulate Bridge & Accumulated Finance 

**General:**
- DefiDevs (Core Maintainer): https://defidevs.io/
- Defacto (Core Maintainer): https://de-facto.pro/
- Proposal: https://gov.curve.fi/t/proposal-to-add-wacme-frxeth-and-stacme-wacme-to-the-gauge-controller/9148
- Analyist Spreadsheet: https://docs.google.com/spreadsheets/d/1gDlENkhPkzSvdi73ySq75VJgcsrvBOlSlTPtrFwS9_k/edit?usp=sharing


**Accumulate Network**
- Webpage: https://accumulatenetwork.io/
- Docs: https://docs.accumulatenetwork.io/
- Whitepaper: https://accumulatenetwork.io/Accumulate-Whitepaper-4-12-22.pdf
- Factom: https://factom.pro/
- Block explorer: https://explorer.accumulatenetwork.io/
- Twitter: https://twitter.com/accumulatehq
- Discord: https://discord.com/invite/X74hPx8VZT
- Telegram: https://t.me/accumulatenetwork
- Youtube: https://www.youtube.com/@AccumulateNetwork
- Audit: https://fairyproof.com/doc/Accumulate-Blockchain-Audit-Report-091622.pdf


**Accumulate Bridge**
- Webpage: https://bridge.accumulatenetwork.io/mint
- Docs: https://docs.accumulatenetwork.io/accumulate/tutorials/bridge
- Github: https://github.com/AccumulateNetwork/bridge
- Audit: https://fairyproof.com/doc/AccumulateBridge-Audit-Report-092822.pdf

**Accumulated Finance**
- Website: https://accumulated.finance/
- Docs: https://docs.accumulated.finance/
- Twitter: https://twitter.com/AccumulatedFi

**Assets**
- ACME: https://beta.explorer.accumulatenetwork.io/acc/ACME
- wACME: https://dune.com/sigrlami/wacme-accumulate


### Relation to Curve

A [gauge proposal](https://gov.curve.fi/t/proposal-to-add-wacme-frxeth-and-stacme-wacme-to-the-gauge-controller/9148) has been put forward by Accumulated Finance for the [WACME/frxETH](https://curve.fi/#/ethereum/pools/factory-crypto-238/deposit) and [stACME/WACME](https://curve.fi/#/ethereum/pools/factory-v2-294/deposit) Curve pools. The goal is to create deep on-chain liquidity for the WACME token and exit liquidity for its stACME liquid staking derivative (LSD). Accumulated Finance has been incentivizing both pools with their WACME token since April 15th and claims they will transition incentives to Votium/Votemarket after a successful gauge vote. Gauge votes for both pools have recently passed on [April 30th, 2023](https://dao.curve.fi/vote/ownership/320) and [May 1st, 2023](https://dao.curve.fi/vote/ownership/321).


## Introduction to Accumulate Ecosystem

### Project History

Accumulate Network has a long project history that began as the Factom Protocol founded by [Paul Snow](https://www.linkedin.com/in/paulsn/) and [David Johnston](https://www.linkedin.com/in/davidajohnston/details/experience/) in 2014. Factom was conceived as an enterprise blockchain that would enable companies and organizations to keep transparent, high integrity data records. It was a grant recipient from the US Department of Homeland Security and partnered with the Bill and Melinda Gates Foundation. 

The Factom protocol improved Bitcoin's verification process by utilizing a unique chain-of-chains architecture that linked specific data chains for faster indexing and reduced computational costs. Factom also introduced innovations for more efficient scaling, such as the separation of token transactions and computation from the data layer, which allowed for continuous and real-time securing of data demanded by enterprise customers. With its competitive speeds, Factom forged several successful partnerships in both the public and private sectors. Accumulate Protocol was born out of the desire to preserve Factom's unique aspects while improving ease of use, mass appeal, and scalability.

The protocol [announced its intention](https://factom-council.medium.com/factom-becomes-accumulate-860ededf771f) to rebrand and upgrade to the Accumulate Network in November 2021, and officially [activated its mainnet](https://accumulatenetwork.io/2022/10/accumulate-mainnet-activation/) the following year. A timeline of significant events are as follows: 

- **2014**: [Factom](https://web.archive.org/web/20210624141324/https://www.factomprotocol.org/) is founded by Paul Snow and David Johnston in Austin, Texas.
- **April 2015**: Factom has one of the first blockchain token sales and raises 579 Bitcoin (~17,370,000 USD), worth $140,000 at the time.
- **Summer 2021**: [Inveniam Capital Partners](https://www.inveniam.io/) acquires Factom's 40 blockchain patents and key engineers Paul Snow and Jay Smith. It also forms the [DeFi Devs](https://defidevs.io/) subsidiary as a developer community for its projects.
- **November 2021**: Factom Authority Node Operators (ANOs) [unanimously vote](https://accumulatenetwork.io/2021/12/factom-validators-vote-unanimously-to-approve-upgrade-to-accumulate/) to rebrand and upgrade the Factom blockchain to the Accumulate blockchain.
-  **April 2022**: Accumulate [whitepaper](https://accumulatenetwork.io/2022/04/accumulate-whitepaper-release/) is released.
- **November 2022**: The Accumulate mainnet is officially activated.


### Team 

Development of Accumulate is headed by Factom lead engineers [Paul Snow](https://www.linkedin.com/in/paulsn/) and [Jay Smith](https://www.linkedin.com/in/jaysmithpmp/) through the community developement organizations. There are a number of organizations that can be considered core maintainers of the Accumulate Ecosystem, most notably: 

- [DeFi Devs](https://defidevs.io/) is a community developer organization for Accumulate Protocol. They are one of the primary contributors to the project, which is developed through open participation from the developer community. 

- [De Facto](https://de-facto.pro/) is a company that provides blockchain development and advisory services to startups. Their goal is to help startups navigate the complexities of blockchain technology and bring their solutions to market quickly and efficiently. De Facto can be considered core maintainers of Accumulate Network, but are also the core development team behind Accumulated Finance.

Accumulate [show on their website](https://accumulatenetwork.io/about/#story) a number of additional organizations that contribute to its development. 


### Brief Ecosystem Overview

|![](https://i.imgur.com/VtZokGd.png)|
|----|
|Conceptual Orientation of Accumulate Ecosystem & Interdependencies

The [Accumulate network](https://accumulatenetwork.io/) is a delegated proof-of-stake blockchain network organized around digital identities called Accumulate Digital Identifiers (ADIs). Other key features include a multi-chain architecture through the use of sub-chains, human-readable addresses, and user assignable key hierarchies for customizable security settings. 

ACME is its native asset (similar to ETH). Users can acquire ACME on the open market or earn it from staking. Usage burns the token and adds it to the token reserve where it can be issued again as staking rewards. The [Accumulate Bridge](https://bridge.accumulatenetwork.io/mint) enables users to bridge ACME from Accumulate to Ethereum where it is represented as the WACME ERC-20 token.

[Accumulated Finance](https://accumulated.finance/) is a DeFi application on Ethereum for staking WACME which allows the user to stake for stACME and earn yield. This allows users to reap the benefits of staking ACME without leaving Ethereum. Staking is liquid, so users can redeem their stACME for WACME at any time.


### Assets

The assets relevant for this report can be summarised as follows: 

| Asset | Description |
| --- | --- |
| [ACME](https://explorer.accumulatenetwork.io/acc/ACME) | ACME is the native token of the Accumulate Network. It is used to pay for transactions and services on the network, and it can be staked for network security and governance. |
| [WACME](https://etherscan.io/token/0xDF4Ef6EE483953fE3B84ABd08C6A060445c01170) | WACME is an ERC-20 token that is pegged 1:1 to ACME and can be minted by depositing ACME on the Accumulate Bridge, which is a bridge that allows users to transfer ACME and other Accumulate tokens between Ethereum and Accumulate. |
| [stACME](https://etherscan.io/token/0x7AC168c81F4F3820Fa3F22603ce5864D6aB3C547) |  stACME is an ERC-20 derivative token of ACME that represents staked ACME tokens on the Accumulate Network. It allows users to earn staking rewards while retaining the ability to use their staked tokens as collateral for other DeFi applications. |
| [ACFI](https://docs.accumulated.finance/accumulated-finance/acfi) | ACFI is the (currently unreleased) governance token of Accumulated Finance, a platform that allows users to participate in liquid staking and other DeFi services for the Accumulate protocol. |
| [FraxETH](https://defillama.com/protocol/frax-ether) | Frax Ether is a liquid ETH staking derivative designed to uniquely leverage the Frax Finance ecosystem to maximize staking yield and smoothen the Ethereum staking process for a simplified, secure, and DeFi-native way to earn interest on ETH.
| [FraxBP](https://mirror.xyz/0x290101596c9f85eB7194f6090a8c94fF5AAa32ca/vtNw6EvBjfqsRWi4YgmV6YJgFyU6Qg2Xjbc2_GchF6Q) |FraxBP is a base pool token on Curve Finance that serves as an alternative to 3CRV. It includes FRAX and USDC, and is used to provide liquidity for trading pairs involving those stablecoins on Curve. |


## Risk Assessment Scope and Specification

For the purpose of this report, we categorise risk into three risk vectors: 

| Risk Vectors | Definition | Guiding questions|
|-----------|------------|------------------|
| Censorship & Asset Security | Risks related to misappropriation of assets, including theft of funds, exit scams, and the possibility that users' assets may be seized, frozen, or otherwise restricted by an external authority or regulatory body. | (1) Is it possible for a single entity to rug its users?, (2) If the team vanishes, can the project continue?|
| Token-based Risk | This refers to risks introduced through the use of tokens, either through governance or token model | (3) Does the project viability depend on additional incentives?, (4) If demand falls to 0 tomorrow, can the users be made whole?|
|Technical Security| Risks related to the project's technical infrastructure, including risks related to the stability and security of the network. This includes risks related to software bugs, code vulnerabilities, and other technical issues that could compromise the project's functionality or security| (5) Do audits reveal any concerning signs|

While this report is primary focused on the proposal put forward by Accumulated Finance for the WACME/frxETH (or WACME/ETH) and stACME/WACME pools, the dependency on Accumulate Network requires a wider scope for our investigation.  

The remainder of the section will first cover risks associated with Accumulate Network, as it underlies the stACME and wACME pools put forward in the proposal. Secondly, the report will look at Accumulate Bridge, which is responsible for issuing wACME. Finally, we will take a look at Accumulated Finance, the issuer of stACME which is governed by ACFI. 


## Risk Analysis: Accumulate Network

### Product Introduction: Accumulate Network 

The Accumulate Protocol is an identity-based blockchain that aims to address the trilemma of security, scalability, and decentralization through a chain-of-chains architecture. Digital identities called Accumulate Digital Identifiers (ADIs) are treated as independent blockchains, and each possesses a hierarchical set of keys with different priority levels. The protocol uses a two-token system to provide predictable costs for enterprise users, and all transactions are anchored to Layer-1 blockchains for enhanced security. The goal is to power the digital economy through interoperability with Layer-1 blockchains, integrations with enterprise tech stacks, and interfacing with web APIs.


#### Block Validator Networks and Anchoring

The Accumulate protocol is designed to optimize parallel processing, linear scaling, and state efficiency. To achieve parallel processing, the network is partitioned into multiple validator networks, called Block Validator Networks (BVNs), that process transactions for a fraction of accounts. Each account is assigned to a particular BVN, and each BVN can process transactions for thousands of accounts. Additional BVNs can be added to linearly scale the network as usage increases.

|![System Overview](https://i.imgur.com/CRqXXMG.png)|
|-------|
|[System Overview](https://accumulatenetwork.io/Accumulate-Whitepaper-4-12-22.pdf)|


The efficiency of the state is realized through Accumulate’s chain-of-chains architecture that organizes transactions into hierarchies of summary hashes. This enables users to validate transactions on low capacity devices such as mobile phones. In a traditional blockchain, transactions are hashed using cryptographic algorithms like SHA-256 and organized into a data structure called a Merkle tree. The Merkle tree is stored within a block and connected to other blocks in the network by including the hash of the previous block in the current block header. This data structure creates a block ‘chain’ that maintains the history of the network.

In contrast, the Accumulate protocol treats each account as an independent chain and manages it as a continuously growing Merkle tree. Blocks are treated as synchronization points for all chains in the network. The indexing of these points allows Accumulate to provide 1-second blocks for finalization and 12-hour blocks for synchronization of the historical ledger.

|![Chains-of-chains architecture of Accumulate](https://i.imgur.com/bFJaChC.png)|
|-------|
|[Chains-of-chains architecture of Accumulate](https://accumulatenetwork.io/Accumulate-Whitepaper-4-12-22.pdf)|


Block Validator Network Nodes (BVNNs) validate transactions for an account. Root hashes from each BVN are fed into a larger network of nodes called the Directory Network (DN), which produces a final root hash that can be ‘anchored’ into another blockchain. DN root hashes will be regularly anchored into Layer-1 blockchains like Bitcoin and Ethereum.

Additionally, each BVN and the DN have Synthetic Transaction Chains, Root Anchor Chains, and one or more Intermediate Anchor Chains. Every chain is anchored into the Root Anchor Chain, creating a chain-of-chains architecture where every chain within a BVN or the DN can be considered a side chain of its respective BVN/DN Root Anchor Chain.


#### Accounts and Key Heirarchy

Each account is uniquely identified by a URL and is uniquely identified by its own Signature Chain and Main Chain. Accumulate accounts are designed to improve the user experience, efficiently organize data, and integrate with web apps, mobile devices, and enterprise tech stacks. Accumulate supports several types of accounts, including Lite Token Account, Lite Data Account, Accumulate Digital Identifier (ADI), Key Book, Key Page, ADI Token Account, ADI Data Account, and Scratch Account.

The protocol has a hierarchical key management structure where Key Books contain Key Pages and Key Pages contain keys that are authorized to sign transactions. Each Key Page specifies m of n where m is the number of unique, authorized signatures required to approve transactions (i.e., the signature threshold) and n is the total number of keys on the page. Accounts are linked at creation to a Main Key Book and an optional Manager Key Book. A signature is authorized only if the signing key corresponds to a key in one of the pages of either the Main Key Book or the Manager Key Book (if specified).

More information on the tech stack is available in the [Accumulate Whitepaper](https://accumulatenetwork.io/whitepaper/).


### Risk Vector 1: Censorship & User Assets Security

This section revolves around the security of user funds on Accumulate. For this, we based our analysis on metrics from this [Consensys research piece](https://consensys.net/research/measuring-blockchain-decentralization/) to determine the level of network decentralisation and therefore to assess the "rug-ability" (i.e. the extend of censorship resistance). 


#### Consensus Algorithm 

Accumulate Network uses a Delegated Proof of Stake System. In essence, this means that tokenholders delegate their ACME to specific validators which propose new blocks and validate transactions. Each Validator runs the [Tendermint Consensus Algorithm](https://docs.tendermint.com/v0.34/introduction/what-is-tendermint.html#consensus-overview). Tendermint is a Byzantine Fault Tolerant (BFT) consensus algorithm designed by [Jae Kwon (2014)](https://tendermint.com/static/docs/tendermint.pdf) to provide a high degree of security and consistency in a decentralized network. At a high level, the Tendermint algorithm works as follows:

1. Validators are responsible for proposing new blocks and verifying transactions. A set of validators is selected through a Byzantine fault-tolerant consensus algorithm that ensures a certain level of decentralization and security.
2. A new block is proposed by one of the validators, who must then broadcast the proposal to the rest of the validators.
3. Validators then vote on the proposed block. They can vote to approve, reject, or abstain.
4. If more than two-thirds of the validators approve the proposed block, it is added to the blockchain. Otherwise, the block is rejected.
5. Once a block has been added to the blockchain, the validators move on to the next block proposal and voting process.

It also supports finality, which means that once a block has been added to the blockchain, it cannot be reversed. The most popular and original use case is part of the [Cosmos SDK](https://tendermint.com/sdk/). Purely on an empirical basis, this suggests a degree of establishment and robustness. Thus far, it has been successfully running with a large amount of capital on the respective chain, suggesting a trustworthy and efficient consensus algorithm.

Due to the absence of more granular data provided by the Accumulate Network, the author is unable to assess stability beyond a heuristical reference to Tendermint.


#### Mining Rewards

Validators are nodes that are responsible for verifying and validating transactions, and adding them to the blockchain. According to the [whitepaper (p. 21)](https://accumulatenetwork.io/Accumulate-Whitepaper-4-12-22.pdf), stakers and validators are compensated with freshly minted ACME:

>  "Every year, 16% of tokens in the unissued pool will be minted at intervals of approximately 1 month to compensate stakers and validators in the absence of a transaction fee"

Accumulate validators earn [10% of staking rewards](https://explorer.accumulatenetwork.io/validators), while 90% goes to stakers. Therefore an approximate cost calculation for the security budget for a respective validator can be expressed as: 

```
Monthly Validator Reward = (16% * Total Unissued Tokens / 12) * 10% * (Validator Stake / Total Staked)
``` 

By doing a simple analysis, we see that of the 40 listed validators, only two make more than $2000 a month from pure validation of blocks and transactions.

|![](https://i.imgur.com/vP0Z8fF.png)|
|------|
|[Validation Rewards](https://docs.google.com/spreadsheets/d/1gDlENkhPkzSvdi73ySq75VJgcsrvBOlSlTPtrFwS9_k/edit?usp=sharing)|

Most validators self-stake as well, which makes their total reward significantly higher. Nevertheless, only six validators earn more than $2000 in USD value. 

|![](https://i.imgur.com/DxUW5Xy.png)|
|------|
|[Validation Rewards incl. Self-Staking](https://docs.google.com/spreadsheets/d/1gDlENkhPkzSvdi73ySq75VJgcsrvBOlSlTPtrFwS9_k/edit?usp=sharing)|

While this may seem like a low-security budget, it may be sufficient in the context of network activity, as Accumulate is essentially a new project. Given that ACME is relatively illiquid, an attacker will be unable to meaningfully cash out on a consensus level attack. This limits the risk at present, although additional liquidity in the Curve pools may increase this risk.


#### Stake Diversity & Growth

There are different types of agents listed as staking and validation participants. They are defined as follows: 

- CoreValidator: A validator node that is responsible for proposing new blocks and validating transactions on the Accumulate Network. CoreValidators are selected through a consensus algorithm and have a significant role in maintaining the security and integrity of the network.
- StakingValidator: A validator node that participates in the consensus process by validating transactions and adding them to the blockchain. Unlike CoreValidators, StakingValidators do not participate in block proposals, but they do earn rewards for their participation in the network.
- CoreFollower: A node that follows a CoreValidator in the network. These nodes are responsible for replicating data and participating in the consensus process.
- Delegate: An individual who holds tokens and delegates them to a validator in exchange for a share of the validator's rewards. Delegators do not participate directly in the consensus process, but they play an important role in securing the network by choosing trustworthy validators.

There are 17 coreFollowers, 24 coreValidators, 145 delegates, and 3 stakingValidators. 

|![](https://i.imgur.com/Ec7Xaxm.png)|
|------|
|[Count of Staker by Type](https://docs.google.com/spreadsheets/d/1gDlENkhPkzSvdi73ySq75VJgcsrvBOlSlTPtrFwS9_k/edit?usp=sharing)|

CoreFollowers hold the lowest number of tokens, at 20,496,467, while the coreValidators hold the largest amount at 73,088,162 tokens. Delegates hold 32,159,501 tokens, and stakingValidators hold 42,458,526 tokens. 

|![](https://i.imgur.com/ejAWyDP.png)|
|------|
|[Aggregated Balance of Stakers by Type](https://docs.google.com/spreadsheets/d/1gDlENkhPkzSvdi73ySq75VJgcsrvBOlSlTPtrFwS9_k/edit?usp=sharing)|

This shows a nearly inverse relationship which may suggest some disproportional power between different stakeholder groups.

*Note: In the following section on Token-based Risk we will elaborate further on centralised aspects of decision-making.*


#### Nodes

Due to a lack of available data about Accumulate Network, limited insight on node distribution can be provided. The current count of active nodes (i.e. coreFollower) as indicated in the previous section amount to 17. The author encourages similar infrastructure to [Etherscan Node Tracker](https://etherscan.io/nodetracker) to be deployed by Accumulate Network. 


#### Client Diversity

Based on the information provided, it seems there is a lack of available information on the client diversity of the Accumulate Network. Therefore, it is difficult to draw any definitive conclusions about the extent of client diversity. However, the limited information available suggests that there may only be one client available for the network. 


#### Network Usage

The absolute figures are unclear. It is not uncommon to see empty blocks, although it should be noted that the network operates on a 1-second block time. 

|![](https://i.imgur.com/c1zHIrG.png)|
|------|
|[Snapshot of latest transactions](https://explorer.accumulatenetwork.io/)|

Another measure of actual usage is the transaction volume of the native ACME token. The number of transactions of the token can be found on the [token page](https://explorer.accumulatenetwork.io/acc/ACME).

|![](https://i.imgur.com/4EgkDfL.png)|
|------|
|[Snapshot of Total ACME Transfers](https://explorer.accumulatenetwork.io/acc/ACME)|

At of this writing, the ACME token page displays 23191 transactions associated with it. Given that the Accumulate Network generated its Genesis block on 2022-10-31 20:11:52 [(see Block 1)](https://explorer.accumulatenetwork.io/), it has operated for approximately 6 months, which averages to (23191/6) = 3865 transactions involving ACME per month or 128 transactions per day. Relative to other blockchains, the activity is notably sparse (compare to this [Chain Comparison Dashboard](https://app.artemis.xyz/dashboard)). 

> *Note: Our analysis excludes function call diversity and growth, as well as Internet Service Providers (ISPs), due to the absence of meaningful information available*


#### User traction 

While social metrics are not a great tool for examining real user traction (as it can be easily manipulated), we nevertheless will consider it. Due to a lack of meaningful analytics, we will use this metric as a proxy to analyse the general interest of users in Accumulate. 

|![](https://i.imgur.com/cXvCzvm.png)|
|-----|
| [Twitter follower's @accumulatehq](https://www.coingecko.com/en/coins/wrapped-accumulate)|

As illustrated in the chart [from Coingecko](https://www.coingecko.com/en/coins/wrapped-accumulate), we can observe a decline of ~6% of Twitter followers (from ~175,000 to 165,000 since listing on the 10th of September, 2022 until the 6th of April, 2023).


#### Developer Activity

Developer activity refers to the level of contribution and engagement of developers in a software project. It can be measured by various metrics such as code commits, pull requests, bug fixes, and code reviews. Developer activity is a critical factor in the success and sustainability of software projects, as it drives faster development cycles, improved collaboration, and better quality software. In open-source projects, developer activity is particularly important, as it fosters community participation and collaboration, and helps drive the project's success. The reader should note that the main development of the Accumulate Network takes places on [GitLab](https://gitlab.com/accumulatenetwork/accumulate/) and GitHub acts as mirror.

|![](https://i.imgur.com/fixDUcr.png)|
|------|
|[Code frequency history of Accumulate Network Main Repository](https://github.com/AccumulateNetwork/accumulate/graphs/code-frequency)|

The developer activity for the main repo on GitHub is relatively consistent, suggesting sustained effort and sustainability of the project.

|![](https://i.imgur.com/SWJHoER.png)|
|------|
|[Commit Activity of Accumulate Network Main Repository](https://github.com/AccumulateNetwork/accumulate/graphs/commit-activity)|

Commits for this repo range somewhere between 5 to 10 commits per week. On a relative basis, this puts Accumulate on par with projects ranked 727 - 1001 in terms of weekly commits on the [Artemis Developer Activity Dashboard](https://app.artemis.xyz/developers).

    
#### Funding 

A substantial ecosystem fund is crucial for the success of a layer 1 blockchain. It provides resources to support growth and development, incentivizes participation, and covers costs when there are low adoption rates. Without such a fund, the network may not be able to pay transaction fees or support dApps. In addition, a large ecosystem fund attracts investors and developers, signalling a commitment to long-term success. It can also incentivize early adoption and provide grants for projects that contribute to the ecosystem's growth. Thus the aspect of funding is key.

Factom raised 579 Bitcoin in 2015. Relative to major recent launches, this is tiny. For instance, Sui raised [336,000,000 USD in Series A and B](https://cryptorank.io/ico/sui), Avalanche raised [248,000,000 USD across funding rounds](https://cryptorank.io/ico/avalanche) and Solana raised [334,000,000 USD](https://cryptorank.io/ico/solana). While Accumulate Network allocated a 60M ACME Grant block (~0.04x60,000,000 = 2,400,000 USD), the minimal funding is a concern as bootstrapping a layer 1 protocol is expensive.

The Accumulate team noted that Factom's tokenomics failed due to unlimited supply and inflation. [Inveniam](https://www.inveniam.io/) purchased all the patents and [invested several million dollars into Accumulate's development](https://accumulatenetwork.io/2022/05/factom-transition-to-accumulate-guide-faq/). An exact figure is not known. It's unlikely that the amount compares to funding of other layer 1 protocols.

Potential evidence of this discrepancy in funding can be seen in the lack of community engagement, marketing, and communication. An excerpt for a discord shows the community's frustration and lack of engagement.  

|![](https://i.imgur.com/NrQ6AOC.png)|
|-----|
|Discord Message Snapshot|


### Risk Vector 2: Token-based Risk

#### Tokenomics $ACME

As [outlined in the whitepaper](https://accumulatenetwork.io/Accumulate-Whitepaper-4-12-22.pdf), Accumulate's ACME token is designed to follow a Burn and Mint Equilibrium (BME) model. The token is used as a means of exchanging value within the Accumulate network, with Credits being used to pay for blockchain services. The Credits are fixed to the USD value, allowing enterprise users to budget their data use long-term without worrying about market conditions.

The BME model is designed to be deflationary, which incentivizes network use and staking. Periodically, ACME tokens are minted and distributed to stakers and validators as a reward for securing the network. At the same time, a portion of the circulating supply of ACME tokens is burned to create Credits, which are used to pay for blockchain services. Burned ACME tokens are returned to an unissued pool to be reissued in future blocks, creating a deflationary model that further incentivizes network use and staking. Initially, the unissued pool contains 300 million ACME tokens, which is 60% of the max supply.

Furthermore, every month, 1-2% of the tokens in the unissued pool will be minted as ACME is burned to create Credits. This ensures a steady supply of tokens for stakers and validators while maintaining the deflationary nature of the BME model.

In the first year, 100% of minted ACME tokens will be delivered to stakers and validators as a reward for securing the network. Later on, some of the minted ACME tokens will be added to the Grant Pool to support partnerships and development.

The [staking process](https://gitlab.com/accumulatenetwork/governance/governance-docs/-/blob/main/Governance.md#lockup-period) in Accumulate aims to lower token velocity and increase predictability, with a goal of 60%-80% of circulating supply being staked. There are two staking methods: undelegated staking, which has no penalties or bonding, and delegated staking, which has higher rewards but with warm-up and cool-down periods and penalties for poor performance. One can choose to delegate stake between 3 months and 24 months. The longer you stake the higher the rewards percentage will be. There will be short-term and long-term lockup implementation, and delegators and operators can be slashed if the operator is not performing or acting maliciously. To incentivize the robustness of the network in its early days, operators may operate multiple active core validators, but this is a temporary measure that may be discontinued in the future.

#### Initial Token Distribution of $ACME

The initial token distribution of $ACME is outlined in [this article](https://accumulatenetwork.io/2022/05/factom-transition-to-accumulate-guide-faq/): 

| Token Metrics | Value |
| --- | --- |
| Supply | 500M Tokens |
| Release at Mainnet | 150M |
| Grant Block | 60M |
| Dev Block | 90M |
| FCT Conversion to ACME | 50M |
| Conversion plus Activation Block | 200M of the 500M Max Supply |
| Uncirculated Pool | 300M tokens (60%) |
| Monthly Mint Rate | 1-2% of uncirculated pool tokens will be minted as ACME to create credits. |

The [Grant Block](https://accumulatenetwork.io/2022/04/accumulate-tokenomics-model-part-2/) will be given to community members for improving the protocol.

| Token Allocation | Amount |
| --- | --- |
| Governance | 30M |
| Core Development | 15M |
| Ecosystem Development | 6M |
| Business Development | 9M |

The [Developer Block](https://accumulatenetwork.io/2022/04/accumulate-tokenomics-model-part-2/) will be allocated directly or indirectly to shareholders, advisors, protocol contributors, and team members. 

| Token Allocation       | Amount      |
|------------------------|------------|
| DeFi Devs               | 79,200,000 |
| Unpaid Work Compensation | 3,600,000   |
| Advisors               | 1,800,000   |
| External Contributors  | 3,600,000   |
| Miscellaneous Expenses | 1,800,000   |

The initial allocation can be considered fair. In this distribution, DeFi Devs and Advisor can be considered protocol insiders putting the insider allocation to approximately 41% which on a relative basis can be considered low.


#### Token Ownership 

In Delegated Proof of Stake (DPoS), the delegated tokens play a crucial role in curating the validator selection process. DPoS networks are designed such that token holders can vote for delegates who will validate transactions and maintain the blockchain. In DPoS, the token holders' voting power is proportional to the number of tokens they hold or control through delegation. This means that token holders with a larger token balance or more delegated tokens have a greater say in the election of validators and hence the ordering of transactions and block construction. In addition, validators are rewarded with newly minted tokens and transaction fees for their efforts.

In essence, whoever controls more than 2/3 of the token stake effectively controls transaction validation and block construction in a Tendermint DPoS System.

|![](https://i.imgur.com/R8fo1vn.png)|
|------|
|[Cumulative Percentage Share of Validator Delgation (incl. Self-Staking)](https://docs.google.com/spreadsheets/d/1gDlENkhPkzSvdi73ySq75VJgcsrvBOlSlTPtrFwS9_k/edit?usp=sharing)|

From the chart, we can see that essentially two validators & their delegates controlled by 1) the DeFi Devs team ([acc://defidevs.acme](https://explorer.accumulatenetwork.io/acc/defidevs.acme)) and 2) the Accumulate Foundation ([acc://accumulate.acme](https://explorer.accumulatenetwork.io/acc/accumulate.acme)) effectively control the network. While not unusual for an early stage protocol, this represents a concerning level of centralization and a greater distribution of the token is needed. 

The staking mechanism has a positive feedback loop (assuming no slashing for technical failures of a given validator, etc.) that increases the balances of large token holders, as they are disproportionally more often selected for block validation and construction. It is likely to observe a circular effect of further entrenching the influence of early insiders, which may stifle future growth.

In the interest of decentralizing the influence of validators, stakers might reconsider the current distribution and delegate to a wider pool of validators. However, this may not be effective to adequately distribute vote power, as the majority of validator stake in both cases is self-staked (56,193,658 ACME and 42,000,000 ACME respectively). As shown below, when excluding delegation, the 2/3 threshold would be met by only 3 entities.

|![](https://i.imgur.com/X3lAXp1.png)|
|------|
|[Cumulative Percentage Share of Validator Self-Stake (excl. Delegation)](https://docs.google.com/spreadsheets/d/1gDlENkhPkzSvdi73ySq75VJgcsrvBOlSlTPtrFwS9_k/edit?usp=sharing)|

We recommend ideating about possible modifications to the validation mechanism. For instance, an option for balancing power more effectively for Accumulate Network might be achieved by implementing Quadratic Voting. For an example [see Axelar Network](https://axelar.network/blog/axelar-implements-quadratic-voting-with-maeve-upgrade). 

Furthermore, as only 45% of the total supply have been issued at present, it may be worth considering an alternative distribution mechanism to distribute to a larger token holder base. 

> *Note: Decentralisation is always a means to an end and not a goal in itself (i.e. most commonly blockchain systems are used to achieve the property of censorship resistance). Given this, the LP should ask him or herself whether the degree of decentralisation is sufficient for them in the context of Accumulate Network.*


#### Governance Processes

Accumulate's governance process is centered around [committees](https://gitlab.com/accumulatenetwork/governance/governance-docs/-/blob/main/Committees.md#committees) that manage critical workflows in the ecosystem. Committee members are selected by stakers and validators. There are four committes called the Governance Committe, the Core Development Committee, the Ecosystem Committee, and the Business Committee. Each is assigned a budget in ACME tokens to carry out operations. The committees manage tasks such as code development, managing the protocol, managing staking services, coordinating integrations with other communities, negotiating with exchanges, providing liquidity, and organizing marketing activities such as conferences and hackathons.

The [grant request process](https://gitlab.com/accumulatenetwork/governance/governance-docs/-/blob/main/Grants.md) is initiated by an applicant who submits a proposal to the appropriate committee. The committee evaluates the proposal and approves or rejects it. Stakeholder voting is required for all grant proposals, and decisions by the committee cannot be appealed. The committee may appoint independent consultants to monitor the execution of awarded grants. The initial Accumulate Grant Pool is allocated 60 million ACME tokens.

In general, the standard process can be summarised below:

|![](https://i.imgur.com/0dwRgtM.png)|
|----|
|High-level Overview: Grant proposal flow|


While Governance is fairly centralised, it is effective and well-documented, and seems appropriate given the current growth stage of the Accumulate Network. Initially, a core committee of individuals with technical expertise will make decisions about the protocol's implementation, but over time, node operators and delegators will have greater input in decision-making. Accumulate has plans to move towards a 100% [on-chain governance model](https://gitlab.com/accumulatenetwork/governance/governance-docs/-/blob/main/Governance.md#more-decentralization-over-time) over the next 2-5 years, with all decisions being made by all staking parties.  The community will eventually be able to vote on Accumulate Improvement Proposals and Grants for application/solution development. The move towards full decentralization is expected to be slower than the implementation of layer 2 grant proposal voting.


- [ ] where are fund of respective committees held?


### Risk Vector 3: Technical Security Risk

A series of [9 testnet releases](https://accumulatenetwork.io/technology/#roadmap) were deployed between November 2021 (when the upgrade to Accumulate was confirmed) and November 2022 (when the Accumulate mainnet was activated).

An [audit report by Fairyproof](https://fairyproof.com/doc/Accumulate-Blockchain-Audit-Report-091622.pdf) has been conducted between July 7, 2022 - Sep 13, 2022. We will highlight the most important issues raised by Fairyproof.


#### Consensus mechanisms are not fully implemented: [FP-1] DPoS Algorithm Not Implemented, [FP-2] No Slashing for Tendermint Validators 

The Accumulate Network has not implemented the DPoS algorithm. This changes the security properties of Accumlate and poses a potential risk to the network's security and integrity. At the time of the audit, the network seems to have been running on pure PoS. 

Additionally, the network's lack of a slashing mechanism poses a risk to users. If a validator engages in misconduct, there is currently no way to penalize them. However, the Accumulate team has acknowledged this issue and plans to implement a slashing mechanism to penalize such validators in future upgrades. Once implemented, evidence will be recorded on a data chain, and a layer 2 staking system will be responsible for discovering evidence and punishing misbehaving validators.  

After inquiring with the team, it was clarified that the DPoS and slashing will go live in May 2023.


#### Unresolved Attack Vectors: [FP-6] Potential Malicious BVN, [FP-15] Inappropriate Validator Power

The Accumulate Network has a potential security vulnerability in its consensus algorithm. When 2/3 of the validators in a single BVN are malicious, that BVN could become malicious and could potentially attack normal BVNs. This is particularly concerning when the number of validators in a BVN is small, as it could be easily manipulated and used to attack the entire system. 

To address this issue, the recommendation is to ensure that each BVN has at least a certain number of validators and to use a well-designed algorithm to randomly allocate validators for each BVN. At launch, each BVN will have at least 10 validators, spread across different mainnet participants. The Accumulate team plans to implement a more robust solution, including randomization, in version 1.1.

In addition, the power of newly added validators is always set to 1 in Accumulate Network's EndBlock function, which could pose a security risk. The recommendation is to implement a DPoS algorithm to calculate the actual power of the validator. The Accumulate team has acknowledged the issue, but no update has been provided yet. With the upgrade to DPoS in May 2023, the validator weight should change.


#### [FP-18] Reinforcing Token Security

The potential issue identified is that Accumulate's token design may have some issues. Currently, a token issuer only keeps its token's metadata in the BVN it belongs to and not the balance of the token holder's account. This implementation leads to issues such as the balance of a sender being verified only in the sender's BVN, but the receiver BVN cannot verify the sender's balance. 

The recommendation given is to consider implementing the logic in a way that a token issuer's BVN keeps all token holders' account balances, and when a token transfer is initiated, the token issuer's BVN will act as a relayer and verify the transaction. 

The update from the Accumulate team is that each account is auditable independently, and the cryptographic proofs of false transactions are small, making the protocol harder to compromise. They believe this makes the blockchain more decentralized and validates components independently. They plan to launch with a small set of trusted operators and randomly shuffle BVN assignments and active validator assignments in PoS v2 to make it harder for a cabal to gain control of a BVN. The Accumulate team has acknowledged this issue.


## Risk Analysis: Accumulate Bridge

### Product Introduction: Accumulate Bridge

|![](https://i.imgur.com/lUyFj7d.png)|
|-----|
|System Overview: Accumulate Bridge|
- [ ] What is the uptime requirement for Accumulate Node operators 
- [ ] What is the stake for a node operator 
- [ ] Slashing parameters etc.

The [Accumulate Bridge](https://docs.accumulatenetwork.io/accumulate/tutorials/bridge) is a decentralized bridge that allows users to transfer Accumulate tokens, including ACME, between Ethereum and the Accumulate blockchain. It uses a technology based on renBridge [Darknodes](https://docs.renproject.io/darknodes/), which act as gatekeepers between different blockchain networks to facilitate cross-chain transfers. At present there is no slashing implemented for bridge operators, meaning it run by a trusted set of nodes (i.e. PoA). Bridge transactions can be paused if misbehavior by any of the nodes is detected. Each bridge node operator has staked over 1.5 million ACME each, which is significant in relation to the total token supply of 200 million, although less so in dollar-terms (~$60k USD). 

The [bridge software](https://fairyproof.com/doc/AccumulateBridge-Audit-Report-092822.pdf) consists of the bridge node, bridge frontend, and smart contracts. The bridge node is a Golang backend application that integrates with the Accumulate blockchain and Ethereum. Bridge node operators set up multi-sig token accounts on both sides of the bridge that can only hold one type of token.

Users can initiate a transfer of Accumulate tokens from one blockchain network to another by sending their tokens to the bridge multi-sig token account. The memo field of the transaction is used to store the destination address for wrapped tokens on the destination blockchain network.

Once the transfer request is validated, the Accumulate Darknodes verify the transaction on both the source and destination blockchain networks. If the transaction is valid, the Darknodes sign a multi-sig contract to trigger the minting of wrapped tokens on the destination blockchain network.

On the Ethereum network, bridge operators parse burn transactions made by users, which releases native ACME tokens from the multi-sig token account on the Accumulate blockchain. Bridge operators mint wrapped tokens on the Ethereum network by generating and signing multi-sig mint transactions using the Gnosis Safe API.

The bridge also includes a fee module, which charges a flat fee without ACME price oracles. The fees collected are accumulated into Accumulate multi-sig token accounts.

Users can interact with the bridge via the bridge frontend, which allows them to initiate transfers of Accumulate tokens between Ethereum and Accumulate. The bridge node, bridge smart contracts, and the bridge frontend are open-source and released under the MIT license.

#### Note on WACME Adoption

The bridge has received modest usage since launch. WACME holders apparently consists mostly of the existing ACME holder base, rather than users onboarding from Ethereum. This may change as incentives to the WACME/frxETH pool grow WACME liquidity. The number of unique WACME holders has expanded since March, but as of this writing adoption remains very low.

|![](https://i.imgur.com/MzWn0oj.png)|
|-----|
|[WACME Holders Over Time](https://dune.com/sigrlami/wacme-accumulate)|


### Risk Vector 1: User Assets Security

At present, a [2-of-3 multi-sig](https://etherscan.io/address/0x76b1E2d258CC4297e7708345E5d99e8ECa967BB1#readProxyContract#F9) governs the Accumulate bridge and is required to initiate cross-chain transfers. While the validator set is small, they are not anonymous and are operated by companies. 

The node operator addresses are listed below:

| [Bridge Signer (Ethereum)](https://etherscan.io/address/0x76b1E2d258CC4297e7708345E5d99e8ECa967BB1#readProxyContract#F9)                                       |
|--------------------------------------------------------------|
| 0xCaD2fddb5009608Fa1ab042d643A61550A7dA63F                    |
| 0x9aB9c0d6741EaA6d6FDAf44c6EE80A2F9F59A8dB                    |
| 0x348975426678c0C0fCeD637b7Aa25C2900db986D                    |


| [Bridge Signer (Accumulate)](https://explorer.accumulatenetwork.io/acc/bridge.acme/validators/1)|
|---------------------------------------------------------------|
| MHz125tGb22fZtxjkViSDLmV5xeDmWRDgvZf39qbWMfbnDkYnJ8LLHT         |
| MHz126MJiCCPe4XSiS4byx5ydc5dk3aM4JAjU4odXvhpF9pwubvBqnU         |
| MHz1277ixNm9EZcLbCBpbw8SDYRddWfzHbXmHcEgbAShLY2mWp5N8xw         |

While the companies behind the bridge signers are not posted publicly, the Accumulate Team has stated that they can [inferred on-chain](https://explorer.accumulatenetwork.io/acc/8091107d12de06e1ec77c5622dd3c46e563b00c3cc06f8b50efde5e3693bf0ee@bridge.acme/1-ACME). 

Based on this, the Bridge Operators are [DeFacto](https://de-facto.pro/), [HighStakes](https://highstakesllc.org/) and [Kompendium](https://kompendium.co/). 


#### Note on Bridges

A user can lose funds even after bridging their asset if the underlying token is stolen. This is because the synthetic ERC-20 WACME token derives its value from the underlying ACME locked on the Accumulate network. If the underlying ACME token is stolen or compromised, the value of WACME will be affected, and depending on the severity of the compromise can potentially render it worthless. Therefore the wrapped ACME is entirely dependent on the integrity of its native network.


### Risk Vector 2: Token-based Risk

Not applicable as the bridge is not governed by a native token.


### Risk Vector 3: Technical Security

The Accumlate Bridge Infrastructure (i.e. [bridge frontend](https://github.com/AccumulateNetwork/bridge-frontend) and [bride contracts](https://github.com/AccumulateNetwork/bridge-contracts)) have be audited by [Fairyproof](https://fairyproof.com/). [The audit](https://fairyproof.com/doc/AccumulateBridge-Audit-Report-092822.pdf) does not show any meaningful unresolved security issues.

However, it is worth highlighting a recommendation mentioned in the report: under the current implementation, bridge validators complete a cross-chain transaction by using private keys stored locally in configuration files, which is not the best security practice. To improve security, it is recommended to use [Clef](https://geth.ethereum.org/docs/tools/clef/introduction) for signing transactions on an EVM chain and [Walletd](https://github.com/walletd/walletd) for signing transactions on the Accumulate network. These are programs that enable the signing of transactions without exposing private keys to the Internet. Clef and Walletd should be deployed independently from the bridge nodes, and access control should be managed by different individuals. This may increase maintenance costs, but it greatly enhances overall security by reducing the risk of private key compromise.


## Risk Analysis: Accumulated Finance

### Product introduction: Accumulated Finance

[Accumulated Finance](https://docs.accumulated.finance/general/about) is a platform that enables users to participate in liquid staking for the Accumulate protocol, similar to Lido for Ethereum. Users can easily stake any amount without worrying about the challenges and risks of managing staking infrastructure. Stakers receive stACME assets, which are liquid, tokenized staking derivatives representing their claim on the underlying stake pool and its yield. This allows users to stake their ACME tokens on the Accumulate protocol without leaving the Ethereum ecosystem. 

Accumulated Finance plans to use deeply liquid Curve pools to provide liquidity for WACME, the wrapped version of ACME on the Ethereum network. A pool paired with frxETH onboards Ethereum users to WACME and a pool paired with stACME given them access to ACME staking rewards.  

Node operators are added to Accumulated Finance through the multi-sig and are responsible for actual staking, maintaining the underlying infrastructure, and ensuring the security of the staked assets. Given that there is a token planned for Accumulated Finance ([$ACFI](https://docs.accumulated.finance/accumulated-finance/acfi)), it is likely that Governance over changes to the Node Operator configuration will eventually be subject to token voting, similar to [Lido](https://messari.io/report/liquid-staking-with-lido).  

|![](https://i.imgur.com/al8QryW.png)|
|-----|
|System Overview: Accumulated Finance|

When a user deposits WACME into the ACME Liquid Staking, deposits are then moved to Accumulate Bridge. From there, they are transferred to the burn address. The Bridge releases the ACME on Accumulate Network where it is staked by whitelisted node operators, and the user is issued stACME in return. A withdrawal event executes the process in reverse, as seen in the figure.


### Risk Vector 1: User Assets Security

The ACME liquid staking contracts are owned by a [2-of-3 multi-sig](https://etherscan.io/address/0xaBaEBBd34E7F79352F55B0Acea9516F6CDB94BB5) wallet composed of Accumulated Finance/Accumulate network team members. The signers include [Anton Ilzheev](https://www.linkedin.com/in/ilzheev/), who represents Accumulated Finance, [Paul Snow](https://www.linkedin.com/in/paulsn/), the Chief Blockchain Scientist of Accumulate Protocol, and [Ethan Reesor](https://www.linkedin.com/in/ethanreesor/), a Core Developer of Accumulate Protocol. These individuals are responsible for managing the multi-sig wallet and have specific admin rights, including ACME Liquid Staking contract ownership, the ability to set staking accounts for deposits, and mint stACME. All relevant contracts are non-upgradeable and there are no timelocks on admin functions.

Below is an overview of Accumulated Finance's liquid staked ACME contracts, contract ownership, and privileged functions:

**[ACMELiquidStaking Contract](https://etherscan.io/address/0xcf1A40eFf1A4d4c56DC4042A1aE93013d13C3217)** - Handles the logic of accepting WACME deposit, transferring to Accumulate Bridge, and minting stACME to users. 

- Owner is Accumulated Finance multi-sig
- `approve_bridge()` - approve bridge address for the WACME token
- `mint_stACME()` - mint stACME to an address
- `renounceOwnership()` - renounce ownership to ACMELiquidStaking
- `renounce_stACME_ownership()` - renounce ownership to stACME
- `set_stakingAccount()` - set Accumulate staking account
- `transferOwnership()` - transfer ownership to another address
- `transfer_stACME_ownership()` - transfer stACME ownership to another address

**[stakedACME Contract](https://etherscan.io/address/0x7AC168c81F4F3820Fa3F22603ce5864D6aB3C547)** - The stACME token contract.

- Owner is ACMELiquidStaking contract (which is owned by Accumulated Finance multi-sig)
- Function calls are made through the LiquidStaking Contract
- `pause()` - This function cannot be called, as the LiquidStaking contract doesnt support this function call. Either the LiquidStaking contract would need to be upgraded or stakedACME ownership transferred to use this function.

**[StakingRewards Contract](https://etherscan.io/address/0xe194d30aFDbae89b3118b8b7bc7B331Cc3333b88)** - Users can stake their stACME and earn staking rewards.

- Owner is [Accumulated Finance: Deployer](https://etherscan.io/address/0x5503e1185c486c327b999b094b151474c1b9bb1f) (EOA)
- `setRewardsDuration()` - set duration of rewards to be paid out (in seconds)
- `notifyRewardAmount()` - set the reward amount for the next reward period


#### Risk Vector 2: Token-based Risk 

Governance as of now, seems to be undefined for Accumulated Finance. However, the [fee page](https://docs.accumulated.finance/accumulated-finance/fees) suggests that a veModel will be utilised.   

Based on the information provided on that page, one can imagine, a potential veTokenomic model for Accumulated Finance could involve the use of veACFI staking to incentivize participation in the platform. Users who stake their ACFI tokens could earn a portion of the fees generated by the platform, including the 8% fee from the ACME Liquid Staking Fee Structure and the 4% fee from the WACME LP Incentives Fee Structure. The amount of the staking reward as well as the decision-making power will most likely be determined by the lock-up to which the user commits.

To encourage liquidity in the stACME/WACME Curve pool, the platform could allocate 8% of the fees generated by the ACME Liquid Staking Fee Structure towards incentivizing liquidity providers. These incentives could be in the form of additional stACME or other rewards.

In addition, the Treasury Fee Structure could be used to fund ongoing development and operations of the platform. A portion of the fees generated by the platform, including the 4% fees from the Treasury Fee Structure, could be allocated to the Treasury to support the platform's growth and sustainability.

Inquire with the team made it clear that they retain the right to launch a token and will only do so if it makes sense. A healthy comment shared by the team is shared below:

|![](https://i.imgur.com/AwDmAAT.png)|
|----|
|Communication with the team in Telegram|

#### Initial Token Distribution of $ACFI

To date there is not much information available on the token apart from that (1)  ACFI token holders will be entitled to 50% of the protocol revenue (2) that 1% of the total ACFI supply will be distributed to early adopters through several rounds of distribution. Early bird liquid staking users who deposit ACME or WACME to the liquid staking before 9 March 23:59, 2023, will be able to participate. The ACFI distribution will be based on a pro-rata basis, determined by the amount and duration of the deposit. The network will distribute ACFI to early birds in a calculated manner, based on their virtual points. The early birds' stACME/WACME liquidity providers will be announced at a later stage.

#### Risk Vector 3: Technical Security
The code has not been audited. 


## LlamaRisk Gauge Criteria
1. Is it possible for a single entity to rug its users?
4. If the team vanishes, can the project continue?
6. Does the project viability depend on additional incentives?
8. If demand falls to 0 tomorrow, can the users be made whole?
10. Do audits reveal any concerning signs?

### LlamaRisk Recommendation
 





### Contract glossary

| Protocol | Name                                | Address                                                                                      | Description                                                                                                                                                                                                                                           |
|----------|-------------------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ACFI     | Gnosis Safe Multisig Admin          | [0xaBaEBBd34E7F79352F55B0Acea9516F6CDB94BB5](https://etherscan.io/address/0xaBaEBBd34E7F79352F55B0Acea9516F6CDB94BB5)        | The Gnosis Safe Multisig Admin is a smart contract used by Accumulated Finance to manage multisig wallets for the platform. Specifically, this contract is used for the Accumulated Finance multisig wallet.                                                                  |
| ACFI     | ACME Liquid Staking (Deposits)      | [0xcf1a40eff1a4d4c56dc4042a1ae93013d13c3217](https://etherscan.io/address/0xcf1a40eff1a4d4c56dc4042a1ae93013d13c3217)        | The ACME Liquid Staking (Deposits) contract is used by Accumulated Finance to manage the staking of ACME tokens in a way that allows them to be used for other purposes while still earning rewards.                                                                                                                         |
| ACFI     | ACME Liquid Staking (Rewards)       | [0xe194d30aFDbae89b3118b8b7bc7B331Cc3333b88](https://etherscan.io/address/0xe194d30aFDbae89b3118b8b7bc7B331Cc3333b88)        | The ACME Liquid Staking (Rewards) contract is used by Accumulated Finance to distribute rewards to users who stake their ACME tokens in the liquid staking pool managed by the platform.                                                                                                                                  |
| Curve     | stACME/WACME            | [0x4424b4A37ba0088D8a718b8fc2aB7952C7e695F5](https://etherscan.io/address/0x4424b4A37ba0088D8a718b8fc2aB7952C7e695F5) | The stACME/WACME  contract is used by Accumulated Finance to incentivize liquidity providers in the stACME/WACME pool. Users who provide liquidity to this pool can earn rewards in the form of stACME or other tokens.                                                                                         |
| Curve     | WACME/frxETH                        | [0x7bbE7a17E501BB6542e975D8A163950149487fa3](https://etherscan.io/address/0x7bbE7a17E501BB6542e975D8A163950149487fa3)        | The WACME/frxETH contract is used by Accumulated Finance to manage the exchange of WACME and frxETH tokens. Users can trade between these tokens through this contract, which enables them to take advantage of different features and benefits associated with each token. |
| ACFI | ADI               | [accumulated.acme](https://explorer.accumulatenetwork.io/acc/accumulated.acme) | ADI is the native token of the Accumulate Network blockchain. It is used for various functions within the platform. |
| ACFI | ACME Staking Account         | [accumulated.acme/staking](https://explorer.accumulatenetwork.io/acc/accumulated.acme/staking)      | The ACME Staking Account is a smart contract used by Accumulate Network to manage staking of ACME tokens.                                                  |
| ACFI | Staking Rewards       | [accumulated.acme/staking-rewards](https://explorer.accumulatenetwork.io/acc/accumulated.acme/staking-rewards)        | The Staking Rewards contract is used by Accumulate Network to distribute rewards to users who stake their ACME tokens on the platform.|
| ACFT | Incentives           | [accumulated.acme/incentives](https://explorer.accumulatenetwork.io/acc/accumulated.acme/incentives) | The Incentives contract is used by Accumulate Network to incentivize users to provide liquidity to different pools on the platform. |
| ACFI | Treasury            | [accumulated.acme/treasury](https://explorer.accumulatenetwork.io/acc/accumulated.acme/treasury) | The Treasury is a public address used by Accumulate Network for various functions, including receiving a portion of the protocol fees. |
| Accumulate Bridge    | Accumulate Bridge Contract         | [0xbA050938970C8eAeDA3e970B571a6fe463Db7d0e](https://etherscan.io/address/0xbA050938970C8eAeDA3e970B571a6fe463Db7d0e#writeContract)      | The Accumulate Bridge Contract is a smart contract that enables cross-chain transfers of ACME tokens between Ethereum and Accumulate blockchains.|
| Accumulate Bridge    | Accumulate Bridge Multi-sig      | [0x76b1E2d258CC4297e7708345E5d99e8ECa967BB1](https://etherscan.io/address/0x76b1E2d258CC4297e7708345E5d99e8ECa967BB1#readProxyContract)       | The Accumulate Bridge Multi-sig is a secure way for bridge operators to manage and authorize cross-chain token transfers. |
| General    | Accumulate Deployer      | [0x5e95454F33F9C88C9a967531CD48192dEb2Fd4fd](https://etherscan.io/address/0x5e95454f33f9c88c9a967531cd48192deb2fd4fd)         | Accumulate Deployer is used to deploy and manage smart contracts on the Accumulate blockchain network.                |
| General              | Wrapped ACME (wACME) |[0xDF4Ef6EE483953fE3B84ABd08C6A060445c01170](https://etherscan.io/address/0xDF4Ef6EE483953fE3B84ABd08C6A060445c01170)| Wrapped ACME (wACME) is a token that represents ACME on the Ethereum blockchain, allowing it to be used in decentralized finance (DeFi) applications.|
| General    | stACME | [0x7AC168c81F4F3820Fa3F22603ce5864D6aB3C547](https://etherscan.io/token/0x7AC168c81F4F3820Fa3F22603ce5864D6aB3C547)| stACME is a liquid staking derivative token of wACME, representing staked ACME in a liquid form. |


========= Ideas to add to the report (with more time) =======
### Possible Extension of the analysis - remove before publishing
- [ ] Enhance analytics by building dashbaord for Bridge & Accumalated Finance  (requires scraping form block explorer)
- [ ] Simulate Centralisation of Token supply over time  (basic cadCAD simulation)
```
Staking Reward = (16% * Total Unissued Tokens / 12) * 90% * (Stake / Total Staked)
``` 

## Risk Mitigation Checklist
- [ ] Simulate Token economy 
- [ ] Reconsider token distriution mechanism 
- [ ] Set greater focus on ecosystem growth & communication
- [ ] Analytics tool!!! Very hard to get concret information 


Comment
- Grant proposals process - well done! Feel highly apporipriate given the stage of accumulate and well documented
