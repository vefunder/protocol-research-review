# Consequences of the Euler Exploit for Angle's agEUR and Euro-Denominated Curve Pools


## Useful links

* [Angle Website](https://www.angle.money/#/) | [General Docs](https://docs.angle.money/overview/readme) | [Developer Docs](https://developers.angle.money/)
* [Angle Governance Forum](https://gov.angle.money/) | [Snapshot page](https://snapshot.org/#/anglegovernance.eth)
* [AgEUR - LlamaRisk Assessment (Jan 2022)](https://cryptorisks.substack.com/p/ageur-angle-protocol)
* [AIP-43: Improve Angle Yield Strategies for USDC and DAI](https://snapshot.org/#/anglegovernance.eth/proposal/0xb1b4d98c080ec587b2563a6aaa6f854e0a42ce6881f61bced62cf9fa8ae42898)
* [Euler exploit analysis](https://medium.com/@numencyberlabs/a-detailed-analysis-of-Euler-finances-196-million-flash-loan-attack-81cdef370024)
* [AgEUR/EUROC Factory Pool](https://curve.fi/#/ethereum/pools/factory-v2-164/deposit) | [3EURpool Factory Pool](https://curve.fi/#/ethereum/pools/factory-v2-66/deposit)
* [Angle Protocol - Q&A Regarding Euler Exploit](https://anglemoney.notion.site/Angle-Protocol-Q-A-Regarding-Euler-Exploit-03af18cbe5e84430b3341b145554492e)
* [Angle Protocol - Ideas for re-launching the protocol](https://gov.angle.money/t/ideas-for-re-launching-the-protocol/674)
* [Euler Hack Reflection: Learning, Adapting, and Strengthening Angle Protocol](https://www.angle.money/#/blog/announcements/euler-hack-reflection-learning-adapting-and-strengthening-angle-protocol)


## Introduction

In a [previous report](https://cryptorisks.substack.com/p/ageur-angle-protocol), we explored the Angle Protocol, an innovative stablecoin framework designed for price stability, over-collateralization, and optimal capital efficiency. The protocol's stablecoin, agEUR, aims to be pegged to the Euro and has demonstrated impressive price stability compared to other Euro-based stablecoins. This report delves into the Euler Finance exploit that occurred on March 13th, 2023, examining its impact on the peg and liquidity on Curve. It also investigates the strategies employed by the Angle Core Module, the crisis management response by the Angle team, actions taken following Euler's recovery, and the lessons learned from the incident. Finally, the report provides an updated risk assessment for agEUR and its related Curve pools.


### Key Findings:

- In Q1 2023, Angle modified its reserve investment approach for the Core Module, allowing anyone to change the investment strategy allocation via a permissionless function. However, Angle did not consider additional risk mitigation by diversifying yield sources. During the exploit, Angle had 74% of its USDC reserves invested in Euler.
- Before the hack, Angle had $36.1m (Core Module), $11.8m (SLP), and a surplus (treasury) of $5.6m. The exploit led to Angle Protocol losing approximately 17.6m USDC, leaving agEUR undercollateralized.
- Despite the relatively high signature threshold, Angle's governance multisigs (Guardian and Governor) quickly paused the protocol's critical functions and minimized further negative impact.
- Although present in the documents, the seniority principle which prioritizes agEUR holders over the Stablecoin Liquidity Providers (SLPs) and Hedging Agents (HAs) was not clearly stated and had to be voted on shortly after the exploit.
- The Euler incident caused considerable disruptions in agEUR associated Curve pools, resulting from a significant net outflow of euro-denominated stablecoins (>€8m).
- Euler's successful recovery of the stolen funds led to Angle protocol restoring over-collateralization. Angle may not have survived had the exploited funds been lost, leading to significant losses for SLP and HAs and a dire collateral ratio for agEUR.
- This de-peg event serves as a reminder of the various trade-offs associated with different stablecoin designs. While fiat-redeemable stablecoins may present challenges regarding transparency and regulatory compliance (EURS, EURT, EUROC), crypto-collateralized stablecoins like Angle grapple with capital efficiency concerns and potentially expose users to other protocol risks.


## Revisiting agEUR and Prior Report Findings: Balancing Overcollateralization and Capital Efficiency

The Angle Protocol presents a novel approach to crypto-collateralized stablecoin design that has gained recognition due to its unique method of balancing over-collateralization with capital efficiency. This balance is achieved by distributing risks among various market participants, namely: **Stable Seekers/Holders**, **Stablecoin Liquidity Providers** (SLP), and **Hedging Agents** (HAs).

Its primary product is the agEUR stablecoin. The agEUR stablecoin aims to maintain its peg to the Euro by leveraging a diverse collateral pool. Overcollateralization is a fundamental aspect of this design, ensuring that agEUR remains stable even during market fluctuations.


### Core Module and its Market Participants

The Core Module oversees collateral pools and mints stablecoins. It comprises agEUR minters, Stablecoin Liquidity Providers (SLPs), and Hedging Agents (HAs). At the same time, the Core Module sustains a debt-to-collateral ratio to ensure the stablecoin's over-collateralization. An in-depth look at the inner workings of the core module and its participants can be found in our [previous article](https://cryptorisks.substack.com/p/ageur-angle-protocol).

![Core Module](https://i.imgur.com/gUR3jGm.png)

Source: [Angle Docs: Core Module](https://docs.angle.money/angle-core-module/overview)


#### agEUR Minters

Users can deposit whitelisted collateral (DAI, USDC, FRAX, WETH, and FEI) and obtain agEUR in return. The operation is done as a 1:1 swap (minus fees). Deposits are then owned by the Core Module, which it subsequently lends out to other platforms. agEUR can always be minted or burned for the underlying collateral at the oracle value during normal operation. 


#### Hedging Agents (HAs)

HAs play a critical role in managing risks associated with collateral fluctuations. The Core Module hedges its volatility by allowing profit-seeking HAs to open perpetual futures positions. The HA opens a position with X amount of their own collateral and Y position size (how much of the Core Module collateral they will insure). Positive fluctuations in the protocol collateral are realized as profit to the HA, and negative fluctuations are realized as losses. The protocol can safely guarantee capital-efficient stablecoin minting/burning when its holdings are adequately hedged.  


#### Stablecoin Liquidity Providers (SLPs)

At times, HAs may not be completely insuring the protocol collateral, so SLPs are attracted to provide a system buffer. SLPs provide liquidity enabling the protocol to earn additional interest and ensure system solvency. In return, they receive a portion of fees generated from agEUR minting/burning and from protocol lending activities. They have additional yield potential, as they also earn interest on capital provided by both stablecoin minters and HAs. 

SLPs are instrumental in maintaining agEUR's system solvency and contribute to the overall stability of the stablecoin. In the case of the protocol becoming under-collateralized from insufficient HA positions, the liquidity provided by an SLP ensures 1:1 redeemability. Should an SLP choose to remove their liquidity in such circumstances, they would incur a slippage. This incentivizes a target system C-ratio. The protocol's C-ratio governs the slippage parameter:

* Above 120% C-ratio, the slippage is zero.
* Slippage increases with decreasing C-ratio.


## Angle's Core Module Reserve Allocation and Recent Changes

The Core module earns interest on these reserves by lending to other platforms. These strategies thereby enable protocol reserve accumulation, veANGLE holder incentivization, and contribute to the Core module's appeal for Standard Liquidity Providers (SLPs). By mobilizing liquidity into blue-chip DeFi protocols, Angle can offer the financial motive for both the cake-having (over-collateralization) and cake-eating (capital efficiency). It is done at the cost of composability risk from exposing the Core Module to third-party protocols (and therefore all risks associated with those protocols).


### AIP-43 and the Shift in Reserve Strategy

In January 2023, a proposal was [submitted to the Angle governance forum](https://gov.angle.money/t/aip-43-improve-angle-yield-strategies-for-usdc-and-dai/541), aiming to enhance the existing Angle strategies for USDC and DAI. The primary objective was to optimize the distribution of funds across various lending venues to maximize the overall yield.

The proposed system allows any individual to harvest the strategy and suggest a new allocation of funds to the underlying venues. The contract then verifies on-chain whether the proposed allocation results in a greater APR. If the proposal is superior, the contract transitions to the new allocation; otherwise, it retains the existing state and deploys idle funds into the highest-yielding lending venue. This approach supersedes the previous system, where funds were fully allocated to the platform offering the highest APR, which may not be the most efficient allocation.

Angle stated that the main benefit of this change was that, by employing efficient bots to execute optimizations, the protocol's lending revenues could increase by up to 50%. This would further boost the revenues for SLPs and veANGLE holders. Additionally, implementing the change necessitated only minimal modifications to the strategy engine and related contracts.

![Lending Strategies](https://user-images.githubusercontent.com/51072084/234950319-d21a93ac-c113-46ec-9738-12d2247654fa.png)

Source: [Angle Docs: Lending Strategies](https://docs.angle.money/angle-core-module/lending)

In February 2023, [AIP-43: Improve Angle Yield Strategies for USDC and DAI](https://snapshot.org/#/anglegovernance.eth/proposal/0xb1b4d98c080ec587b2563a6aaa6f854e0a42ce6881f61bced62cf9fa8ae42898) was successfully implemented following a favorable snapshot vote. It is also worth noting that this marked the introduction of the GenericEuler strategy, as advertised [in the official announcement](https://www.angle.money/#/blog/announcements/increasing-angle-yield-and-revenue-with-permissionless-off-chain-optimization).

[`OptimizerAPRStrategyV2`](https://github.com/AngleProtocol/angle-strategies/blob/main/contracts/strategies/OptimizerAPR/OptimizerAPRStrategy.sol) (based on Yearn's GenLender contract) was deployed, with the [following whitelisted strategies](https://github.com/AngleProtocol/angle-strategies/tree/main/contracts):
* [Generic Aave](https://etherscan.io/address/0xe4377620697Be18E6d6aa911CA488571EeB3f081)
* [Generic Compound](https://etherscan.io/address/0xE2773fB045e53De5344f245E03eA614AF1064Ce3#readProxyContract)
* [Generic Euler](https://etherscan.io/address/0xf5aD02F3DbBF4b42DEE1f1255607f929CA2a7c5a#readProxyContract)

These strategies are regular lenders, meaning they supply the capital to these Lending & Borrowing protocols and harvest interest earnings. Over the next few months, the system performed as intended, with debt allocation being changed periodically but primarily assigned to Generic Euler due to the added EUL incentives.

## Unraveling the Euler Exploit: Timeline, Reserve Implications, and Consequences for Curve Pools

On March 13th, 2023, Euler Finance suffered an exploit resulting in a loss of around $200 million. The vulnerability stemmed from Euler Finance permitting donations without an account health check, specifically in the `donateToReserves` function of the EToken implementation. This issue was introduced through [eIP 14: Contract Upgrades](https://forum.euler.finance/t/eip-14-contract-upgrades/305) and remained on-chain for eight months before being exploited.

Following the attack, Euler's native token, EUL, fell by 50%. The team responded by disabling the EToken module and the vulnerable donation function. Numerous protocols were affected, including Angle, Balancer, Idle, Inverse, SwissBorg, Swivel, Temple DAO, Yield, etc.

### Timeline (UTC)
Below is the timeline of March 13th events and actions taken by Angle:

- 08:50 UTC: Euler protocol is [exploited via flashloan](https://etherscan.io/tx/0xc310a0affe2169d1f6feec1c63dbc7f7c62a887fa48795d327d4d2da2d6b111d) 
- 09:42 UTC: Angle's Governance (4/6 multisig) [withdraws 3,360,927 agEUR](https://etherscan.io/tx/0x4ef9d6f7671905057d0473d7c3263af9a0e994430644e0027c3ef4c765ddf7b7)
- 10:03 UTC: Angle's Guardian (2/3 multisig) starts [pausing `StableMasterFront`](https://etherscan.io/tx/0x17ac0acd72566eac4cd3e05a8e9c76ba9695701aa562ad1f54a93450175916a7) (Core module)
- 10:48 UTC: `DAI/agEUR`, `USDC/agEUR` & `FRAX/agEUR` perpetuals Gauges [are also paused](https://etherscan.io/tx/0x2f58b0cb411c804c791c873ae7c4af2603b8b26dcba0efababd524eb58b18be9).
- 11:09 UTC: [burns the remaining agEUR](https://etherscan.io/tx/0x46540733e163ba1d5cce04d77bd6395c52c50126fd97c1c78d75f676d099115c) on the Euler contract
- 11:52 UTC: Debt ceiling [updated to 0](https://etherscan.io/tx/0x515b4679db46356bb9a598247095cbafab7dcf1ac5eb7f6ecafad3ea14961140) for the Borrowing module. The core module is fully paused.
- 13:42 UTC: Angle [removes liquidity](https://etherscan.io/tx/0xcf7f9c25034faaa5dcd3c5acd8fde5f06dc586e8ed7272e2368ae0a80af313f5) from `Curve agEUR/EUROC Factory`
- 14:56 UTC: Angle [posts a Q&A](https://anglemoney.notion.site/Angle-Protocol-Q-A-`Regarding-Euler-Exploit-03af18cbe5e84430b3341b145554492e) addressing the exploit
- 15:59 UTC: Reserves funds are [removed from Compound strategy](https://etherscan.io/tx/0x61f20b2bd6ebb5fa567e36ab6a60332a4749f066e931fc22b06e0880658d5f6e) (3,942,967 DAI & 6,126,537 USDC)

### Impact on Angles Core Module's Reserves

During the exploit, Angle's USDC reserves were heavily invested (74%) in the `GenericEuler` strategy. Fortunately, the DAI reserves suffered no losses as they were only allocated to the `GenericCoumpound` strategy.

![Angle USDC Reserve](https://i.imgur.com/J2PJUhl.png)

The Euler exploit resulted in **Angle losing 17.6m USDC** as detailed in the[ Angle Protocol - State of the Protocol spreadsheet](https://docs.google.com/spreadsheets/d/1SYkNR0BYBh4dHKSomJAXHXHZ9RkvYenn0DAvUAdTlJo/edit#gid=1182401585). At the time of the attack, the Core Module held the following assets for an estimated TVL of $36.1m:

* 4.11m DAI
* 24.99m USDC
* 6.9m FRAX
* 1.52 wETH
* 203k FEI

The total agEUR supply was 26,418,421 agEUR (excluding the agEUR burned on the Euler contract), with 17.3m having been minted through the Core Module. The protocol also had ~$11.8m in deposits from its SPLs, ~$71.0k from the HAs, and a surplus of ~$5.58m.

Following the exploit, Angle's liabilities far exceeded assets. In the event that no funds were recovered, the TVL of the Core Module would stand at $18.4m. The reserves in the Core module were inferior to the value of the claims of agEUR holders, Standard Liquidity Providers, and the remaining hedging agents. There would be a shortfall of around $12m ($18.4m in assets - $30.4m in liabilities).

![Screen Shot 2023-04-27 at 12 05 50 PM](https://user-images.githubusercontent.com/51072084/234966425-47502577-f2b0-4e3f-a373-06643ff06197.png)

Source: [Tokens Claimable from CM before Hack](https://docs.google.com/spreadsheets/d/1SYkNR0BYBh4dHKSomJAXHXHZ9RkvYenn0DAvUAdTlJo/edit#gid=38781252)

### Effect on Curve Pools and agEUR Peg

The peg on various Curve pools containing agEUR began to drop rapidly as the news of the exploit began to propagate.

![](https://i.imgur.com/uO98jir.png)

Balances of the two main Curve pools containing agEUR, 3EUR & agEUR/EUROC changed considerably as users offloaded agEUR.

![](https://i.imgur.com/JpNz3ug.png)

![](https://i.imgur.com/UeaLY2C.png)

Overall, the Euler exploit contributed to a net outflow of Euro-denominated stablecoins exceeding €8m.

![agEUR EUROC outflow](https://user-images.githubusercontent.com/51072084/234969580-13400110-5ac8-4111-a8a5-4530d2706b57.png)

Source: [Outflow from agEUR/EUROC pool- Llama.airforce](https://llama.airforce/#/curve/pools) 

Other pools that experienced minor effects included: [agEUR/ibEUR](https://curve.fi/#/ethereum/pools/factory-v2-78/deposit), [Euro Pool](https://curve.fi/#/ethereum/pools/factory-v2-145/deposit), [agEUR/EURe](https://curve.fi/#/ethereum/pools/factory-v2-231/deposit) and [agEUR/FRAXBP](https://curve.fi/#/ethereum/pools/factory-crypto-93/deposit). No significant impact has been observed on other euro Curve pools.

We also looked into a [large withdrawal](https://etherscan.io/tx/0x376a748c4eb1a174adf1de4c6a58ff4a3665196dfe652603c47f4e19b87d43bd) (approximately €1m equivalent) from the agEUR/EUROC pool that occurred a few days before the exploit. This appears unrelated to these events and could be a market participant derisking his positions during the USDC depeg event.


## Funds Recovery and Proposal to Restart the Protocol

On March 25th, the hacker began to return the stolen funds to the Euler DAO. By April 3rd, Euler had confirmed that [the entire amount was recovered](https://twitter.com/eulerfinance/status/1643027386802270210). The Euler hack affected [ETH/stETH, WBTC, USDC and DAI](https://etherscan.io/address/0x036cec1a199234fc02f72d29e596a09440825f1c#tokentxns), which the hackers had consolidated into ETH and DAI, constituting approximately 97% of the stolen amount. This situation prompted several discussions on the best approach for Euler to return the funds to users.

* **Initial exploit balances**: 66.3k wstETH, 34.4m USDC, 849.14 WBTC, 8,099.3 WETH, 8.88m DAI & 3,897.5 stETH.
* **Returned funds**: 95.5k WETH and 43.1m DAI


### Plan v1: Initial Feedback

On April 4th, Euler presented its [Redemption of Euler Funds plan](https://forum.euler.finance/t/plan-for-redemption-of-euler-funds/906) on the governance forum for discussion. The proposed plan entailed simulating the repayment of all liabilities at the block when the protocol was disabled, calculating the net asset value (NAV) for each account, and allowing users to claim the recovered ETH, DAI, and USDC based on their proportion of the total NAV. A smart contract would be created to facilitate the claims. 


### Plan v2: Redemption Contract

[Redemption Proposal v2](https://forum.euler.finance/t/plan-for-redemption-of-euler-funds-v2/947) was published on April 11th. The updated redemption plan addressed feedback on the initial proposal and outlined a mechanism to distribute the recovered assets to affected users following the attack. Critical updates include a more detailed calculation of net asset values (NAVs) for each sub-account and an alternate NAV calculation, including potential foregone profits due to the protocol's pause. It also addressed returning funds to smart contracts on a case-by-case basis:

> ### Smart Contract Accounts
>
> There are 141 affected smart contract accounts. Smart contracts can not necessarily execute the claim method on the merkle distributor contract. Furthermore, claims can not necessarily be sent directly to smart contracts without causing problems with their internal accounting.
>
> For these reasons, smart contracts will have to be handled on a case-by-case basis. Representatives of the Euler Foundation will communicate with Affected protocols and smart contract wallet holders can contact this email address for guidance given their particular situations: contact@euler.foundation
>
> Source: [Redemption Proposal v2](https://forum.euler.finance/t/plan-for-redemption-of-euler-funds-v2/947#smart-contract-accounts-8)

The redemption contract was activated on the same day at 2:00 UTC.


### Angle Funds Reclamation

Angle communicated with Euler and ultimately received the recovered funds on April 12th through these two transactions:

* [0xc8509333e66415834d8d44e392132730a2b20f2f4efa8a66b7728326b4668276](https://etherscan.io/tx/0xc8509333e66415834d8d44e392132730a2b20f2f4efa8a66b7728326b4668276)
* [0x8ab2c73bd55fb255124c720f8bc64b73d0932b653005b2ab8a6b355e94c29022](https://etherscan.io/tx/0x8ab2c73bd55fb255124c720f8bc64b73d0932b653005b2ab8a6b355e94c29022)

The breakdown of the returned funds is as follows:

* 7,408.8 ETH
* 3,416,991 DAI
* 263,379 USDC


### Vote to re-launch the protocol

On April 13th, a snapshot vote [was initiated](https://snapshot.org/#/anglegovernance.eth/proposal/0x3214d2a1f5ce14ebc976d9e3a171dc185c1fa466aaf401bd34e5d84f37095921) for re-launching the protocol.

![](https://i.imgur.com/jU1Uq8I.png)

Source: [Twitter](https://twitter.com/AngleProtocol/status/1646485888103518208?s=20)

This proposal outlined the necessary steps to secure the Angle and agEUR positions after Angle successfully recovered funds from the Euler hack. The primary objective of the vote was to establish a way to maintain agEUR's peg and provide a method for Staking Liquidity Providers (SLPs) to retrieve their funds and repay Hedge Accounts (HAs). 

Two options were proposed for swapping the recovered assets. Both options suggested upgrading the PoolManager contracts, unpausing SLP contracts, and settling HAs. The vote had a 25-hour timespan to allow for a quick swap, as the protocol held long ETH and bore significant exchange risk. This vote was intended as a first step towards getting the protocol back on track, with the ultimate goal of building a better, more scalable, and robust system that can make agEUR a backbone in DeFi.

#### Option 1 - Swap all recovered funds and additional DAI into EUROC

Option 1 suggested swapping the 7,408.8 ETH and 3,416,991 DAI received from Euler's redemption into EUROC and removing 2,700,000 DAI from the Core module's DAI PoolManager contract to be swapped into EUROC. This aimed to cancel the protocol's exposure to ETH and USD, backing all agEUR issued through the Core module with EUROC. This option would allow the Governor or Guardian multisig to manage the agEUR-EUROC pool liquidity. This option would pause mints and redemptions through the Core module.

#### Option 2 - Swap 50% of recovered ETH into USDC and the other 50% into EUROC.

Option 2 proposed swapping 50% of the 7,408 ETH obtained from Euler into EUROC and the other half into USDC. This option aimed to eliminate the protocol's exposure to ETH and partially remove its exposure to USDC. While the Core module could technically be restarted for agEUR holder redemptions after the swap, the protocol may need more USDC to handle all agEUR to be redeemed. The implications of this situation would be decided in a future vote.


### Results

The voting concluded on April 14th, 2023, witnessing substantial participation from the community. Option 2 emerged successful, albeit by a narrow margin.

![](https://i.imgur.com/Pr2kUSP.png)

Source: [Snapshot](https://snapshot.org/#/anglegovernance.eth/proposal/0x3214d2a1f5ce14ebc976d9e3a171dc185c1fa466aaf401bd34e5d84f37095921)


### OTC Trade

Angle decided to use a combination of Paraswap, 1inch, and [Keyrock Trading](https://twitter.com/KeyrockTrading) (OTC) to execute their swaps.

![](https://i.imgur.com/OdlRR1p.png)

Source: [Twitter](https://twitter.com/AngleProtocol/status/1646918304408416257?s=20)

The transactions were executed via the [Guardian multi-sig](https://app.safe.global/transactions/history?safe=eth:0x0C2553e4B9dFA9f83b1A6D3EAB96c4bAaB42d430) and generated as surplus (1,580,586$ ) since the price of ETH had recently increased. [Discussions were ongoing](https://gov.angle.money/t/protocol-profits-and-slp-redemptions/690) to determine how the excess will be distributed.


### Hedging Agents (HA) Redemption and Profit Distribution

On April 21st, Angle opened the redemption for hedging agents (HA) to settle all transactions at the pre-hack block 16818578. Position holders were [provided information on how to claim](https://anglemoney.notion.site/Angle-HA-redemptions-process-e35a2b428cc84d39b8191f07a3b41940) their tokens from a multisig contract via Etherscan contract interface.

At the time of writing this article, a [snapshot vote](https://snapshot.org/#/anglegovernance.eth/proposal/0x7c1aad3b8293a5cb15af1d8f80ccead11c84f6e61fe6b1dc7e719f1f22e831bc) is underway to determine the distribution of excess profit generated by the ETH to USDC swaps to the Stablecoin Liquidity Providers (SLP). The vote currently favors immediately paying SLPs 30% of the USDC profit proportional to the $ value of their sanTokens as of the 24th of April 2023. The staking module withdrawal function is expected to be reinstated shortly after that.


### Unpause agEUR Burn

As a measure to bring agEUR back to peg, a [vote concluded](https://snapshot.org/#/anglegovernance.eth/proposal/0xd4957f5c97c09883dcde69afbb27cdf68d1636a8a6fc7958fd68a5c5f0669482) to unpause agEUR burn on April 18th. This would allow arbitrageurs to buy agEUR off the market and redeem through the protocol for €1 (minus a .5% burn fee). Since unpausing, agEUR has experienced ~$4m of redemptions.

![Screen Shot 2023-04-27 at 2 49 39 PM](https://user-images.githubusercontent.com/51072084/235000842-5aab0cea-b37a-401f-9f2c-197bc7ef7917.png)

Source: [Angle Analytics](https://analytics.angle.money/#/agEUR)


## Lessons Learned and a Path Forward for V2

On April 5th, 2023, Angle published [Euler Hack Reflection: Learning, Adapting, and Strengthening Angle Protocol](https://www.angle.money/#/blog/announcements/euler-hack-reflection-learning-adapting-and-strengthening-angle-protocol) which gives a glimpse of a possible way forward for the Angle Protocol. 


### Vote on agEUR Holders' Seniority

The Euler hack prompted a [governance discussion](https://gov.angle.money/t/moving-forward-with-angle-post-euler-hack/647) on the need to define the order of "seniority" among different token holders of the Angle Protocol, not only for the Euler situation but also for any future occurrences of losses not automatically handled by the Core Module's smart contracts. This seniority concept was not explicitly stated before the exploit, only being obliquely alluded to in the document ([collateral settlement section](https://docs.angle.money/angle-core-module/other-aspects/collateral-settlement#use-cases)).

Angle governance considered various seniority options and ultimately decided that by prioritizing agEUR holders over Standard Liquidity Providers (SLPs) and Hedging Agents (HAs), the protocol could ensure its pegged value is defended using the available collateral reserves. 

After deliberations, the proposed seniority of agEUR holders was submitted for a [snapshot vote](https://snapshot.org/#/anglegovernance.eth/proposal/0xd5404ecbd1e76a01c9991bcac74b631d562e0445a1998ea407678391ad95474a), resulting in veANGLE holders supporting the seniority of agEUR holders.


### Re-Launching the Protocol

The ongoing discussion surrounding the protocol's future includes the introduction of a Price Stability Module 2.0. This proposed module would support agEUR by backing it with a diversified basket of various Euro-denominated stablecoins and financial instruments. This enhanced system would be adept at managing stablecoin depegs. It would operate in tandem with the Borrowing module and algorithmic market operations. As part of this plan, the protocol would divest itself of USDC reserves, with additional details to be disclosed shortly.

In light of these observations, a discussion has been initiated on the Angle Governance Forum ([Ideas for re-launching the protocol - thread](https://gov.angle.money/t/ideas-for-re-launching-the-protocol/674)) to explore options for safely reopening the protocol after the hack while repayments are still pending, as well as refining the Angle Core module design. The community is encouraged to actively participate in these discussions, sharing insights and collaborating to build a more secure and robust future for the Angle Protocol.

The recent pause of the Core Module presents an opportune moment to address existing technical debt, paving the way for a more resilient, robust, and scalable system. Before the Euler hack, the Core Module's limitations were exposed when a rapid decline in USDC's price led to the liquidation of most hedging agents. This left the protocol inadequately hedged and resulted in a significant decrease in the protocol surplus. The absence of effective hedging mechanisms within the Core Module underscores the need for a system to withstand such events and manage unfavorable USD/EUR price fluctuations. Enhancements should ensure agEUR price stability and foster a more resilient and scalable protocol. Angle also mentioned that preparing transaction payloads for different scenarios could improve their emergency response.

On April 14th, 2023, further precisions were provided on the possible way forward for Angle V2 through the governance forum thread [Angle Protocol - Core Module V2](https://gov.angle.money/t/angle-protocol-core-module-v2/). Essential requirements for the future system include low slippage for mints and burns, scalability, autonomy during unforeseen events, equal treatment of stablecoin holders, ability to accept Euro-denominated assets, proper risk diversification, and no exposure to foreign exchange risk.


### Future Risk Management

The Angle protocol's previous reserve risk management strategy has proven ineffective. Relying solely on allocating funds based on pure APY is at odds with best practices in risk management and statistical analysis. Furthermore, Angle sought yield from a singular source, specifically lending and borrowing platforms, which carry the potential risk of becoming illiquid during certain periods.

On the other hand, Angle has demonstrated excellent emergency response to the Euler exploit, successfully halting the Core Module to avert further financial shortfalls.

Risk assessment of agEUR in the context of the Curve pool and associated risks to LPs will depend on changes implemented by the Angle team. Llama Risk will re-evaluate Angle Protocol after the V2 has been live for some time.


## Conclusion

In conclusion, the Euler exploit highlights the importance of robust risk management strategies and the potential dangers of composable DeFi legos. Had Euler not been able to recover the funds, the consequences for Angle could have been disastrous. The Euler incident has caused considerable disruptions in agEUR associated Curve pools, resulting from a significant net outflow of euro-denominated stablecoins (>€8m). The full effect on the global euro-stablecoins ecosystem has yet to be determined.

The upcoming release of Angle Protocol V2 will need to address the weaknesses exposed by the Euler exploit, with discussions surrounding the introduction of a Price Stability Module 2.0 and a diversified basket of Euro-denominated stablecoins and financial instruments. However, this challenge presents an opportunity to enhance the protocol's resilience and scalability.

Angle Protocol's transparency and prompt response demonstrate its commitment to addressing the issues and seeking solutions to strengthen the system. As an integral part of the Euro stablecoin ecosystem, Angle plays a vital role in diversification, enabling users to hedge against risks associated with more centralized stablecoins. We are fortunate, in this case, that this crisis can serve as a learning experience without the loss of user funds, and it's our hope to see Angle come back stronger.

