# Risk Assessment: Metronome


## Context 


### Useful links:

Metronome Synth Protocol:

* Metronome Documentation: [https://docs.metronome.io/](https://docs.metronome.io/) 
* Quantstamp Audit: [https://github.com/autonomoussoftware/metronome-synth-audit](https://github.com/autonomoussoftware/metronome-synth-audit) 
* Telegram: [https://t.me/metronometoken](https://t.me/metronometoken) 
* Discord: [https://discord.gg/metronome](https://discord.gg/metronome) 
* Github: [https://github.com/autonomoussoftware](https://github.com/autonomoussoftware) 
* Snapshot: [https://snapshot.org/#/metronome.eth](https://snapshot.org/#/metronome.eth)
* LlamaRisk Dune Dash by DiligentDeer: [https://dune.com/diligentdeer/metronome-llamarisk](https://dune.com/diligentdeer/metronome-llamarisk) 

Metronome V1 for historical context: 

* Prime Rating Report on Metronome V1: [https://primerating.mypinata.cloud/ipfs/QmTAHVmLMTEFpmKSAzsrw13af2SaomaDPMtMYn6ALR3TYK](https://primerating.mypinata.cloud/ipfs/QmTAHVmLMTEFpmKSAzsrw13af2SaomaDPMtMYn6ALR3TYK)
* Defi Safety Risk Assessment: [https://defisafety.com/app?title=metron](https://defisafety.com/app?title=metron) 


## Relation to Curve  

In January 2023, Metronome submitted a [proposal](https://gov.curve.fi/t/proposal-to-add-msusd-fraxbp-mseth-eth-to-the-gauge-controller/8608) to the Curve governance forum requesting to add gauges for the [msUSD/FraxBP](https://curve.fi/#/ethereum/pools/factory-v2-251/deposit) and [msETH/ETH](https://curve.fi/#/ethereum/pools/factory-v2-252/deposit) pools. The proposed gauges received approval from the community [here](https://dao.curve.fi/vote/ownership/255) and [here](https://dao.curve.fi/vote/ownership/256). Ascertaining the potential risks to Curve's liquidity providers from assets in Curve pools is an essential objective of the LlamaRisk Team. In pursuit of this objective, the LlamaRisk Team has been conducting research on the Metronome protocol.


## Introduction to Metronome 

Metronome is an Ethereum-based DeFi platform for synthetic assets. By crypto standards, it has been around a long time — more than four years. The project has evolved substantially in that period.


### Metronome 1.0 (2018-2022)

The initial Metronome product suite launched in 2018. Metronome is an offshoot of Bloq, a U.S.-based company that provides enterprise clients with blockchain infrastructure: cloud services, analytics, mining, staking, etc. Bloq has strong roots in the Bitcoin community; one of its founders is the former Bitcoin core developer Jeff Garzik, a prominent coder and entrepreneur. Garzik was a Bitcoin core developer from 2010 through 2016 and is one of the only developers who worked directly with Satoshi Nakamoto on Bitcoin’s code.

By 2018, Garzik had become disillusioned with debates about the virtues of Bitcoin versus Ethereum. Along with Bloq co-founder Matt Roszak, an early crypto investor, he launched Metronome to explore the nascent DeFi space at a time before the term “DeFi” really even meant anything.

Garzik initially envisioned Metronome as a cross-chain platform and currency. He sought to connect Bitcoin with the fast-growing world of Ethereum smart contracts by building architecture around a [new kind of token](https://www.ibtimes.com/will-metronome-cryptocurrency-fulfill-jeff-garziks-blockchain-vision-2654089). “Metronome itself is an asset like gold in a warehouse, and moving it across chains is almost the opposite of a swap,” he told reporters at the time. “It’s the same asset moving to a different warehouse.” The Metronome token, then called MTN but later referred to as MET, would be affordable and “[built to last](https://cryptoslate.com/metronome-thousand-year-crypto/).”

Version 1.0 of the protocol launched with a set of Ethereum smart contracts and an AMM DEX that were innovative for their time. The team raised millions through an ICO. However, Metronome 1.0 was unable to find a [meaningful product-market fit](https://github.com/autonomoussoftware/metronome2-documentation/issues/1).


### Metronome 2.0 (2022- )

The protocol was then redesigned from scratch, relaunching in August 2022 as Metronome 2.0, with a roadmap to transition governance to a newly created Metronome DAO. Because Metronome 2.0 bears little resemblance to 1.0, the authors of this report believe it should be viewed as a new protocol.

The first application launched by Metronome 2.0 is a synthetic asset platform called Metronome Synth, currently in public beta. Users deposit a variety of collateral tokens, and the protocol allows users to mint its own representations of USD, ETH BTC and DOGE — msUSD, msETH, msBTC, msDOGE. These synthetics can then be swapped or farmed for yield.

Metronome now has a DAO roadmap that emphasizes its commitment to decentralizing its governance, although governance currently remains highly centralised (see the “Governance” section below). It currently relies on a multisig, the signers of which are [fully doxxed](https://www.metronome.io/about) Bloq personnel. Some Metronome team members are also involved with Vesper Finance (also [doxxed](https://vesper.finance/about/)), another DeFi offshoot of Bloq and a “[sister project](https://medium.com/vesperfinance/metronome-vesper-gateway-to-synthetic-assets-15509ca023b)” of Metronome. 

Vesper is a DeFi yield aggregator, like Yearn Finance, with its own staking derivatives of stablecoins and blue chip assets. Launched in February 2021, Vesper offers about 20 different yield-generating pools on Ethereum and additional pools on Polygon and Avalanche. Vesper pool tokens are yield-bearing and are composable with other DeFi protocols. Metronome Synth accepts Vesper derivatives as collateral.


# Assets 


## MET as an Asset

With Metronome 2.0, the team has introduced MET, a new “utility token” with three primary use cases: 

* **Collateral (planned)**: MET will be used in the future as collateral to mint synthetic assets on the Metronome dApp. 
* **Governance (in progress):** MET is intended as the governance token for the Metronome DAO. A MET holder will be able to lock tokens to create and vote on proposals. Currently MET is used in [Snapshot voting](https://snapshot.org/#/metronome.eth).
* **Trading Fee Discounts (planned):** Users will be able to stake MET in a tiered system, in which the number of tokens deposited and the duration of the lockup will give the user a discount on synthetic trading fees.

The team has not published detailed tokenomics for MET. At this time, it’s difficult to assess the token’s value proposition and soundness. Much will depend on the protocol’s implementation. Currently, we have some concerns about the risk introduced by MET as a collateral asset (see the “Economic Risk” section for more detail) and the value of MET in governance (see “Governance”). 


## Synthetic assets

The protocol mints its own synthetic assets against deposited collateral. The synths are [msUSD](https://etherscan.io/address/0xab5eB14c09D416F0aC63661E57EDB7AEcDb9BEFA#readProxyContract), [msETH](https://etherscan.io/address/0x64351fC9810aDAd17A690E4e1717Df5e7e085160#readProxyContract), [msBTC](https://etherscan.io/address/0x8b4F8aD3801B4015Dea6DA1D36f063Cbf4e231c7#readProxyContract) and [msDOGE](https://etherscan.io/address/0x7CEbE35b46b8078e7ffBF754EEc4A48653c47524#readProxyContract#F6), representing USD, ETH, BTC and Doge respectively. The price of the synthetics should maintain a “soft peg” with the underlying assets, generally trading 1:1. This strategy depends on market forces such as liquidity incentives and arbitrage to maintain the peg.


### msETH & msUSD

Metronome has built its liquidity strategy around Curve. There is currently about $1.5 million in the [msETH pool](https://curve.fi/#/ethereum/pools/factory-v2-252/deposit) and $1.3m in the [msUSD/FraxBP pool](https://curve.fi/#/ethereum/pools/factory-v2-251/deposit) after CRV incentives have been active for ~3 weeks.

Much of the LP in both pools is Protocol-Owned Liquidity. The Metronome team initially seeded the msETH pool with 300 ETH + 300 msETH and the msUSD pool with $150,000 msUSD + $150,000 FraxBP, according to Zane Huffman. The POL in the msETH pool is collateralized by a vaETH position.

This query shows the value of outstanding synths is still quite low, with the combined USD value of msETH and msUSD at under $1m:

![Screen Shot 2023-02-25 at 7 17 17 PM](https://user-images.githubusercontent.com/51072084/221390522-38841406-d62e-4049-ab0e-f2ce03d992c7.png)

Source: [LlamaRisk Dune Dash by DiligentDeer](https://dune.com/queries/2050243/3392158)


### Further Synthetics: msBTC and msDOGE

Aside from the synthetic assets that have been added to the Curve Gauge (i.e. msETH & msUSD), Metronome offers msBTC and msDOGE, neither of which is currently tradeable in public markets. The [total supply of msBTC is 0.25 BTC](https://etherscan.io/token/0x8b4F8aD3801B4015Dea6DA1D36f063Cbf4e231c7#readProxyContract#F10) and [msDOGE has a supply of 0](https://etherscan.io/address/0x7CEbE35b46b8078e7ffBF754EEc4A48653c47524#readProxyContract#F10).These synths can only be swapped on Metronome’s internal synth marketplace, and the Metronome team does not currently have plans to introduce further DeFi integration for these tokens.


### Synth Marketplace 

The [Synth Marketplace](https://docs.metronome.io/metronome-synth/metronome-synth-protocol/synth-marketplace) is a key feature of the Metronome Synth dApp, where users can swap synthetic assets. Swaps on the marketplace have no slippage and a low trading fee of 0.25%. Users can create long or short positions by swapping between synthetic assets, with individual liquidity limits based on collateral factors. However, liquidity is limited by a mintage cap on each synthetic asset, which prevents excessive concentration in any one synthetic or posted collateral. Synths can be exchanged for other synths at their current market price, provided they do not exceed the global synth mintage limitation, which is designed to maintain the health of the overall ecosystem.

Users should be aware of the history of [oracle frontrunning](https://blog.synthetix.io/frontrunning-synthetix-a-history/) with regard to synthetic asset protocols that depend on an oracle for execution pricing. The .25% fee on swaps does mitigate some frontrunning risk, but Metronome does not have additional protections in place. This can possibly result in losses that may be borne by Curve LPs.


## Collateral assets 

Metronome finance accepts the following assets as collateral and categorises them in their documentation: 

![Screen Shot 2023-02-26 at 1 02 29 PM](https://user-images.githubusercontent.com/51072084/221437144-b6b5605a-f58e-47c5-8931-b156b3bdc5f6.png)


## Vesper

As a yield-producing collateral type, it’s no surprise that Vesper pool tokens make up the vast majority of collateral backing on Metronome. This query by DiligentDeer finds >92% of Metronome's collateral backing to be by vaAssets:

![Screen Shot 2023-02-25 at 7 07 17 PM](https://user-images.githubusercontent.com/51072084/221390100-7f90bb9f-ef1e-4611-b0ba-51fb28ccbda2.png)

Source: [LlamaRisk Dune Dash by DiligentDeer](https://dune.com/queries/2050368/3393228)

Therefore, it’s prudent to explain the Vesper system architecture in more detail.

Strategies are designed with a modular philosophy. This is done to preserve simplicity for the end user while allowing Vesper the flexibility to continuously optimise as market conditions change. All that is required of the user is to select a “conservative” or “aggressive” pool strategy and deposit their chosen asset into the designated pool. Everything else happens under the hood.

Within the Vesper pool, multiple strategies are employed in parallel with capital weightings assigned to each strategy. A strategy might deposit funds into a different protocol or even a secondary Vesper pool. Active strategies may be reweighted as often as each week, and new strategies may be added with less frequency after undergoing an audit.


![image](https://user-images.githubusercontent.com/51072084/221347257-c16aae5e-2449-46e0-acb8-7e489cd94933.png)


#### Source: [Vesper's Modular Pool Architecture](https://docs.vesper.finance/vesper-pools-and-strategies/vespers-modular-pool-architecture)

Management of the underlying strategies is handled by the governor (a Vesper 3-of-5 multisig) and by keepers. Regular pool operations (such as strategy reweightings or to withdraw assets from a strategy in case of emergency) are done by authorized keeper addresses. Keepers can be added or removed by the governor. The governor controls critical system functionality, such as adding new strategies or pausing/permanently disabling contract execution.

Users with exposure to vaToken pools should be aware of the underlying strategies and risks associated with the various protocols where their funds are deployed. They should actively monitor for major updates to the pool strategy, and be comfortable placing trust in the governor (Vesper team) and authorized keepers. The [Vesper UI pool page](https://app.vesper.finance/eth/pools) is a convenient way to check the active strategies and weightings.


## A closer look at Vesper Pool-share tokens

Each Vesper pool deploys a unique set of strategies to generate yield, thus each asset needs to be evaluated individually. Note that the strategies as described are expected to change over time in both weighting and strategy composition.


### vaETH

vaETH pool accepts user deposits in ETH. Vesper then splits up the ETH by weight and lends it to various protocols (i.e. Euler, Aave and Alpha Homora Lend). DAI received from Aave and Euler is then deposited into an aggressive Vesper DAI Pool (which employs a strategy that does 2x leverage yield farming on Compound). The Alpha Homora Lend strategy directly deposits the ETH into the protocol to earn yield. There is a pool buffer which maintains a balance of undeployed capital to more cheaply facilitate typical withdrawal requests. More information on the strategy is available on [Github](https://github.com/vesperfi/vesper-pool-ops/issues/222) and in the [documentation](https://docs.vesper.finance/vesper-pools-and-strategies/strategies/direct-to-aave-or-compound).

We've created a flow chart to help visualize the current vaETH strategy: 

![image](https://user-images.githubusercontent.com/51072084/221347342-a3ed71d8-a677-497f-8416-66f597a0549d.png)


### vaUSDC

A Vesper user receives vaUSDC when they deposit USDC. The USDC is partially converted into FRAX and DOLA. (The risks of DOLA as an asset have been [previously reviewed](https://cryptorisks.substack.com/p/asset-risk-assessment-dola) by the Curve Risk Team.) The assets are then deposited into the Curve pools [DOLA/FraxBP](https://curve.fi/#/ethereum/pools/factory-v2-176/deposit) and [FRAX/USDC](https://curve.fi/#/ethereum/pools/fraxusdc/deposit), and the resulting LP tokens are deposited into Convex for additional yield. The "Convex For Frax" strategy additionally locks the Convex LP into Frax for a period of 6 days and 21 hours. A pool buffer requires 5% of USDC deposits to sit idle. Note the higher buffer compared to vaETH: the buffer is typically set higher for newer pools with lower TVL. 

We've created a flow chart to help visualize the current vaUSDC strategy: 

![image](https://user-images.githubusercontent.com/51072084/221347429-e805aa2a-bcf3-49d3-9dd1-14392b7c6a9f.png)


### vaFRAX

The user deposits FRAX, which is deposited into the Curve DOLA/FraxBP and Frax/USDC LP pools. The LP tokens are deposited into Convex for extra yield. The "Convex For Frax" strategy additionally locks the Convex LP into Frax for a period of 6 days and 21 hours.

We've created a flow chart to help visualize the current vaFRAX strategy:

![image](https://user-images.githubusercontent.com/51072084/221347459-8ea62237-5f5f-493b-8c15-233a080051f6.png)


### Final Thoughts on Metronome + Vesper

There is no spot market for synthetic Vesper assets, and must be redeemed through the protocol. A user or liquidity provider who interacts with these assets should be aware they are taking on multiple layers of smart-contract risk — both from the Vesper contracts and the contracts of the various protocols used in the yield strategies.

Additionally, it’s worth noting that the Metronome and Vesper teams are part of the same overarching company (Bloq). There is potential for bias in the Metronome team’s risk evaluation in accepting Vesper collateral. On the other hand, several large and reputable DeFi projects have also integrated Vesper tokens in a similar capacity, including [Frax](https://medium.com/vesperfinance/this-week-frax-gauge-goes-live-mstable-advances-january-in-review-5847508b253) and [Alchemix](https://medium.com/vesperfinance/you-can-now-use-vesper-with-alchemix-68dd3937f6de). Vesper’s pool contracts are [audited](https://github.com/vesperfi/doc/tree/main/audit/v3%2B), and every new strategy first undergoes an audit.

It’s likely that the Curve pool and gauge will substantially increase liquidity for Metronome synths and help maintain the peg, making the assets more attractive to LPs and the protocol more robust. However, Metronome is a young protocol that has yet to prove demand for its Synth product. Questions remain whether it will thrive long-term in a competitive market where other synthetics platforms (i.e. Synthetix) are more established and offer a wider range of features. The Metronome team commented that their unique value proposition within the synthetic space lies in the productive collateral niche.


# Risk Vectors 


## Governance

Metronome is governed by the MET token, which has existed in two different versions. MET 1.0 was issued when the project first launched in 2018. Later, as part of the 2022 relaunch, the MET DAO was created, along with a new token to govern it: MET 2.0 ([see MIP-001 for more on the tokenomics](https://github.com/autonomoussoftware/metronome2-documentation/issues/1)). The following graph shows how it was distributed:

![image](https://user-images.githubusercontent.com/51072084/221347582-abc087e1-c841-4dbf-af9f-ea24831ae729.png)

Source: Metronome docs

MET DAO works much like other DAOs: Members can devise and share Metronome Improvement Proposals (MIPs), cast votes on MIPs (off-chain via Snapshot), and participate in forum debates. At this stage of the protocol’s evolution, the DAO is highly centralised, the protocol being governed by a team-owned multisig. However, the Metronome roadmap stresses a commitment to [progressive decentralisation](https://www.metronome.io/post/the-role-of-governance-in-decentralized-finance).

There is an uneven distribution of MET tokens. While 14,377,915 MET currently exist, only 3,756,991 are effectively circulating and able to participate in governance. More than half of the total supply — 8,620,924 MET — is locked in the [treasury](https://etherscan.io/token/0x2ebd53d035150f328bd754d6dc66b99b0edb89aa?a=0xd1de3f9cd4ae2f23da941a67ca4c739f8dd9af33), while 2,000,000 additional MET are used by the protocol in a [Uniswap V3 LP position](https://etherscan.io/token/0x2ebd53d035150f328bd754d6dc66b99b0edb89aa?a=0xceb5c29bde4604296135dd7b027a433fd3633516). It’s unclear how the 8.6 million tokens in the treasury will be distributed.

The 3,756,991 effective circulating tokens are highly concentrated in the hands of team members and project advisors, who own 2,000,000 MET, representing 53% of the effective circulating supply. These tokens have been vested, unlocking [linearly over a 24-month vesting period](https://github.com/autonomoussoftware/metronome2-documentation/issues/1). During that period, however, the team members and advisors are allowed to participate in governance and have done so as seen in past [snapshot votes](https://snapshot.org/#/metronome.eth/proposal/0xd96f4626bdd23a2b0c13315c1d08c9b66796e7c36fcc2cd5c358092c232bdf9b). The other 47% of the effective circulating tokens remain unclaimed in the [token migration contract](https://etherscan.io/token/0x2ebd53d035150f328bd754d6dc66b99b0edb89aa?a=0xe67516417a934b27cf0c14868f8165b1bc94bf73) (1,541,560 MET or 41% of the effective circulating supply). It should be noted that there is no expiration date on the claim to the token migration contract for v1 holders. The remaining 6% of the effective circulating supply is owned by only [125 holders](https://etherscan.io/token/0x2ebd53d035150f328bd754d6dc66b99b0edb89aa#balances) with the largest being a [centralised exchange address (Gate.io)](https://etherscan.io/address/0x0d0707963952f2fba59dd06f2b425ace40b492fe) holding 2%, according to Etherscan.

![image](https://user-images.githubusercontent.com/51072084/221347627-ab2ece99-5896-4071-84e5-9c9ab7bcd8f9.png)

Source: Metronome docs + enrichment by authors

This lopsided token distribution creates barriers to meaningful community participation in governance. For instance, only one [EOA address](https://etherscan.io/address/0x73f10d7affbab50326441c56d775a075e54698f9) has sufficient voting power — a delegation of at least 25,000 MET — to [bring a proposal to vote](https://docs.metronome.io/metronome-2.0/governance/voting-and-participation) using Snapshot, an off-chain voting tool. According to MET DAO rules, at least 4% of the circulating supply must vote on a proposal for the vote to be binding. In practical terms, this means that 150,279 MET tokens (3,756,991 * 0.04) must participate in a vote to push any proposal through governance. Although 4% is not high compared to the governance procedures of other projects, the token distribution makes this bar more difficult for a non-team member to reach (while the team could reach it easily). The cost of reaching quorum at the current price of MET ($1.15) is about $170,000. Additionally, even if a community member does push a Snapshot vote through, they must rely on the team to implement the result.

Snapshot provides a sentiment check, not meaningful control over the protocol. Realistically, the only way to decentralise governance is to transfer control from the team multisig to an on-chain DAO. At the moment, however, the multisig has very broad authority (see “Centralisation in Operation” below).

The team has laid out a plan to give the community full control of decisions about the protocol over the next [15 months](https://docs.metronome.io/metronome-2.0/governance). However, some of the deadlines in the decentralisation roadmap appear to have been missed. For example, one month after launch, the number of signers on the treasury multisig wallet was supposed to [expand](https://docs.metronome.io/metronome-2.0/governance). Six months later, this hasn’t happened yet.

On the other hand, they submitted a [proposal](https://gov.vesper.finance/t/discussion-sunset-vvsp-introduce-locked-vsp/102) last March outlining a plan to upgrade tokenomics to a locked token model inspired by veCRV. This model is intended to be used by both Vesper and Metronome and will include on-chain governance control for adding new pools, updating pool rates, and setting deposit/mint caps. According to the team, the audit for the token lockup module is concluding shortly, after which a gradual transition to on-chain governance is expected to take place.

There are some concerning similarities between Metronome and Vesper when it comes to the pace and execution of their decentralisation plans. After launching in early 2021, the Vesper team said they would hand over control of the protocol to the community within one year. Yet today there are still signers on the treasury multisig who are also members of the core team.


## Centralisation in Operation 

Metronome is operated by three working groups: Engineering, Growth, and Operations. Their roles are described in [this blog post](https://www.metronome.io/post/the-role-of-governance-in-decentralized-finance):  

* Engineering maintains the existing Metronome smart contract framework, develops new features, tries to keep the protocol secure, and collaborates with code auditors.
* Growth focuses on building the DAO community, finding product-market fit, and increasing the user base through marketing and partnerships.
* Operations handles administrative tasks such as treasury management, operations, and legal.

Each working group has its own multisig wallet with at least three signers. Currently, none of the wallets hold meaningful value except for the Team & Advisor Wallet and Governance Participatory Committee Wallet. When we look at the multisigs, we can see a significant overlap between signers:

![Screen Shot 2023-02-26 at 1 49 56 PM](https://user-images.githubusercontent.com/51072084/221439512-d45123fb-5245-4a54-b788-459b08a9d911.png)

As shown in the table, out of 20 signers across all project multisig wallets, only 7 are unique. One signer — 0xf3e — is on every multisig. Two other signers appear on 4 of the multisigs.


### The Treasury Multisig

The most important and powerful multisig is the [2-of-4 treasury multisig wallet](https://etherscan.io/address/0xd1DE3F9CD4AE2F23DA941a67cA4C739f8dD9Af33). This wallet holds most of the assets owned by the protocol, including 8.5 million MET and 2,600 ETH. The size of the treasury dwarfs Metronome Synth’s [current TVL](https://defillama.com/protocol/metronome-synth) ($2 million).

![image](https://user-images.githubusercontent.com/51072084/221347675-e74060aa-4d8b-475b-ba09-c4fe601c4f74.png)

Source: [Debank Portfolio Tracker](https://debank.com/profile/0xd1de3f9cd4ae2f23da941a67ca4c739f8dd9af33)

Beyond simply controlling these assets, the Treasury multisig also governs the smart contracts in the protocol. It has extensive privileges. According to the [Quantstamp audit](https://github.com/autonomoussoftware/metronome-synth-audit/wiki/Audit) of Metronome, the multisig can do the following:  


* Transfer the governor role of the protocol’s smart contracts. For example, see [PoolRegistry.sol](https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#writeProxyContract#F8), yet in essence all contracts are affected
* [Add](https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#writeProxyContract#F5) and [remove](https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#writeProxyContract#F10) pools through the PoolRegistry.sol contract
* [Pause](https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#writeProxyContract#F4)/ [Unpause](https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#writeProxyContract#F9) the protocol by disabling/enabling deposits in the PoolRegistry.sol
* [Shutdown](https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#writeProxyContract#F6)/ [re-open](https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#writeProxyContract#F3) the protocol through the PoolRegistry.sol contract by prohibiting/re-enabling calls to liquidate, swap, repay, withdraw, issue debt or deposit  
* Change the current maximum total supply of [deposit](https://etherscan.io/address/0xA77B145c7Fa5B412eb8aD41D587bE892b9c1EFC3#writeProxyContract#F12), [debt](https://etherscan.io/address/0xF43de8E0c2596E30c77d69d158842D1d9B937D7c#writeProxyContract#F13) and [synthetic](https://etherscan.io/address/0x64351fC9810aDAd17A690E4e1717Df5e7e085160#writeProxyContract#F11) tokens
* Mint an arbitrary amount of [deposit](https://etherscan.io/address/0xA77B145c7Fa5B412eb8aD41D587bE892b9c1EFC3#writeProxyContract#), [debt](https://etherscan.io/address/0xF43de8E0c2596E30c77d69d158842D1d9B937D7c#writeProxyContract#F5) and [synthetic](https://etherscan.io/address/0x64351fC9810aDAd17A690E4e1717Df5e7e085160#writeProxyContract#F6) tokens (up to a max supply that is itself changeable) to an arbitrary address 
* Burn an arbitrary amount of [debt](https://etherscan.io/address/0xF43de8E0c2596E30c77d69d158842D1d9B937D7c#writeProxyContract#F3) or [synthetic](https://etherscan.io/address/0x64351fC9810aDAd17A690E4e1717Df5e7e085160#writeProxyContract#F2) tokens from an arbitrary address
* Arbitrarily move [deposit](https://etherscan.io/address/0xA77B145c7Fa5B412eb8aD41D587bE892b9c1EFC3#writeProxyContract#F10), [debt](https://etherscan.io/address/0xF43de8E0c2596E30c77d69d158842D1d9B937D7c#writeProxyContract#F11) and [synthetic tokens](https://etherscan.io/address/0x64351fC9810aDAd17A690E4e1717Df5e7e085160#writeProxyContract#F10) from one account to another 
* Change the speed of reward emissions through the [RewardDistributor.sol](https://etherscan.io/address/0xb975a3813AaCC7c9A8b61BFf5c3a0276154dDa2e#writeContract#F9)
* [Move all deposit, synthetic and underlying tokens](https://etherscan.io/address/0x3691EF68Ba22a854c36bC92f6b5F30473eF5fb0A#writeProxyContract#F2) from the Treasury.sol contract to an arbitrary address
* [Change the collateralization ratio (from 0% to 100%)](https://etherscan.io/address/0xA77B145c7Fa5B412eb8aD41D587bE892b9c1EFC3#writeProxyContract#F11) and thereby impact the amount of underlying assets that a user receives when withdrawing or depositing
* Change the [interest rate on debt tokens](https://etherscan.io/address/0xF43de8E0c2596E30c77d69d158842D1d9B937D7c#writeProxyContract#F12) to an arbitrary value 

The [2-of-4 treasury multisig wallet](https://etherscan.io/address/0xd1DE3F9CD4AE2F23DA941a67cA4C739f8dD9Af33) effectively controls the entire protocol. If this wallet is compromised, or if two of its signers act maliciously, all user funds and treasury funds would be at risk. The fact that Metronome’s leaders are doxxed gives some “legal” assurance to users against the rugging of funds by the team itself. But Metronome possesses an unusually large treasury for a project of its size and TVL, making it an appealing target for social-engineering attacks.

Asked about this concern, a team member said that by the time Metronome Synth leaves beta, some functions will be transferred to the community. Users will be able to add new pools, update pool rates, and set deposit/mint caps through an on-chain governance mechanism based on NFTs that represent locked positions. The same governance module is planned to be implemented by both Metronome and Vesper.


## Smart Contract Risk

The Metronome Synth code is audited by Quantstamp. In addition, they are currently running an [Immunefi bug bounty program](https://immunefi.com/bounty/metronome/). The [Quantstamp audit report](https://github.com/autonomoussoftware/metronome-synth-audit/wiki/Audit) is available on Metronome’s GitHub. Quantstamp identified two medium-risk issues, three low-risk issues and 11 informational issues. Out of all 16 issues, 9 have been resolved, according to the report. This section will focus on a subset of selected issues that might impact the protocol. (It should be noted that the code repository audited by Quantstamp seems to have moved since the audit; Metronome’s GitHub does not contain the audited code and the link in the audit report that points to the repo is broken.) 

**Absence of Fallback Oracle:** DeFi platforms depend on oracles to accurately price assets. If an attacker is able to falsify price data, he may deplete the funds by minting more of a synthetic representation (i.e. msETH, msUSDC). In addition, the collateralization ratio may get triggered prematurely or not at all with a falsified price datapoint. In the current deployment of Metronome Synth, the protocol uses a call to Chainlink — masterOracle — to get pricing data. The protocol does not cross-check the data with another source, and there is no fallback oracle in case the Chainlink network falters. Quantstamp classified this issue as Medium risk. The Metronome team responded that they would soon add a fallback oracle, but as of today, there is none.

**Proxy Contracts:** Smart contracts can give privileged roles to specific Ethereum addresses, such as the owner. There is often good reason to assign such privileges, like enabling contract upgrades or handling emergencies. Still, this can be a risk for the end user, which is why many smart contracts are immutable. All of Metronome’s smart contracts are upgradable, except for the deployed NativeTokenGateway and RewardsDistributor contracts. This means that, in practice, the 2-out-of-4 MET Treasury multisig can change the logic of these contracts at any time. 

The best practice when using a proxy pattern is to make the owner of the smart contract a [timelock contract](https://wiki.rugdoc.io/docs/timelocks-explained/). This generates trust between the developer and the user because, in theory, the user can exit the smart contract before changes are implemented. The Metronome proxy contract is not connected to a time lock contract. Therefore, the contract can be upgraded as soon as the threshold of two signatures of the treasury wallet is reached.  

**Block Timestamp Manipulation:** The protocol appears to use the block timestamp to calculate the interest accrued on debt or rewards. Quantstamp notes that validators can manipulate the timestamp by 900 seconds (~15 min). This may lead to minor changes in Metronome users’ debt or rewards. This issue could be easily mitigated by using the block height, instead of the timestamp, to calculate interest.


## Oracle Risk

Metronome relies entirely on Chainlink oracles to price its Synth assets. Chainlink is the market standard for price oracles and is generally considered to be secure. Nevertheless, the user should realise that the proper functioning of the protocol involves a [delegation of trust](https://samczsun.com/so-you-want-to-use-a-price-oracle/) to the Chainlink network of nodes.

The Metronome team [informs users](https://docs.metronome.io/metronome-synth/risks/oracle-disruptions) about oracle risk in the docs, explaining that an oracle disruption can lead to the de-pegging of asset pairs, liquidation of users, and failed transactions on-chain when users try to mint or close synthetics positions.


### Oracle Front-running

There is a potential front-running risk associated with the marketplace in the Metronome Synth dApp. (The market leader in this space, Synthetix, has detailed its [multi-year battle](https://blog.synthetix.io/frontrunning-synthetix-a-history/) against front-running.) Traders who persistently front-run oracle updates could harm the pool balance over time and hurt msUSD/USD LPs, who would become unwitting exit liquidity for toxic trade flow.

For example: Imagine, a trader has $1500 worth of collateral in WBTC, which is fully hedged somewhere else. They mint $1000 msBTC, but a sudden 10% BTC price drop causes the value of their collateral to decrease, resulting in the trader owing the protocol $900. The trader then swaps $1000 msBTC to $1000 msUSD without slippage on Synth Marketplace before the oracle can update. After the oracle update, the collateral is only worth $1350. The trader then swaps $1000 msUSD back to $900 msBTC and uses it to retrieve their collateral, incurring no losses due to hedging. The trader has now made $100 profit in msUSD, which they sell for USDC in the msUSDC/USDC Curve LP. 

To deter front-running by making it unprofitable, Metronome has implemented a 0.25% fee on trades. However, the effectiveness of this strategy depends on the volatility between oracle updates being lower than 0.25%. The team says they are also planning a privileged discount system for platforms such as Curve/1inch, in the hopes that most trading will occur on layer 2s.


## Economic Attack Vectors


### Market Manipulation

We do not see an immediate risk of market manipulators harming the protocol. Assets accepted as collateral (ETH / WBTC / DAI / USDC / FRAX / vaETH / vaUSDC / vaFRAX) are stablecoins, blue chip assets, or synthetic representations from Vesper Finance assets. The markets for the stablecoins and blue chips are deeply liquid. An attacker would need large sums of money to substantially move the price, making an attack costly and inefficient.

However, the future [introduction of MET as collateral](https://docs.metronome.io/metronome-synth/the-metronome-token-usdmet) may pose a risk. The recent attack on Mango Markets, in which an attacker drained the protocol by artificially inflating the price of MNGO, has shown that low-liquidity DAO tokens such as MET can introduce vulnerabilities if used as collateral. For more detail on this risk, see this excellent [breakdown of the Mango attack](https://www.soliduslabs.com/post/mango-hack) by Solidus Labs.

Likewise, Metronome users should be aware that they are taking on smart-contract risk from the project’s reliance on Vesper Finance. If an attacker is able to somehow mint arbitrary amounts of vaETH and use it as collateral in Metronome Synth, loanable assets can be drained. If an attacker can exchange the minted vaETH for msETH or msUSD, he can drain the proposed LP pools on Curve. It should be noted that a maximum supply is in place for collateral (for instance, [vaETH](https://etherscan.io/address/0x45AC59746Ea5Eb74cF782855eca460A8Adc8925a#writeProxyContract#F12) currently has totalSupply=980 ETH and maxTotalSupply=4170 ETH) and for synthetic tokens ([msETH](https://etherscan.io/address/0x64351fC9810aDAd17A690E4e1717Df5e7e085160#writeProxyContract#F11) has totalSupply=327 and maxTotalSupply=1250), yet both parameters can be changed by the [2-of-4 treasury multisig wallet](https://etherscan.io/address/0xd1DE3F9CD4AE2F23DA941a67cA4C739f8dD9Af33). 


### The parameterisation of the protocol 

The Metronome docs outline the all-important Liquidation and Collateral factors for debt positions in the protocol. Each position is over-collateralized, meaning that users must deposit more collateral than the value of synthetics they receive. 

Different collateral assets have different collateral factors determining how many synthetics can be generated from a deposit. Poor collateralization factors and liquidation procedures can leave the protocol with bad debt. Normally, when a user’s debt exceeds the value of his collateral, his collateral is typically liquidated by a third party (e.g. a bot). However, if liquidation does not occur fast enough, the collateral might not cover the outstanding debt. Bad debt can also build up if collateralization factors are too high (see this [Risk DAO dashboard](https://bad-debt.riskdao.org/) for more detail on bad debt in crypto projects). When the protocol accrues too much bad debt, some will be unable to withdraw their funds.

Metronome’s [current liquidation and collateral factors](https://docs.metronome.io/metronome-synth/metronome-synth-protocol/liquidations-and-collateral-factors) seem reasonable: 75% for stablecoins, 70% for blue chips and 60% for Vesper Synthetics. However, it appears that Metronome has chosen the collateral factors without testing whether they are optimal. We would like to see the protocol run some simulations on parameter settings, to minimise the risk of bad debt. We also recommend the creation of an insurance fund to make users whole if the worst occurs.


# Discussion 



1. **Is it possible for a single entity to rug its users?**

Yes. Due to the powers of the [2-out-of-4 treasury multisig wallet](https://etherscan.io/address/0xd1DE3F9CD4AE2F23DA941a67cA4C739f8dD9Af33), the entire protocol can essentially be shut down at any moment by two people who work for the same private U.S. company (Bloq). This degree of centralisation may be justifiable at this relatively early stage of Metronome 2.0. Still, it poses a clear risk for users. While the team has outlined plans to decentralise by degrees, this remains mostly just a promise.



2. **If the team vanishes, can the project continue?** 

In theory, the project could continue. Smart contracts can be forked and maintained by the community. In practice, though, this is unlikely, because if the team vanished, the community would no longer have access to the treasury, the remainder of the MET token supply or the Metronome Synth contracts. It’s worth noting that most of the MET earmarked for longtime community members remains unclaimed in the [migration contract](https://etherscan.io/address/0xe67516417a934b27cf0c14868f8165b1bc94bf73#code), which suggests a lack of engagement by the Metronome community in the first place. 



3. **Does the project viability depend on additional incentives?**

No. Incentives aren’t fundamental to the project’s success, although it may be difficult to bootstrap adoption without them.



4. **If demand falls to 0 tomorrow, can the users be made whole?**

Yes, with a caveat: The collateralization ratios have not been tested, increasing the chance that bad debt may build up, which would prevent users from withdrawing funds.



5. **Do audits reveal any concerning signs?**

The Quantstamp audit reveals some issues with the smart contracts, including the absence of fallback oracles. But the bigger problem is not with the code itself but with the privileges afforded to the [2-of-4 treasury multisig wallet](https://etherscan.io/address/0xd1DE3F9CD4AE2F23DA941a67cA4C739f8dD9Af33). The multisig controls all protocol functions and can “update” it at will without a timelock.


# Conclusion

From our perspective, the main issue with Metronome is its considerable degree of centralization. The project team says they are working on this and plan to give users greater autonomy down the line. So far, however, community interest in governance has been lacklustre, raising the question of whether people will participate even if they have the tools for it. Also, because user adoption of Metronome Synth has been slow, it remains unclear if the protocol can effectively compete in the Synthetic assets category. However, the Metronome team has the experience and resources to keep operating and improving. It’s also important to keep in mind that Metronome 2.0 is relatively new and still in beta, implying that this is an early developmental stage of the project.

Right now, Metronome Synth users and Curve LPs are required to put a lot of trust in the team. We encourage the Curve DAO to monitor the team’s stated roadmap to decentralise governance and make sure they are hitting those marks. The DAO should also keep an eye on the adoption rate of msTokens. We recommend that Curve LPs view the current gauge as a bootstrapping mechanism. Progress toward decentralisation and protocol maturity will determine if the gauge should remain.


#Appendix: Metronome Contract Addresses

The following are the addresses of key Metronome contracts. Though they are not published in the docs, team members in the project Discord readily gave the addresses when asked.


<table>
  <tr>
   <td colspan="2" ><strong>Metronome Synth Contracts</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Deposit Contract</strong>
   </td>
  </tr>
  <tr>
   <td>DAIDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0x1f9732B84e22E936cFc2FF6F2d4994097DCCC93e#readProxyContract">0x1f9732B84e22E936cFc2FF6F2d4994097DCCC93e</a>
   </td>
  </tr>
  <tr>
   <td>FRAXDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0x608249cc11728E3b978f7B27F1EA13F607D484EF">0x608249cc11728E3b978f7B27F1EA13F607D484EF</a>
   </td>
  </tr>
  <tr>
   <td>USDCDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0x1A9551de6d56f7768398a82aA2186624a43d89e3">0x1A9551de6d56f7768398a82aA2186624a43d89e3</a>
   </td>
  </tr>
  <tr>
   <td>wBTCDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0x7f9e66640Fec701D9f46ed5eD69F925fFDbb4683#readProxyContract">0x7f9e66640Fec701D9f46ed5eD69F925fFDbb4683</a>
   </td>
  </tr>
  <tr>
   <td>wETHDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0xA77B145c7Fa5B412eb8aD41D587bE892b9c1EFC3#readProxyContract">0xA77B145c7Fa5B412eb8aD41D587bE892b9c1EFC3</a>
   </td>
  </tr>
  <tr>
   <td>vaETHDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0x45AC59746Ea5Eb74cF782855eca460A8Adc8925a#readProxyContract#F6">0x45AC59746Ea5Eb74cF782855eca460A8Adc8925a</a>
   </td>
  </tr>
  <tr>
   <td>vaFRAXDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0x63EC45313149b1fa677b2b91CB93880232EF63AC#readProxyContract#F6">0x63EC45313149b1fa677b2b91CB93880232EF63AC</a>
   </td>
  </tr>
  <tr>
   <td>vaUSDCDepositToken
   </td>
   <td><a href="https://etherscan.io/address/0xdAec887E37e86ea9B78852EB7470D70bbF266258#readProxyContract#F6">0xdAec887E37e86ea9B78852EB7470D70bbF266258</a>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Debt Contracts</strong>
   </td>
  </tr>
  <tr>
   <td>msBTCDebt
   </td>
   <td><a href="https://etherscan.io/address/0xB93f48D3eA42a25f367fAde092A6Bb56DAB5F7CB#readProxyContract">0xB93f48D3eA42a25f367fAde092A6Bb56DAB5F7CB</a>
   </td>
  </tr>
  <tr>
   <td>msDOGEDebt
   </td>
   <td><a href="https://etherscan.io/address/0x4c02B2F1B003B1A454A2f401203beD2499798b54#readProxyContract#F7">0x4c02B2F1B003B1A454A2f401203beD2499798b54</a>
   </td>
  </tr>
  <tr>
   <td>msETHDebt
   </td>
   <td><a href="https://etherscan.io/address/0xF43de8E0c2596E30c77d69d158842D1d9B937D7c#readProxyContract#F7">0xF43de8E0c2596E30c77d69d158842D1d9B937D7c</a>
   </td>
  </tr>
  <tr>
   <td>msUSDDebt
   </td>
   <td><a href="https://etherscan.io/address/0x480e3178Fa102dF852643d47CAbdb9adf5dB0174#readProxyContract#F7">0x480e3178Fa102dF852643d47CAbdb9adf5dB0174</a>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Synthetic Contracts</strong>
   </td>
  </tr>
  <tr>
   <td>msDOGESynthetic
   </td>
   <td><a href="https://etherscan.io/address/0x7CEbE35b46b8078e7ffBF754EEc4A48653c47524#readProxyContract#F6">0x7CEbE35b46b8078e7ffBF754EEc4A48653c47524</a>
   </td>
  </tr>
  <tr>
   <td>msBTCSynthetic
   </td>
   <td><a href="https://etherscan.io/address/0x8b4F8aD3801B4015Dea6DA1D36f063Cbf4e231c7#readProxyContract">0x8b4F8aD3801B4015Dea6DA1D36f063Cbf4e231c7</a>
   </td>
  </tr>
  <tr>
   <td>msETHSynthetic
   </td>
   <td><a href="https://etherscan.io/address/0x64351fC9810aDAd17A690E4e1717Df5e7e085160#readProxyContract">0x64351fC9810aDAd17A690E4e1717Df5e7e085160</a>
   </td>
  </tr>
  <tr>
   <td>msUSDSynthetic
   </td>
   <td><a href="https://etherscan.io/address/0xab5eB14c09D416F0aC63661E57EDB7AEcDb9BEFA#readProxyContract">0xab5eB14c09D416F0aC63661E57EDB7AEcDb9BEFA</a>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Protocol Contract</strong>
   </td>
  </tr>
  <tr>
   <td>NativeTokenGateway
   </td>
   <td><a href="https://etherscan.io/address/0x945E05D2F519908c18758a905e5D31Dd94892bD5#readContract">0x945E05D2F519908c18758a905e5D31Dd94892bD5</a>
   </td>
  </tr>
  <tr>
   <td>Pool
   </td>
   <td><a href="https://etherscan.io/address/0x3364f53cB866762Aef66DeEF2a6b1a17C1F17f46#readProxyContract">0x3364f53cB866762Aef66DeEF2a6b1a17C1F17f46</a>
   </td>
  </tr>
  <tr>
   <td>PoolRegistry
   </td>
   <td><a href="https://etherscan.io/address/0x11eaD85C679eAF528c9C1FE094bF538Db880048A#code">0x11eaD85C679eAF528c9C1FE094bF538Db880048A</a>
   </td>
  </tr>
  <tr>
   <td>Rewards Distributor
   </td>
   <td><a href="https://etherscan.io/address/0xb975a3813AaCC7c9A8b61BFf5c3a0276154dDa2e">0xb975a3813AaCC7c9A8b61BFf5c3a0276154dDa2e</a>
   </td>
  </tr>
  <tr>
   <td>Synth Collteral Vault
   </td>
   <td><a href="https://etherscan.io/address/0x3691EF68Ba22a854c36bC92f6b5F30473eF5fb0A">0x3691EF68Ba22a854c36bC92f6b5F30473eF5fb0A</a>
   </td>
  </tr>
  <tr>
   <td>MasterOrcacle 
   </td>
   <td><a href="https://etherscan.io/address/0x80704Acdf97723963263c78F861F091ad04F46E2">0x80704Acdf97723963263c78F861F091ad04F46E2</a>
   </td>
  </tr>
  <tr>
   <td>METRewardsDistributor
   </td>
   <td><a href="https://etherscan.io/address/0x9f6A09dd0ba23b5AD4234677C831146366678Ae3#readProxyContract">0x9f6A09dd0ba23b5AD4234677C831146366678Ae3</a>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Metronome Protocol Contracts</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Token Related Contract</strong>
   </td>
  </tr>
  <tr>
   <td>MET Token
   </td>
   <td><a href="https://etherscan.io/address/0x2Ebd53d035150f328bd754D6DC66B99B0eDB89aa">0x2Ebd53d035150f328bd754D6DC66B99B0eDB89aa</a>
   </td>
  </tr>
  <tr>
   <td>MET Mirgration contract
   </td>
   <td><a href="https://etherscan.io/address/0xe67516417a934b27cf0c14868f8165b1bc94bf73#code">0xe67516417a934b27cf0c14868f8165b1bc94bf73</a>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Treasury and Governor multisig </strong>
   </td>
  </tr>
  <tr>
   <td>MET Treasury
   </td>
   <td><a href="https://etherscan.io/address/0xd1DE3F9CD4AE2F23DA941a67cA4C739f8dD9Af33">0xd1de3f9cd4ae2f23da941a67ca4c739f8dd9af33</a>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>External Liquidity Pools</strong>
   </td>
  </tr>
  <tr>
   <td>Uniswap V3: msETH
   </td>
   <td><a href="https://etherscan.io/address/0x811d626f2fce65494268b9590a95a40a3461c429">0x811d626f2fce65494268b9590a95a40a3461c429</a>
   </td>
  </tr>
  <tr>
   <td>Uniswap V3: msUSD
   </td>
   <td><a href="https://etherscan.io/address/0xcc8d0c3bda2e46f846c70190dd8973381ca5cc30">0xCC8d0c3bDa2e46f846C70190dd8973381ca5Cc30</a>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>Curve Pools & Gauges</strong>
   </td>
  </tr>
  <tr>
   <td>msUSD/ FraxBP
   </td>
   <td><a href="https://etherscan.io/address/0xc3b19502f8c02be75f3f77fd673503520deb51dd">0xc3b19502F8c02be75F3f77fd673503520DEB51dD</a>
   </td>
  </tr>
  <tr>
   <td>msUSD Gauge
   </td>
   <td><a href="https://etherscan.io/address/0xd5f2e6612E41bE48461FDBA20061E3c778Fe6EC4#code">0xd5f2e6612E41bE48461FDBA20061E3c778Fe6EC4</a>
   </td>
  </tr>
  <tr>
   <td>msETH/ETH Curve Pool
   </td>
   <td><a href="https://etherscan.io/address/0xc897b98272aa23714464ea2a0bd5180f1b8c0025">0xc897b98272AA23714464Ea2A0Bd5180f1B8C0025</a>
   </td>
  </tr>
  <tr>
   <td>msETH Gauge
   </td>
   <td><a href="https://etherscan.io/address/0x941C2Acdb6B85574Ffc44419c2AA237a9e67be03">0x941C2Acdb6B85574Ffc44419c2AA237a9e67be03</a>
   </td>
  </tr>
</table>
