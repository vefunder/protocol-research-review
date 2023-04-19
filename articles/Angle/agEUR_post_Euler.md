# Consequences of the Euler Exploit for Angle's agEUR and Euro-Denominated Curve Pools

## Useful links

* [Angle Website](https://www.angle.money/#/) | [General Docs](https://docs.angle.money/overview/readme) | [Developer Docs](https://developers.angle.money/)
* [Angle Governance Forum](https://gov.angle.money/) | [Snapshot page](https://snapshot.org/#/anglegovernance.eth)
* [AgEUR - LlamaRisk Assessment (Jan 2022)](https://cryptorisks.substack.com/p/ageur-angle-protocol)
* [AIP-43: Improve Angle Yield Strategies for USDC and DAI](https://snapshot.org/#/anglegovernance.eth/proposal/0xb1b4d98c080ec587b2563a6aaa6f854e0a42ce6881f61bced62cf9fa8ae42898)
* [Euler exploit analysys](https://medium.com/@numencyberlabs/a-detailed-analysis-of-Euler-finances-196-million-flash-loan-attack-81cdef370024)
* [AgEUR/EUROC Factory Pool](https://curve.fi/#/ethereum/pools/factory-v2-164/deposit) | [3EURpool Factory Pool](https://curve.fi/#/ethereum/pools/factory-v2-66/deposit)
* [Angle Protocol - Q&A Regarding Euler Exploit](https://anglemoney.notion.site/Angle-Protocol-Q-A-Regarding-Euler-Exploit-03af18cbe5e84430b3341b145554492e)
* [Angle Protocol - Ideas for re-launching the protocol](https://gov.angle.money/t/ideas-for-re-launching-the-protocol/674)
* [Euler Hack Reflection: Learning, Adapting, and Strengthening Angle Protocol](https://www.angle.money/#/blog/announcements/euler-hack-reflection-learning-adapting-and-strengthening-angle-protocol)

## Introduction

In a [previous article](https://cryptorisks.substack.com/p/ageur-angle-protocol), we explored the Angle Protocol, an innovative stablecoin framework designed for price stability, over-collateralization, and optimal capital efficiency. The protocol's stablecoin, agEUR, aims to be pegged to the Euro and has demonstrated impressive price stability compared to other Euro-based stablecoins. This report delves into the Euler Finance exploit that occurred on March 13th, 2023, examining its impact on the peg and liquidity on Curve. It also investigates the strategies employed for Angle Core Module's reserves, the crisis management measures Angle took, actions taken following Euler's recovery, and the lessons learned from the incident. Finally, the report provides an updated risk assessment for agEUR and its related Curve pools.

### Key Findings:

- In Q1 2023, Angle modified its reserve investment approach, enabling permissionless proposals for allocation changes and acceptance based solely on APR. However, Angle did not consider additional risk mitigation by diversifying yield sources. During the exploit, Angle had 74% of its USDC reserves invested in Euler.
- Before the hack, Angle had $36.1m (Core Module), $11.8m (SLP), and a surplus (treasury) of $5.6m. The exploit led to Angle Protocol losing approximately 17.6m USDC, leaving agEUR undercollateralized.
- Angle's governance multisigs (Guardian and Governor, despite the relatively high signature threshold) were quick to pause the protocol's critical functions, minimizing further negative impact.
- Although present in the documents, the principle of seniority, which prioritizes agEUR holders over the Stablecoin Liquidity Providers (SLPs) and Hedging Agents (HAs), was not clearly stated and had to be voted on shortly after the exploit.
- The Euler incident caused considerable disruptions in agEUR associated Curve pools, resulting from a significant net outflow of euro-denominated stablecoins (>€8m).
- Euler's successful recovery of the stolen funds led to Angle protocol restoring over-collateralization. Angle may not have survived had the exploited funds been lost, leading to significant losses for SLP and HAs and a dire collateral ratio for agEUR.
- This de-peg event serves as a reminder of the various trade-offs associated with different stablecoin designs. While fiat-redeemable stablecoins may present challenges regarding transparency and regulatory compliance (EURS, EURT, EUROC), crypto-collateralized stablecoins like Angle grapple with capital efficiency concerns and potentially expose users to other protocol risks.

## Revisiting agEUR and Prior Report Findings: Balancing Overcollateralization and Capital Efficiency

The Angle Protocol presents a novel approach to decentralized finance by offering stablecoins backed by diversified collateral. Its primary product, the agEUR stablecoin, has gained recognition due to its unique method of balancing over-collateralization with capital efficiency. This balance is achieved by distributing risks among various market participants, namely: the **Core Module** (CM), **Stablecoin Liquidity Providers** (SLP), and **Hedging Agents** (HAs). The agEUR stablecoin aims to maintain its peg to the Euro by leveraging a diverse collateral pool. Overcollateralization is a fundamental aspect of this design, ensuring that agEUR remains stable even during market fluctuations.

### Market Participants: Roles and Contributions
![](https://i.imgur.com/gUR3jGm.png)

#### Core Module (CM)
The Core Module oversees collateral pools and mints stablecoins. Users can deposit collateral and obtain agEUR in return. At the same time, the Core Module sustains a debt-to-collateral ratio to ensure the stablecoin's over-collateralization.

#### Stablecoin Liquidity Providers (SLPs)
SLPs are providing liquidity that enables the exchange of agEUR with other stablecoins. In return, they receive fees generated from the trades they facilitate. SLPs are instrumental in maintaining agEUR's liquidity and contribute to the overall stability of the stablecoin.

#### Hedging Agents (HAs)
HAs play a critical role in managing risks associated with collateral fluctuations. By purchasing and selling collateral to preserve an appropriate debt-to-collateral ratio, they help maintain agEUR's stability while promoting capital efficiency.

## Angle's Core Module Reserve Allocation and Implemented Changes
The Core module earns interest on these reserves by lending to other platforms, relying on strategies to determine the allocation of funds across various protocols. These strategies contribute to the Core module's appeal for Standard Liquidity Providers (SLPs), enabling interest generation, reserve accumulation, and veANGLE holder incentivization.

### AIP-43 and the Shift in Reserve Strategy

In January 2023, a proposal was [submitted to the Angle governance forum](https://gov.angle.money/t/aip-43-improve-angle-yield-strategies-for-usdc-and-dai/541), aiming to enhance the existing Angle strategies for USDC and DAI. The primary objective was to optimize the distribution of funds across various lending venues to maximize the overall yield.

The proposed system allows any individual to harvest the strategy and suggest a new allocation of funds to the underlying venues. The contract then verifies on-chain whether the proposed allocation results in a greater APR. If the proposal is superior, the contract transitions to the new allocation; otherwise, it retains the existing state and deploys idle funds into the highest-yielding lending venue. This approach supersedes the previous system, where funds were fully allocated to the platform offering the highest APR, which may not be the most efficient allocation.

Angle stated that the main benefit of this change was that the protocol's lending revenues could increase by up to 50% by employing efficient bots to execute optimizations. This would further boost the revenues for SLPs and veANGLE holders. Additionally, implementing the change necessitated only minimal modifications to the strategy engine and related contracts.

![](https://i.imgur.com/B59O32i.png)

In February 2023, [AIP-43: Improve Angle Yield Strategies for USDC and DAI](https://snapshot.org/#/anglegovernance.eth/proposal/0xb1b4d98c080ec587b2563a6aaa6f854e0a42ce6881f61bced62cf9fa8ae42898) was successfully implemented following a favorable snapshot vote. It is also worth noting that this marked the introduction of the GenericEuler strategy, as advertised [in the official announcement](https://www.angle.money/#/blog/announcements/increasing-angle-yield-and-revenue-with-permissionless-off-chain-optimization).

[`OptimizerAPRStrategyV2`](https://github.com/AngleProtocol/angle-strategies/blob/main/contracts/strategies/OptimizerAPR/OptimizerAPRStrategy.sol) (based on Yearn's GenLender contract) was deployed, with the [following whitelisted strategies](https://github.com/AngleProtocol/angle-strategies/tree/main/contracts):
* Generic Aave
* Generic Compound
* Generic Euler

These strategies are regular lenders, meaning they supply the capital to these Lending & Borrowing protocols. Over the next few months, the system performed as intended, with debt allocation being changed periodically but primarily assigned to Generic Euler due to the added EUL incentives.

## Unraveling the Euler Exploit: Timeline, Reserve Implications, and Consequences for Curve Pools

On March 13th, 2023, Euler Finance suffered an exploit resulting in a loss of around $200 million. The vulnerability stemmed from Euler Finance permitting donations without an account health check, specifically in the `donateToReserves` function of the EToken implementation. This issue was introduced through [eIP 14: Contract Upgrades](https://forum.euler.finance/t/eip-14-contract-upgrades/305) and remained on-chain for eight months before being exploited.

Following the attack, Euler's native token, EUL, fell by 50%. The team halted the attack by disabling the EToken module and the vulnerable donation function. Numerous protocols were affected, including Angle, Balancer, Idle, Inverse, SwissBorg, Swivel, Temple DAO, Yield, etc.

### Timeline (UTC)
Below is the timeline of March 13th events and actions taken by Angle:

- 08:50 UTC: Euler protocol is [exploited via flashloan](https://etherscan.io/tx/0xc310a0affe2169d1f6feec1c63dbc7f7c62a887fa48795d327d4d2da2d6b111d) 
- 09:42 UTC: Angle's Governance (4/6 msig) [withdraws 3,360,927 agEUR](https://etherscan.io/tx/0x4ef9d6f7671905057d0473d7c3263af9a0e994430644e0027c3ef4c765ddf7b7)
- 10:03 UTC: Angle's Guardian (2/3 msig) starts [pausing `StableMasterFront`](https://etherscan.io/tx/0x17ac0acd72566eac4cd3e05a8e9c76ba9695701aa562ad1f54a93450175916a7) (Core module)
- 10:48 UTC: `DAI/agEUR`, `USDC/agEUR` & `FRAX/agEUR` perpetuals Gauges [are also paused](https://etherscan.io/tx/0x2f58b0cb411c804c791c873ae7c4af2603b8b26dcba0efababd524eb58b18be9).
- 11:09 UTC: [burns the remaining agEUR](https://etherscan.io/tx/0x46540733e163ba1d5cce04d77bd6395c52c50126fd97c1c78d75f676d099115c) on the Euler contract
- 11:52 UTC: Debt ceiling [updated to 0](https://etherscan.io/tx/0x515b4679db46356bb9a598247095cbafab7dcf1ac5eb7f6ecafad3ea14961140) for the Borrowing module. The core module is fully paused.
- 13:42 UTC: Angle [removes liquidity](https://etherscan.io/tx/0xcf7f9c25034faaa5dcd3c5acd8fde5f06dc586e8ed7272e2368ae0a80af313f5) from `Curve agEUR/EUROC Factory`
- 14:56 UTC: Angle [posted a Q&A](https://anglemoney.notion.site/Angle-Protocol-Q-A-`Regarding-Euler-Exploit-03af18cbe5e84430b3341b145554492e) addressing the exploit
- 15:59 UTC: Reserves funds are [removed from Compound strategy](https://etherscan.io/tx/0x61f20b2bd6ebb5fa567e36ab6a60332a4749f066e931fc22b06e0880658d5f6e) (3,942,967 DAI & 6,126,537 USDC)

### Impact on Angles Core Module's Reserves

During the exploit, Angle's USDC reserves were heavily invested (74%) in the GenericEuler strategies. Fortunately, the DAI reserves suffered no losses as they were only invested in the `GenericCoumpound` strategy.

![](https://i.imgur.com/J2PJUhl.png)

The Euler exploit resulted in **Angle's reserves losing 17,6m USDC** as detailed in the[ Angle Protocol - State of the Protocol spreedsheet](https://docs.google.com/spreadsheets/d/1SYkNR0BYBh4dHKSomJAXHXHZ9RkvYenn0DAvUAdTlJo/edit#gid=1182401585). At the time of the hack, the total supply was 26,418,421 agEUR (excluding the agEUR burned on the Euler contract). The Core Module held the following assets, for an estimated TVL of $36.1m:

* 4.11m DAI
* 24.99m USDC
* 6.9m FRAX
* 1.52 wETH
* 203k FEI

The protocol also had ~$11.8m in deposits from its SPLs, ~$71.0k from the HAs and a surplus of ~$5.58m.

Following the exploit, Angle's liabilities far exceeded assets, and the reserves in the Core module were inferior to the value of the claims of agEUR holders,  Standard Liquidity Providers, and of the remaining hedging agents.

## Effect on Curve pools and agEUR peg

The peg on various Curve pools containing agEUR began to drop rapidly as the news of the exploit began to propagate.

![](https://i.imgur.com/uO98jir.png)

Balances of the two main Curve pools containing agEU, 3EUR & agEUR/EUROC changed considerably as users offloaded agEUR.

![](https://i.imgur.com/JpNz3ug.png)

![](https://i.imgur.com/UeaLY2C.png)

Overall, the Euler exploit contributed to a net outflow of Euro-denominated stablecoins for an amount exceeding €8m.

Other pools that experienced minor effects included: [agEUR/ibEUR](https://curve.fi/#/ethereum/pools/factory-v2-78/deposit), [Euro Pool](https://curve.fi/#/ethereum/pools/factory-v2-145/deposit), [agEUR/EURe](https://curve.fi/#/ethereum/pools/factory-v2-231/deposit) and [agEUR/FRAXBP](https://curve.fi/#/ethereum/pools/factory-crypto-93/deposit). No significant impact has been observed on other euro Curve pools.

We also looked into a [large liquidity removal](https://etherscan.io/tx/0x376a748c4eb1a174adf1de4c6a58ff4a3665196dfe652603c47f4e19b87d43bd) (approximately €1m equivalent) from agEUR/EUROC pool that occurred a few days before the exploit. This appears unrelated to these events and could be a market participant derisking his positions during the USDC depeg event.

## Recovery plan

### Vote on agEUR holders' seniority

The Euler hack prompted a governance discussion on the need to define the order of "seniority" among different token holders of the Angle Protocol, not only for the Euler situation but also for any future occurrences of losses not automatically handled by the Core module's smart contracts and voted parameters. This seniority concept was not explicitly stated before the exploit, only being obliquely alluded in the document ([collateral settlement section](https://docs.angle.money/angle-core-module/other-aspects/collateral-settlement#use-cases)), and voted on by governance after the exploit.

Angle governance argued that by prioritizing agEUR holders over Standard Liquidity Providers (SLPs) and Hedging Agents (HAs), the protocol could ensure its pegged value is defended using the available collateral reserves. 

After deliberations, the proposed seniority of agEUR holders was submitted for a [snapshot vote](https://snapshot.org/#/anglegovernance.eth/proposal/0xd5404ecbd1e76a01c9991bcac74b631d562e0445a1998ea407678391ad95474a), resulting in veANGLE holders supporting the seniority of agEUR holders.

## Funds recovery and Angle proposal to Restart the Protocol

On March 25th, the hacker began to return the stolen funds to the Euler DAO. By April 4th, the entire amount was recovered. The Euler hack affected multiple assets. However, the restored funds were in ETH and DAI only, constituting approximately 97% of the stolen amount (converted to native assets at the time of transfer). This situation prompted several discussions on the best approach for Euler to return the funds to users.

* **Initial exploit balances**: 66.3k wstETH, 34.4m USDC, 849.14 WBTC, 8,099.3 WETH, 8.88m DAI & 3,897.5 stETH.
* **Returned funds**: 95.5k WETH and 43.1m DAI

### Plan v1 and initial feedback
On April 5th, Euler presented its initial recovery plan on the governance forum for discussion. The proposed plan entailed simulating the repayment of all liabilities at the block when the protocol was disabled, calculating the net asset value (NAV) for each account, and allowing users to claim the recovered ETH, DAI, and USDC based on their proportion of the total NAV. A smart contract would be created to facilitate the claims. 

### Plan v2 and redemption contract
Proposal v2 was published on April 11th. The updated redemption plan addressed feedback on the initial proposal and outlined a mechanism to distribute the recovered assets to affected users following the attack. Critical updates include a more detailed calculation of net asset values (NAVs) for each sub-account and an alternate NAV calculation, including potential foregone profits due to the protocol's pause. The redemption contract was activated on the same day at 2:00 UTC.

### Angle receiving its share of the reclaimed funds
In the updated redemption plan, smart contract accounts cannot directly claim their recovered funds through the proposed smart contract mechanism because of potential issues with their internal accounting and inability to execute the `claim` method on the Merkle distributor contract. In addition, ending recovered funds directly to smart contracts may cause problems with their internal accounting, leading to incorrect or inconsistent record-keeping. 

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

This proposal outlined the necessary steps to secure the Angle and agEUR positions after Angle successfully recovered funds from the Euler hack. The primary objective of the vote was to establish a way to maintain agEUR's peg and provide a method for Staking Liquidity Providers (SLPs) to retrieve their funds and repay Hedge Accounts (HAs). 

Two options were proposed for swapping the recovered assets. Both options suggest upgrading the PoolManager contracts, unpausing SLP contracts, and settling HAs. The vote had a 25-hour timespan to allow for a quick swap, as the protocol held long ETH and bears significant exchange risk. This vote is intended as a first step towards getting the protocol back on track, with the ultimate goal of building a better, more scalable, and robust system that can make agEUR a backbone in DeFi.

#### Option 1 - Swap all recovered funds and additional DAI into EUROC

Option 1 suggests swapping the 7,408.8 ETH and 3,416,991 DAI received from Euler's redemption into EUROC and removing 2,700,000 DAI from the Core module's DAI PoolManager contract to be swapped into EUROC. This aims to cancel the protocol's exposure to ETH and USD, backing all agEUR issued through the Core module with EUROC. This option allows the Governor or Guardian multisig to manage the agEUR-EUROC pool liquidity. This option will pause mints and redemptions through the Core module.

#### Option 2 - Swap 50% of recovered ETH into USDC and the other 50% into EUROC.

Option 2 proposes swapping 50% of the 7,408 ETH obtained from Euler into EUROC and the other half into USDC. This option aims to eliminate the protocol's exposure to ETH and partially remove its exposure to USDC. While the Core module could technically be restarted for agEUR holder redemptions after the swap, the protocol may need more USDC to handle all agEUR to be redeemed. The implications of this situation would be decided in a future vote.

### Results
The voting concluded on April 14th, 2023, witnessing substantial participation from the community. Option 2 emerged successful, albeit by a narrow margin.
![](https://i.imgur.com/Pr2kUSP.png)

As the Angle team initially favored Option 1, further plans are awaited from the team. The winning option will partially address the low EUROC liquidity issue. However, the precise volume of agEUR holders seeking to liquidate their positions remains to be determined, pending the unpausing of the Core Module.

### OTC trade

Angle decided to use a combinaison of Paraswap, 1inch and [Keyrock Trading](https://twitter.com/KeyrockTrading) (OTC) to execute their swaps.

![](https://i.imgur.com/OdlRR1p.png)

The transactions were executed via the [Guardian multisig ](https://app.safe.global/transactions/history?safe=eth:0x0C2553e4B9dFA9f83b1A6D3EAB96c4bAaB42d430) and generated as surplus (1,580,586$ ) due to the increased ETH price. [Discussion are ongoing](https://gov.angle.money/t/protocol-profits-and-slp-redemptions/690) to determine how the excess will be distributed.

## Lessons learned and a path forward for V2

On April 5th, 2023, Angle published [Euler Hack Reflection: Learning, Adapting, and Strengthening Angle Protocol](https://www.angle.money/#/blog/announcements/euler-hack-reflection-learning-adapting-and-strengthening-angle-protocol) which give a glimpse of a possible way forward for the Angle Protocol. 

The ongoing discussion surrounding the protocol's future includes the introduction of a Price Stability Module 2.0. This proposed module would support agEUR by backing it with a diversified basket of various Euro-denominated stablecoins and financial instruments. This enhanced system would be adept at managing stablecoin depegs. It would operate in tandem with the Borrowing module and algorithmic market operations. As part of this plan, the protocol would divest itself of USDC reserves, with additional details to be disclosed shortly.

In light of these observations, a discussion has been initiated on the Angle Governance Forum ([Ideas for re-launching the protocol - thread](https://gov.angle.money/t/ideas-for-re-launching-the-protocol/674)) to explore options for safely reopening the protocol after the hack while repayments are still pending, as well as refining the Angle Core module design. The community is encouraged to actively participate in these discussions, sharing insights and collaborating to build a more secure and robust future for the Angle Protocol.

The recent pause of the Core Module presents an opportune moment to address existing technical debt, paving the way for a more resilient, robust, and scalable system. Before the Euler hack, the Core Module's limitations were exposed when a rapid decline in USDC's price led to the liquidation of most hedging agents. This left the protocol inadequately hedged and resulted in a significant decrease in the protocol surplus. The absence of effective hedging mechanisms within the Core Module underscores the need for a system to withstand such events and manage unfavorable USD/EUR price fluctuations. Enhancements should ensure agEUR price stability and foster a more resilient and scalable protocol.

On April 14th 2023, further precisions were provided on potential way forward for Angle V2 throught the governance forum thread [Angle Protocol - Core Module V2](https://gov.angle.money/t/angle-protocol-core-module-v2/). Key requirements for the future system include low slippage for mints and burns, scalability, autonomy during unforeseen events, equal treatment of stablecoin holders, ability to accept Euro-denominated assets, proper risk diversification, and no exposure to foreign exchange risk.

## Risk assessment

The Angle protocol’s previous reserve risk management strategy has proven to be ineffective. Relying solely on allocating funds based on pure APY is at odds with best practices in risk management and statistical analysis. Furthermore, Angle sought yield from a singular source, specifically lending and borrowing platforms, which carry the potential risk of becoming illiquid during certain periods.

On the other hand, Angle has demonstrated excellent emergency response to the Euler exploit, successfully halting the Core Module to avert further financial shortfalls.

Risk assessment of agEUR in the context of Curve pool asset will depend on changes implemented by the Angle team.

# Conclusion

In conclusion, the Euler exploit highlights the importance of robust risk management strategies and the potential dangers of composable DeFi legos. Angle Protocol's transparency and prompt response demonstrate its commitment to addressing the issues and seeking solutions to strengthen the system. As an integral part of the Euro stablecoin ecosystem, Angle plays a vital role in diversification, enabling users to hedge against risks associated with more centralized stablecoins.

The upcoming release of Angle Protocol V2 will need to address the weaknesses exposed by the Euler exploit, with discussions surrounding the introduction of a Price Stability Module 2.0 and a diversified basket of Euro-denominated stablecoins and financial instruments. The reopening of the Core Module will likely result in a decrease in agEUR supply as holders liquidate their positions. However, this challenge presents an opportunity to enhance the protocol's resilience and scalability.

Had Euler not been able to recover the funds, the consequences for Angle could have been disastrous. As an integral part of the Euro stablecoin ecosystem, Angle plays a vital role in diversification, enabling users to hedge against risks associated with more centralized stablecoins.

The Euler incident has caused considerable disruptions in agEUR associated Curve pools, resulting from a significant net outflow of euro-denominated stablecoins (>€8m). The full effect on the global euro-stablecoins ecosystem has yet to be determined and will mostly be felt once the module is unpaused, allowing for the burning of agEUR. 
