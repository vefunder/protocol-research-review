
# **Asset Risk Assessment: DOLA**


#### A deep dive into Inverse Finance’s stablecoin DOLA


# Useful links:


* [Inverse Finance website](https://www.inverse.finance/)
* [Twitter](https://twitter.com/InverseFinance)
* [GitHub](https://github.com/InverseFinance)
* Docs [Smart Contracts](https://docs.inverse.finance/inverse-finance/technical/smart-contracts)
* [Transparency overview](https://www.inverse.finance/transparency/overview) (smart contracts relation)
* [Inverse Finance docs](https://docs.inverse.finance/inverse-finance/about-inverse)
* [Firm Whitepaper](https://www.inverse.finance/firm.pdf)
* Coingecko: [INV](https://www.coingecko.com/en/coins/inverse-finance)
* Coingecko: [DOLA](https://www.coingecko.com/en/coins/dola)
* [Dune Analytics dashboard - DOLA metrics](https://dune.com/naoufel/dola-metrics)
* [2/6 Fed Chair multi-signature wallet](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8)
* PeckShield [audit](https://drive.google.com/file/d/1LWNG08mib2GcI1WqnMt5IdFoW73QU2F8/view) for debt repayment contracts
* DebtRepayer [Contract](https://etherscan.io/address/0x9eb6BF2E582279cfC1988d3F2043Ff4DF18fa6A0#readContract) (Etherscan)
* Debt Converter [Contract](https://etherscan.io/address/0x1ff9c712B011cBf05B67A6850281b13cA27eCb2A) (Etherscan)


# Abstract

Inverse Finance is a protocol that manages and develops multiple DeFi products and tools. Its main products are a lending market and the stablecoin DOLA. The Frontier lending market was paused after two exploits earlier this year. Since then, Inverse has prioritized reducing its bad debt that resulted from these hacks. At the same time, it developed and just released its newest product FiRM (Fixed-Rate Money Market). FiRM is an improved version of Frontier with additional features, increased security, and a new primitive to facilitate fixed-rate borrowing of DOLA. 

DOLA is the hybrid collateralized/uncollateralized stablecoin integrated with all of Inverse's other products. It is partially collateralized by deposits in its lending markets, and partially issued by Inverse to regulate the balance of DOLA in a variety of liquidity pools. During the period after Frontier was paused, all minting and burning of DOLA has been managed by Inverse's Fed Chair multisig. All protocol-controlled DOLA is deployed through the Fed contracts, each of which employ a liquidity pool strategy that deposits DOLA directly into those pools.


### A quick TL;DR of our findings:



* Inverse Finance is focused on developing DeFi primitives and financial tools, such as lending markets, a stablecoin, fixed-interest products, and more.
* The protocol is a rather complex apparatus with many moving parts and a history of swiftly pivoting and introducing new products. Their first product DCA was stopped to develop Anchor (later renamed to Frontier), which was paused and is being replaced by FiRM. The stablecoin DOLA has persisted throughout but has undergone several transformations. It began as a collateral-backed stablecoin. Currently, it’s not backed by exogenous collateral but holds the peg via AMM pool management (through Fed contracts). Currently it is transitioning to a hybrid model by lending DOLA through FiRM with variable and fixed lending rates, in addition to AMM injections via Feds.
* DOLA has liquidity pools on Curve, Balancer, Velodrome (Optimism), and Sushi. Curve and Balancer AMMs are the most significant “stability mechanisms”, as there is almost no exogenous collateral backing the stablecoin.
* Inverse suffered from two exploits in April (~$15.6M) and June (~$5.8M) 2022, due to oracle price manipulation. The deployment of two debt-repayment contracts allows affected users to redeem small portions of their collateral. The income for the repayments stems mostly from Inverses’ bonds (i.e. an Olympus Pro integration). The largest bad debt position is the DOLA portion (approx. $9.5M). 
* After the exploits, Inverse minimized Frontier’s operations and started the development of a new lending protocol called FiRM (Fixed Rate Market). FiRM comes with security improvements (i.e. pessimistic oracle, isolated market pools, borrowing rights). It also has a novel architecture and introduces new DeFi primitives DBRs and PCE.
* $INV is the protocol’s governance token that can be staked (xINV). Both INV and xINV provide voting rights and can be used as collateral in the Frontier lending market.
* The two debt-repayment contracts have undergone an audit by Peckshield. FiRM went through a bug bounty contest at code4arena, whereby some issues of mediocre and low severity were found and resolved.


# Inverse Finance - Introduction

Inverse Finance is building a suite of DeFi tools that are governed by the Inverse Finance DAO. The first product was DCA (Dollar Cost Average) vaults, but the DAO decided to put all resources toward Anchor and their synthetic stablecoin DOLA. Anchor (later renamed to Frontier) is a lending market. Technically it’s mostly a fork of Compound.

DOLA is Inverse’s second main product - a stablecoin soft-pegged to the US-Dollar. The stable was originally issued through Frontier, where users could borrow DOLA against different collateral tokens. For instance, ETH, WBTC, and YFI, as well as yield-bearing tokens such as yvDAI, yvUSDC, or stETH. At its peak in January 2022, Inverse had over [$140M](https://defillama.com/protocol/inverse-finance) in TVL.

However, the protocol stopped all borrowing activities after an exploit in April 2022. The protocol has now pivoted to developing a new product called “FiRM” (Fixed Rate Market). FiRM was just launched on December 16th, 2022. It’s a fixed-rate lending protocol where users can borrow DOLA. Besides FiRM, Frontier (which is [paused](https://docs.inverse.finance/inverse-finance/frontier)), and DOLA, Inverse also provides a swap function called the Stabilizer to swap DOLA against DAI. Moreover, the protocol sells its own governance token $INV, through an Olympus Pro integration. Users can buy [bonds](https://www.inverse.finance/bonds) (i.e. INV at a discount), in return for DOLA or INV-DOLA Sushi LP tokens.

Over the past six months, the team has mostly focused on dealing with the consequences of two exploits. A debt repayment process was implemented to reimburse affected users and reduce the amassed bad debt. Additionally, FiRM is intended to create new revenue streams with a priority on debt repayment. Read further to learn more.


## Inverse Exploits

In April 2022, Anchor suffered from a severe exploit. Via a sophisticated and capital-intensive market manipulation, a hacker was able to withdraw over [$15.6M](https://twitter.com/inversefinance/status/1510282040809299972) in user funds. By successfully manipulating the price of $INV, the hacker heavily inflated the value of INV which was used as collateral. This allowed them to borrow millions from Anchor’s pools. According to [DeFi Safety](https://www.defisafety.com/app/pqrs/199) and [rekt.news](https://rekt.news/inverse-finance-rekt/), the hacker’s rather professional tactic revealed an unforeseen attack vector. In other words, this was not an obvious vulnerability. Altogether, the hacker was able to run away with 1,588 ETH, 94 WBTC, 39 YFI, and 3,999,669 DOLA (worth $15.6M at the time).

The second exploit happened two months later. This time a flash loan attack led to [$5.8M](https://rekt.news/inverse-rekt2/) lost overall. The hacker was able to run away with $1.2M. This second attack was deemed “foreseeable” by DFS. The image below shows the TVL drop after the first exploit.


![Inverse-TVL-DeFiLlama](https://user-images.githubusercontent.com/89845409/210050945-af1b220f-f728-4d52-8c6e-bbff7a0b971b.png)


(source: [DeFi Llama](https://defillama.com/protocol/inverse-finance))

The two attacks left many Anchor users stuck with IOU tokens (called anTokens). Over 24% of the TVL at that time was stolen, leaving Inverse with a significant pile of bad debt. As an initial response, Anchor was paused and the team began working on solutions to repay the bad debt and reimburse affected users. According to RiskDAO’s [dashboard](https://bad-debt.riskdao.org/), Inverse Finance still has over $14.4M in bad debt today.


## Reimbursing Users

At first, Inverse referred to their liquidation engine to repay the debt. But this strategy was only accessible to technically savvy users, as it required the use of liquidation bots. A governance [vote](https://www.inverse.finance/governance/proposals/mills/57) enacted the deployment of the DebtRepayer and the DebtConverter. These two [contracts](https://docs.inverse.finance/inverse-finance/frontier/debt-converter-and-repayer#debt-converter) were designed to pay back all users affected by the hack. 

The DebtRepayer allows holders of the IOU tokens to withdraw the original collateral. Inverse regularly [fills](https://debank.com/profile/0x9eb6bf2e582279cfc1988d3f2043ff4df18fa6a0/history) the [contract](https://etherscan.io/address/0x9eb6BF2E582279cfC1988d3F2043Ff4DF18fa6A0#readContract) with WBTC, YFI, and ETH by forwarding some of their income to DebtRepayer. Users can then withdraw the collected funds. However, there is a discount mechanism that corresponds to the reserves in the pool. Users who are rushing to redeem their collateral will have to accept a reduction. The higher the reserves, the lower the discount rate (starting at 55% discount when the reserve is empty, to 0% discount when the pool has collected 15%+ of the missing funds).

The DebtConverter is the second contract that lets users convert their IOU tokens (anETH, anWBTC, or anYFI) into IOU tokens that are denominated in DOLA. This helps to reduce the volatility of the bad debt, which is denominated in ETH, WBTC, or YFI. Moreover, it lets Inverse pay back the debt in DOLA. The DebtConverter can set a maxConvertPrice for each asset, thus preventing debt increase when prices rise. The DAO regularly [funds](https://debank.com/profile/0x1ff9c712b011cbf05b67a6850281b13ca27ecb2a/history) it with DOLA, which can be withdrawn by holders of anTokens.

Both contracts were [audited](https://drive.google.com/file/d/1LWNG08mib2GcI1WqnMt5IdFoW73QU2F8/view) by Peckshield. Three minor issues were discovered, and all were resolved.  However, some concerns were raised regarding the repayment process. The next section will clarify where the money for debt repayment comes from.


## Repaying Bad Debt

As mentioned above, Inverse regularly refills both contracts. The funds are derived from several income streams. So far the [DebtRepayer](https://etherscan.io/address/0x9eb6BF2E582279cfC1988d3F2043Ff4DF18fa6A0#readContract) has been sponsored with 44.7 WETH, 3.19 WBTC, and 1.08 YFI. In total, roughly $138k worth of collateral tokens. But user interaction with the contract is rather limited. Only 11 addresses have used it. These users accepted a hefty haircut, as there was little funding in the contracts. The image below shows the funding of the DebtRepayer (green arrows) and the withdrawals (red arrows).

![DebtRepayer-Visualizer-Arkham](https://user-images.githubusercontent.com/89845409/210051023-e2d9d644-3cf6-477d-8141-96596c67a611.png)

(source: [Arkham Intelligence](https://platform.arkhamintelligence.com/visualizer/0x9eb6BF2E582279cfC1988d3F2043Ff4DF18fa6A0))


The [DebtConverter](https://etherscan.io/address/0x1ff9c712B011cBf05B67A6850281b13cA27eCb2A) has had similarly sparse usage. Only 10 addresses have withdrawn some DOLA, and mostly in small amounts at a discount. In total $29.5k DOLA has been deposited to this reserve by Inverse.

![DebtConverter-Visualizer-Arkham](https://user-images.githubusercontent.com/89845409/210051063-dfb97027-9d17-49df-92c8-29c7d3016dc6.png)

(source: [Arkham Intelligence](https://platform.arkhamintelligence.com/visualizer/0x1ff9c712B011cBf05B67A6850281b13cA27eCb2A))


In summary, these efforts are honorable, but modest in relation to the total bad debt. So far, roughly 6.5% of the initial bad debt was eliminated over the second half of 2022, mostly through liquidations at a discount. However, there is still more than [$14M](https://www.inverse.finance/transparency/shortfalls) in bad debt remaining.

As seen in the images above, the main source of funding for both debt repayer contracts is the [Treasury Working Group (TWG) multi-sig](https://www.inverse.finance/transparency/multisigs). The second funder is the Inverse [Anchor Treasury](https://etherscan.io/address/0x926dF14a23BE491164dCF93f4c468A50ef659D5B) (the DAO’s main treasury), which also funds the TWG’s multi-sig.

The [TWG](https://debank.com/profile/0x9d5df30f475cea915b1ed4c0cca59255c897b61b) is tasked with managing liquidity and treasury operations. Liquidity operations include deploying funds to various protocols, which can generate multiple income streams. Most income is derived from yield farming. The following is a rundown of the major TWG activities:

* Roughly $469k of TWG funds sit in the [DOLA/INV](https://app.balancer.fi/#/ethereum/pool/0x441b8a1980f2f2e43a9397099d15cc2fe6d3625000020000000000000000035f) Balancer pool staked on [Aura Finance](https://app.aura.finance/#/). This is further incentivized via their vote-locked Aura position (~$204k). Some of the earned $BAL rewards are swapped for $AURA ([example](https://etherscan.io/tx/0x2a038f4b67cd176b0c778323cb689920a31c3ff5fb391fa1f9f08787549ed4b6)).
* About $217k in CVX is vote-locked CVX on Convex, boosting their pool. The earned $CRV is mostly kept and currently the second largest position in their treasury (after USDC).
* The TWG also deposited some $WBTC, $ETH, and $YFI into Frontier, in order to provide exit liquidity for attack-affected users.

The majority of the income used to refund their debt is generated via their Olympus Pro [bonding service](https://www.inverse.finance/bonds). By selling $INV at a discount, the treasury is buying $DOLA as well as INV-DOLA Sushi LP tokens. The $DOLA purchased this way is transferred back to their [main treasury](https://platform.arkhamintelligence.com/explorer/address/0x926dF14a23BE491164dCF93f4c468A50ef659D5B) via the [BondsManager](https://etherscan.io/address/0x9de7b925247c9bd98ecee5abb7ea06a4aa7d13cd) contract. From there it is used to cover operational expenses and to fund the two repayment contracts mentioned above.


# DOLA as an asset

DOLA is a stablecoin soft-pegged to the US-Dollar. DOLA originally started as a hybrid, partially collateral-backed and partially issued through Fed contracts. However, since the deprecation of Frontier, DOLA is no longer backed by any significant exogenous collateral position. Its supply is fully controlled via the Fed multisig and Fed smart contracts that are deployed and controlled by Inverse Finance. Fed contracts define how much DOLA is available for each of its implementations.


## Fed Contracts

From their [docs](https://docs.inverse.finance/inverse-finance/using-dola/dola-faq#where-does-the-dola-that-i-borrow-come-from): “_Inverse Finance Fed contracts mint DOLA directly to the supply side of lending markets, to the DOLA-3CRV[ Curve](https://curve.fi/factory/27) pool, or other fed-enabled liquidity pools in response to market DOLA demand_.”

In short, the minting and burning of DOLA is done by [Fed](https://docs.inverse.finance/inverse-finance/technical/the-dola-fed) contracts. All Feds are controlled by a multi-sig (Fed chair). The contracts have three main functions to control the DOLA supply:



* Expansion - Mints DOLA into the lending side of the pool (lowers borrow APY)
* Contraction - Burns DOLA on the lending side of the pool (increases borrow APY)
* TakeProfit - Transfers all profits earned from borrowers to the Inverse treasury (causes borrow APY % to increase slightly)


![Contractreader-Fed-contract](https://user-images.githubusercontent.com/89845409/210051148-909f42f9-23ce-4b0f-9abd-ba59f487f54a.png)

(source: [Fed.sol](https://www.contractreader.io/contract/0x5E075E40D01c82B6Bf0B0ecdb4Eb1D6984357EF7))

These functions can only be called and operated by the account holding the “Fed Chair” role. The current Fed chair for all DOLA lending pools is a 2-of-6 [multi-sig](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8) wallet. The signers are Inverse team members (see [here](https://www.inverse.finance/transparency/multisigs)).

The protocols website provides an overview of all current and previous Feds (see image below). The Feds holding the most TVL at the time of writing this report are the Yearn, Convex, and Aura Fed.

![Inverse-Fed-Overview](https://user-images.githubusercontent.com/89845409/210051171-d46a06de-a5f1-49c1-8295-c16d6a60c168.png)

(source: [Inverse](https://www.inverse.finance/transparency/feds/policy/all))


## Minting DOLA

Originally, DOLA was issued in exchange for collateral deposited into Frontier. However, this functionality is currently paused for the reasons mentioned above. Currently, DOLA can be added into circulation in the following ways:



* **Borrowing DOLA** - DOLA can be borrowed through FiRM against WETH, or via the supply side of partnering money markets. These integrations are enabled by the Fed contracts. As of today, the only public lending Fed still active is the Fuse6 pool on Rari. The other two Feds are Inverse’s own Frontier (to be deprecated) and the new FiRM product.
* **Liquidity Pools** - The second venue to obtain DOLA is through one of the liquidity pools on Curve or Balancer. The Curve pool acts as the main funnel for adding and removing DOLA into/from circulation, by injecting it into the factory pool: [DOLA/FRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-176/deposit). With over $18.8M in DOLA, the pool is the largest holder of DOLA, closely followed by the Balancer [DOLA-bb-a-USD](https://app.balancer.fi/#/ethereum/pool/0x5b3240b6be3e7487d61cd1afdfc7fe4fa1d81e6400000000000000000000037b) pool (holding $5M DOLA).
* **The Stabilizer** - Finally, DOLA can be minted with $DAI using the Stabilizer contract. A [0.4% fee](https://docs.inverse.finance/inverse-finance/using-dola/how-to-acquire-dola) on each transaction generates some income for the DAO. However, it hasn't accrued any [profits](https://www.inverse.finance/transparency/stabilizer) in the past three months. Nonetheless, it can serve as a protection tool in case of a positive depeg (meaning DOLA is above 1 dollar).

This leaves us with four injection points, the Curve [FraxBP](https://curve.fi/#/ethereum/pools/factory-v2-176/deposit) factory pool, the Balancer [DOLA-bb-a-USD](https://app.balancer.fi/#/ethereum/pool/0x5b3240b6be3e7487d61cd1afdfc7fe4fa1d81e6400000000000000000000037b) pool, the [Rari Fed](https://etherscan.io/address/0xe3277f1102c1ca248ad859407ca0cbf128db0664), and the FiRM Fed.

Only the first two are relevant today. In fact, the majority of the DOLA [supply](https://etherscan.io/token/tokenholderchart/0x865377367054516e17014ccded1e7d814edc9ce4) ($34.7M) is held by Inverse itself. In other words, all DOLA in circulation belongs to Inverse and is lent out via the Fed contracts. A deeper look at the DOLA distribution confirms this:



* The Curve pool holds 53.5% of all DOLA. This was injected via the [Fed](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8) chair.
* The second largest share is [anDOLA](https://etherscan.io/address/0x7fcb7dac61ee35b3d4a51117a7c58d53f0a8a670) (22.9%), which refers to DOLA deposited into Frontier. However, [99.9%](https://etherscan.io/token/0x7fcb7dac61ee35b3d4a51117a7c58d53f0a8a670#balances) of all anDOLA belongs to the AnchorFed (i.e. Inverse itself).
* The Balancer vault (15.8%), the Optimism bridge (3%), and the Fantom bridge (1.4%) make up to rest of the top 5. They were, again, injected with DOLA via the [Fed](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8) chair.

Altogether the top 5 DOLA positions account for almost 97% of the total circulating supply. All of these pools or vaults are controlled through Inverse Fed contracts.

The largest “external Fed” is a [Yearn](https://etherscan.io/token/0xd4108bb1185a5c30ea3f4264fd7783473018ce17?a=0xcc180262347F84544c3a4854b87C34117ACADf94) vault. This private vault is part of an agreement between Yearn and Inverse. Yearn manages this vault and provides the counterparty assets (i.e. $FRAX and $USDC) to the Curve factory pool in return for the majority of pool rewards. Again, Inverse can fund this vault with DOLA ([example 1](https://etherscan.io/tx/0xe6463de40e786a5f26a2f61cac5fecc425dbef97c1b8ce3ea7cf3e5cf147ec51) & [example 2](https://etherscan.io/tx/0xe792d46193a7162c465a394d29be141dab7a08c77e6b66313f05503d0306b109)).

The image below shows the only publicly accessible Yearn vault, which is the almost inactive [DOLA-3pool](https://curve.fi/#/ethereum/pools/factory-v2-27/deposit).

![Yearn-DOLA-vault](https://user-images.githubusercontent.com/89845409/210051211-4a7f0547-110e-40d7-be1d-85f3d0b06eb9.png)

(source: [yearn.finance](https://yearn.finance/vaults))

In conclusion, DOLA paints a rather fragile picture. There is basically no DOLA natively backed by collateral. Frontier, which was supposed to hold DOLA’s collateral, has over 90% of its TVL in $INV and $DOLA. The rest of the [TVL](https://defillama.com/protocol/inverse-finance) (\~$600k) is too little to back the circulating DOLA supply (~$34M). FiRM just released as a guarded launched with negligible TVL at this time. In other words, all DOLA in circulation entered the market via Fed contracts, most notably via Curve and Balancer. The [Fed](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8) chair can simply expand and contract the DOLA supply to these pools.


## DOLA Peg Stability

Inverse keeps DOLA at a 1:1 peg to the US-Dollar and manages its supply with the following three components:



1. **Fed Contracts** - Minting (or recalling) DOLA to Inverse’s own products (Frontier and FiRM) and to external protocols/partners (Curve, Balancer, Yearn, Rari, Aura, Velodrome).
2. **Liquidity Pools** - on Curve ([DOLA/FRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-176/deposit)), Balancer ([DOLA/bb-a-USD](https://app.balancer.fi/#/ethereum/pool/0x5b3240b6be3e7487d61cd1afdfc7fe4fa1d81e6400000000000000000000037b)) and [DOLA/INV](https://app.balancer.fi/#/ethereum/pool/0x441b8a1980f2f2e43a9397099d15cc2fe6d3625000020000000000000000035f).
3. **Amplification Factor** - A relatively high amplification parameter of 200 on Curve and Balancer allows DOLA to be off-balance with regards to the other pool counterparty assets without it losing its peg. This reduces slippage in normal pool operation, but would not protect against a loss of confidence in the peg. 

The Curve and Balancer pools serve as the main liquidity venues. Both are metapools, which allows DOLA to be paired with the base Curve and Balancer pools with deep liquidity (e.g. FRAXBP). Yield farmers who want to use DOLA earn interest on one of the protocols must acquire DOLA from one of the existing metapools.

As mentioned above, metapools are the largest DOLA holders. In the current state, it is enough to use the Curve TWAP price feed, to keep DOLA stablecoin pegged to $1. In addition to the Curve pool, Inverse also uses Balancer’s version of metapool called "Composable Pools". On Balancer, DOLA is paired with the [bb-a-USD](https://etherscan.io/token/0xa13a9247ea42d743238089903570127dda72fe44) base pool (Balancer Aave Boosted StablePool):

![Balancer-DOLA-pool-composition](https://user-images.githubusercontent.com/89845409/210051251-92973256-09b0-4e19-867a-23c4597d6ca2.png)

(source: [Balancer](https://app.balancer.fi/#/ethereum/pool/0x5b3240b6be3e7487d61cd1afdfc7fe4fa1d81e6400000000000000000000037b))

In this composable pool, DOLA has an amplification factor of [200](https://app.balancer.fi/#/ethereum/pool/0x5b3240b6be3e7487d61cd1afdfc7fe4fa1d81e6400000000000000000000037b), similar to the Curve metapool. The Inverse team had proposed in August 2022 to [increase the DOLA Curve pool A parameter from 100 to 450](https://gov.curve.fi/t/proposal-to-raise-a-parameter-on-dola-to-450/4204), although a more modest increase was ultimately agreed upon.

The peg stability during normal operation is strengthened by a high amplification factor (i.e. [A = 200](https://curve.fi/#/ethereum/pools/factory-v2-176/deposit)) in both metapools. This helps to keep the DOLA peg for a larger range of imbalance within the pool. However, the amplification value constitutes a leveraged bet on peg stability, rather than an enforcement of the peg. In times of distress, higher amplification can lead to severely reduced liquidity as the pool balance reaches the shoulder of the bonding curve.


# FiRM - Fixed Rate Market Protocol

After the two exploits of the Frontier lending protocol, Inverse Finance turned to the development of their new lending protocol FiRM, which was launched on December 16th, 2022. FiRM is a lending protocol with many features that are primarily adapted to the ways Inverse functions and the revenue model of the protocol.

There is also a strong emphasis on protocol security, which was the weakest point in Frontier's design. The most significant FiRM features are:

* Fixed interest rates
* DOLA borrowing rights (DBR)
* Personal collateral escrows
* Unlocked governance token utility
* Isolated lending pools by token and account
* Re-design of the oracle price feed and implementation of a pessimistic price oracle (PPO)

The image below displays the system architecture of FiRM:

![FiRM-whitepaper-chartE](https://user-images.githubusercontent.com/89845409/210051312-a471b524-1cd5-4ac9-a264-207f7a255c5b.png)

(source: [FiRM Whitepaper](https://www.inverse.finance/firm.pdf))

The system is built with three foundational smart contracts:



* The [Escrow Contract](https://etherscan.io/address/0xc06053fcad0a0df7cc32289a135bbea9030c010f#code) - initializes the creation of a unique escrow contract for each borrower account and each deployed market in the borrower account.

![Contractreader-SimpleEscrow](https://user-images.githubusercontent.com/89845409/210051337-2e66c859-3d53-4d06-8381-1d34201c7f8f.png)

(source: [SimpleERC20Escrow.sol](https://www.contractreader.io/contract/0xc06053fcad0a0df7cc32289a135bbea9030c010f))



* The [Market Contract](https://etherscan.io/address/0x63df5e23db45a2066508318f172ba45b9cd37035#code) is a central contract of the FiRM protocol and contains almost all core logic (e.g. set admin roles, lending, collateral, borrowing functions, and set lending parameters). It also has a call function for the borrowerController (setBorrowerController), escrow contract (getEscrow), and pauseBorrows function that can be called by the pause guardian or governance.

![Contractreader-Market-Sol](https://user-images.githubusercontent.com/89845409/210051357-f34113f1-7dca-4ec0-8e1d-30fdd2862450.png)

(source: [Market.sol](https://www.contractreader.io/contract/0x63Df5e23Db45a2066508318f172bA45B9CD37035))

Both the Pause Guardian and governance can pause borrowing for a market, but only governance can unpause.



* [DBR Contract](https://etherscan.io/address/0xad038eb671c44b853887a7e32528fab35dc5d710#code) (DBR.sol) is an ERC-20 token (non-standard) that gives the holder the right to borrow DOLA at a 0% interest rate and burn 1 DBR token for 1 borrowed DOLA per year.


![Contractreader-DolaBorrowingRights](https://user-images.githubusercontent.com/89845409/210051398-cca693ad-d553-465e-bb52-e48f3c166bd2.png)

(source: [DolaBorrowingRights.sol](https://www.contractreader.io/contract/0xAD038Eb671c44b853887A7E32528FaB35dC5D710))

DBRs are a new DeFi primitive that still needs to prove their viability. In short, DBRs are minted by Inverse and sold to the market (later they may be distributed through other means). Holders of DBR are allowed to borrow DOLA at a fixed-interest rate for one year. 1 DBR allows holding 1 DOLA for one year, or 2 DOLA for six months, or 4 DOLA for three months, and so on. The DBR amount in the user's wallet is steadily declining.

If a borrower of DOLA does not have enough DBR in their wallet, additional DBR will be added by a third party, according to the [docs](https://docs.inverse.finance/inverse-finance/dbr-dola-borrowing-rights). The costs of the DBR recharging are paid by increasing the DOLA loan of the borrower. In addition, the DBRs added via recharging are at a premium to the market price.

In other words, the interest rate of DOLA is now defined by the price of DBRs. Thus, allowing fixed-interest rates, as the DBRs are bought in advance and then used up (as long as the loan is not expanded over time).


Another newly deployed smart contract is the FiRM [Fed contract](https://etherscan.io/address/0x2b34548b865ad66a2b046cb82e59ee43f75b90fd#code), which has the same functions as the old one used for Frontier. The new [oracle contract](https://etherscan.io/address/0xabe146cf570fd27ddd985895ce9b138a7110cce8#code) also uses Chainlink’s price feed interface. It’s possible to use other oracles as well and the new Fed uses a pessimistic price oracle (PPO) solution. The PPO uses the lower of either the available collateral prices on Chainlink or the 48-hour low price as observed by the PPO, divided by the collateral factor. The implementation of the new oracle model represents an additional layer of security to the FiRM protocol in the context of manipulations.

![Contractreader-Oracle](https://user-images.githubusercontent.com/89845409/210051432-4be1493c-c2d6-4a4c-b8d0-93877983888e.png)


(source: [Oracle.sol](https://www.contractreader.io/contract/0xaBe146CF570FD27ddD985895ce9B138a7110cce8))


# Risk Vectors

Some of the risks are highlighted below.


## Smart Contract Risk

FiRM has not been audited by a third-party auditing firm. However, there was a bug bounty program running on [Code4arena](https://github.com/InverseFinance/FiRM-code4rena), which attracted high participation (140 wardens). The final [report](https://code4rena.com/reports/2022-10-inverse) reveals no bugs of high severity, but 18 vulnerabilities with a risk rating of medium and 54 issues of low to non-critical severity.

Besides FiRM, the team also recently deployed the two debt repayment contracts mentioned above. Both of the contracts were audited by Peckshield. The [audit report](https://drive.google.com/file/d/1LWNG08mib2GcI1WqnMt5IdFoW73QU2F8/view) did not find any major bugs. Only three minor issues were resolved.

Inverse Finance opened a bug bounty vault on [Hats Finance](https://app.hats.finance/vaults), but the vault is still empty.

As an additional security measure, the Inverse Analytics Working Group has created a sophisticated “[in-house alerting system](https://www.inverse.finance/blog/posts/en-US/a-taste-of-analytics-at-inverse)”, which warns members in relevant working groups of on-chain events.

In summary, there are solid attempts to improve the protocol’s security. But since the newest product has not undergone extensive real-market testing, and it wasn’t audited by a highly reputable auditing firm, it doesn't yet warrant high confidence in its safety.


## Governance Risk

Inverse Finance is a DAO governed by its team and the Inverse Community. Officially, decision-making is in the hands of $INV holders via on-chain voting. INV and staked INV (xINV) tokens represent voting rights. Voting rights can also be delegated.

To submit an on-chain proposal, one needs to hold a minimum of 1900 votes. Voting lasts for three days. To pass, the proposal requires a minimum quorum of 9500 votes and a majority of votes FOR. The community has a social agreement to discuss and debate proposals for at least 24 hours before on-chain submission. Discussions take place via [Discord](https://discord.com/invite/YpYJC7R5nv) and the [governance forum](https://forum.inverse.finance/).

Some aspects of the protocol are controlled via the GovernorMills module (a fork of Compound’s Governor Bravo).

![Contractreader-GovernorMills](https://user-images.githubusercontent.com/89845409/210051453-3fef42d0-0772-49c8-841f-f4ff11341f0c.png)

(source: [GovernorMills.sol](https://www.contractreader.io/contract/0xBeCCB6bb0aa4ab551966A7E4B97cec74bb359Bf6))


The DAO has control over the following aspects of the protocol:


* The INV governance token
* The treasury (INV tokens, Anchor profits, and protocol-owned liquidity)
* Governance parameters (e.g. quorum, requirements to submit a proposal, etc.)
* Lending and Stabilizer parameters
* Creating “Working Groups” with budgets

However, GovernorMills is not fully permissionless. The GovGuardian has the power to cancel every proposal submitted to GovernorMills. This role is currently held by one address, which appears to be Inverse’s founder (i.e. InverseDeployer).

![Etherscan-governormills-guardian](https://user-images.githubusercontent.com/89845409/210051491-23dab519-2688-45cb-8baf-abb92e89f42e.png)

(source: [Etherscan](https://etherscan.io/address/0xbeccb6bb0aa4ab551966a7e4b97cec74bb359bf6#readContract))

To better manage daily operations, Inverse has many permissioned roles. Most are assigned to working groups via governance proposals. An overview of all working groups and multi-sigs can be seen [here](https://www.inverse.finance/transparency/multisigs) and are listed below:



* **Fed Chair** - Manages and implements Fed policies (2/6 [multi-sig](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8)).
* **Treasury Working Group** - Optimizes the Inverse treasury and manages liquidity operations (Ethereum 3/4 [multi-sig](https://etherscan.io/address/0x9d5df30f475cea915b1ed4c0cca59255c897b61b); Fantom 3/5 [multi-sig](https://ftmscan.com/address/0x7f063f7b7a1326ee8b64acfdc81bf544ecc974bc); Optimism 3/4 [multi-sig](https://optimistic.etherscan.io/address/0xa283139017a2f5bade8d8e25412c600055d318f8)).
* **Policy Committee** - Handles the reward rate policies and has a [BondsManager](https://etherscan.io/address/0x9de7b925247c9bd98ecee5abb7ea06a4aa7d13cd) role (5/9 [multi-sig](https://etherscan.io/address/0x4b6c63e6a94ef26e2df60b89372db2d8e211f1b7)).
* **Growth Working Group** - Manages investments and costs related to growth initiatives (2/3 [multi-sig](https://etherscan.io/address/0x07de0318c24d67141e6758370e9d7b6d863635aa)).
* **Community Working Group** - Works on community participation and on-chain onboarding (2/3 [multi-sig](https://etherscan.io/address/0xa40fbd692350c9ed22137f97d64e6baa4f869e8c)).
* **Analytics Working Group** - Improves protocol and DAO analytics (2/3 [multi-sig](https://etherscan.io/address/0x49bb4559e65fc5f2236780079265d2f8f4f75c03)**).**
* **Risk Working Group** - Creates the toolset necessary to monitor the various money markets. It has a pause guardian role and can pause a market (1/3 [multi-sig](https://etherscan.io/address/0xe3ed95e130ad9e15643f5a5f232a3dae980784cd)).
* **Bug Bounty Program** - Handles rewards for bug bounties (4/5 [multi-sig](https://etherscan.io/address/0x943dbdc995add25a1728a482322f9b3c575b16fb)**).**
* **Inversedao.eth EOA** -  has a governance guardian role which means that it can cancel governance proposals.

It’s worth pointing out that the Fed chair has far-reaching powers. As alluded above, it can mint DOLA into any gov-approved Fed. This contract is managed by a 2-of-6 [multi-sig](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8). A second centralized point is the GovGuardian. As mentioned above, it can veto any proposals. It’s controlled by a single EOA wallet (inversedao.eth).


## Depeg Risk

Apart from a choppy start, the DOLA peg has been quite stable [historically](https://www.coingecko.com/en/coins/dola). As explained above, nowadays DOLA is mostly relying on AMM Fed management of the Curve and Balancer pools to keep the peg stable. In short, Inverse deploys DOLA into these pools and controls the stability with expansion and contraction of the supply for each Fed contract. For the current supply level, this strategy seems to be working fine, but it is questionable whether it's also scalable.

As of December 16, a new stability mechanism was introduced together with FiRM. [Dola Borrowing Rights](https://docs.inverse.finance/inverse-finance/dbr-dola-borrowing-rights). The DBR is a token that can support the stabilization mechanism. When DOLA increases above $1.00, additional DBRs are minted and sold to the market. Thus lowering borrowing costs and creating more demand for DOLA. When DOLA is below the $1.00 peg, DBR’s minting rate is reduced, which incentivizes borrowers to repay their loans and take profits on unused DBRs.

![FiRM-whitepaper-chartC](https://user-images.githubusercontent.com/89845409/210051526-86cd9d53-1d40-44a7-b958-aa2d2ae92430.png)

(source: [FiRM Whitepaper](https://www.inverse.finance/firm.pdf))


## Collateral Risk

As mentioned above, the collateral backing DOLA is negligible. At the time of writing, the collateral on Frontier is mostly $INV and $DOLA (overall [87.12%](https://www.inverse.finance/transparency/overview) of the total TVL). 99.9% of that belongs to Inverse itself. Plus Frontier will be deprecated anyway.

[FiRM](https://www.inverse.finance/firm.pdf)’s initial liquidity, provided as part of an initial guarded launch, is $100k in DOLA. It went live with one market (i.e. WETH) and the DOLA available to borrow might be increased during its guarded launch. FiRM is designed to reduce systemic risks with fully isolated collateral deposits that can’t be borrowed. In that regard, it resembles the CDP design of MakerDAO, with the significant difference that borrowers of DOLA need to hold DBRs as explained above. However, FiRM does not play a role in backing DOLA yet. 

The current collateral situation appears very fragile. DOLA is heavily under-collateralized and the new product, which is designed to increase collateral and demand for DOLA, just launched.


## Oracle Risks

On 11 December 2022, Inverse Finance deployed a new [oracle](https://etherscan.io/address/0xabe146cf570fd27ddd985895ce9b138a7110cce8#code) model - the “Pessimistic Price Oracle” (PPO) based on Chainlink feeds. The PPO reduces the risk of price manipulation by including collateral factors in the pricing formula and by observing the last 48-hour low price.

This provides an improvement to the previous oracle implementation. It needs to be pointed out though, that the first exploit was possible due to $INV’s low liquidity. This allowed the hacker to briefly inflate the price, which was reported correctly by the oracle. The new pessimistic oracle price should prevent these kinds of attacks.


# Discussion



**1. Is it possible for a single entity to rug its users?**

In theory, the 2-of-6 multi-sig controlling the Fed chair could mint infinte DOLA. While we have no reason to believe the Inverse team has any malicious intention, there are theoretical rug vectors that could arise from abuse of Fed chair privileges. For instance:

- Fed chair mints Xm DOLA into curve pool, flashloan Xm USDC into Dola pool, Fed chair burns Xm DOLA from pool, remove USDC and pay back loan.
- Borrow Xm DOLA against ETH collateral, sell Xm DOLA for Xm (-fees and slippage) FRAXBP, Fed chair mints infinite DOLA into the pool, buy DOLA with less FRAXBP, repay loan. (This vector is not currently viable thanks to low borrow limits during FiRM's guarded launch)

Absence of a timelock for mints/burns, the small number of signatures required by the current Fed chair, and the GovGuardian EOA's ability to veto on-chain DAO votes are factors to be aware of when considering the rug potential. 

FiRM on the other hand is non-custodial and it appears not to have any central access points. However, as mentioned above, the product just launched a week into writing this report and it was not the main focus of this risk assessment.



**2. If the team vanishes, can the project continue?**

If all multi-sig signers disappear, it would be challenging, but possible, for the protocol to survive. Access to the TWG treasury would be disabled. It would also cause difficulties for DOLA. The stablecoin requires active management from the Fed chair multi-sig. The Fed chair could be replaced by a governance vote without permission of the multi-sig, but it is questionable at this time whether anyone outside the team could actually manage the complex Fed situation. Inverse is designed to empower trusted actors while retaining a decentralized fallback mechanism. There would certainly be headwinds to restructure, but this contingency has been accounted for.



**3. Does the protocol rely on CRV or other incentives to keep its peg?**

For the large part yes. Curve and Balancer pools are used to keep DOLA’s stability. Without the rewards for these pools, it will be difficult to attract counterparty assets at scale. Members of the Inverse team have publicly recognized that the system's bad debt will likely cause a depeg in the absence of incentives.

![DOLA CRV incentives](https://media.discordapp.net/attachments/1037395545506984067/1059870781653397606/7B929F76-7121-4CEC-9FD3-E56EB69A024E.jpg)

The DOLA/FRAXBP pool has been incentivized by bribes, with an average weekly allotment of [$28k in INV over the past 5 weeks](https://votemarket.stakedao.org/analytics), and is the 4th most bribed pool on the StakeDAO Votemarket.


**4. Do audits reveal any concerning signs?**

The repayment contracts were audited by Peckshield and didn’t show any concerning signs. The newly deployed product FiRM underwent a Code4rena contest, with no severe bug or vulnerability findings (18 mediocre and 54 low-impact issues). Code4rena is a crowd-sourced auditing platform, and may not match the quality offered by a reputable auditing firm.


# Conclusion

Inverse Finance is a rather complex protocol with many moving parts and a difficult history. The pace at which new products are added and removed is impressive but also peculiar. For a short period, the lending market Frontier seemed to attract a good amount of TVL. However, this mostly vanished after two exploits. Since then, Inverse paused most lending services, and the recently launched FiRM (as well as DOLA, for that matter) still have to prove their product market fit.

The protocol puts a lot of effort into creating transparency, but at the same time manages to be rather intransparent through complexity. For example, the management of DOLA supply expansion and contraction is more or less a black box. There is partially outdated information on the website and in the docs. Users have to manage a flood of data and despite all the information, Inverse manages to cloud the collateral situation around DOLA (e.g. the effective collateralization ratio is not visible anywhere).

Some aspects regarding the management of DOLA raise questions. Most concerning is the fact that almost all DOLA in circulation was just minted by Inverse, and injected into Feds without the backing of any exogenous collateral (i.e. not $INV or DOLA itself). It is only backed by the counterparty assets in the liquidity pools. While this mechanism might work to keep the DOLA at peg in the short term. It is questionable whether this strategy works at scale or during hostile market conditions. What speaks for them, is the fact that DOLA has not deviated after the exploits and kept its peg well during the volatile and bearish year of 2022. However, DOLA is dependent on incentivized pools (Balancer and Curve) plus it requires active management from the team. It’s not a trustless stablecoin that functions without regular intervention.

As mentioned a few times, the protocol is putting efforts into reducing its bad debt, while simultaneously developing new products. This is honorable. However, the protocol is taking rather risky approaches here and there to speed up its debt reduction.


# Risk Team Recommendation

Given the state of Inverse's substantial bad debt, leading to the majority of DOLA in circulation being uncollateralized, creating a cycle of dependence on CRV gauge emissions and other yield farming strategies to support the peg, we question the organic viability of the stablecoin. The release of the FiRM lending platform is a step in the right direction toward sustainability, but it is too early to determine the product's success. Although we appreciate the efforts made by the Inverse team to provide transparency and to protect its users in the wake of the previous exploits, we do not believe DOLA currently merits a gauge. We recommend to kill the DOLA gauge until it has achieved 100% collateralization and has demonstrated itself to be organically sustainable. The status of its Curve gauge does not preclude Inverse from incentivizing in their own token while on their path to sustainability.

# Sources



* [Inverse Finance website](https://www.inverse.finance/)
* [GitHub](https://github.com/InverseFinance)
* Docs [Smart Contracts](https://docs.inverse.finance/inverse-finance/technical/smart-contracts)
* [Diagram overview](https://www.inverse.finance/transparency/overview) (Smart Contracts relation)
* [Inverse Finance docs](https://docs.inverse.finance/inverse-finance/about-inverse)
* [Firm Whitepaper](https://www.inverse.finance/firm.pdf)
* Coingecko: [INV](https://www.coingecko.com/en/coins/inverse-finance)
* Coingecko: [DOLA](https://www.coingecko.com/en/coins/dola)
* Rekt.news [Exploit 1](https://rekt.news/inverse-finance-rekt/)
* Rekt.news [Exploit 2](https://rekt.news/inverse-rekt2/)
* DeFi Safety [Score](https://www.defisafety.com/app/pqrs/199)
* [Dune Analytics dashboard - DOLA metrics](https://dune.com/naoufel/dola-metrics)
* [2/6 multi-signature wallet](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8)** **Fed Chair multi-sig
* [Proposal](https://www.inverse.finance/governance/proposals/mills/57) to activate bad debt repayment contracts
* DebtRepayer [Contract](https://etherscan.io/address/0x9eb6BF2E582279cfC1988d3F2043Ff4DF18fa6A0#readContract) (Etherscan)
* Debt Converter [Contract](https://etherscan.io/address/0x1ff9c712B011cBf05B67A6850281b13cA27eCb2A) (Etherscan)
* PackShield [audit](https://drive.google.com/file/d/1LWNG08mib2GcI1WqnMt5IdFoW73QU2F8/view) for repayment contracts
* Anchor [Fed](https://debank.com/profile/0x5e075e40d01c82b6bf0b0ecdb4eb1d6984357ef7)
* FiRM [Fed](https://etherscan.io/address/0x2b34548b865ad66a2b046cb82e59ee43f75b90fd#code)
* [Fed](https://etherscan.io/address/0x8f97cca30dbe80e7a8b462f1dd1a51c32accdfc8) chair (2-of-6 msg)
* [GovernorMills](https://etherscan.io/address/0xbeccb6bb0aa4ab551966a7e4b97cec74bb359bf6#readContract)
* Treasury Working Group [Multi-sig](https://debank.com/profile/0x9d5df30f475cea915b1ed4c0cca59255c897b61b?chain=eth)
* Inverse main [treasury](https://debank.com/profile/0x926df14a23be491164dcf93f4c468a50ef659d5b?chain=eth)
* DOLA (13 days vesting) bond [contract](https://etherscan.io/address/0xBfB90b9CE47F36841c776a1B82EE49157D4c746b)
* INV-DOLA SLP BOND (6 days vesting) [contract](https://etherscan.io/address/0x34eb308c932fe3bbda8716a1774ef01d302759d9)
