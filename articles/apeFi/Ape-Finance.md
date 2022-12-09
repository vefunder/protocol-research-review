
# Asset Risk Assessment: ApeUSD

A look into ApeCoin’s stablecoin ApeUSD


# Useful links:



* ApeFinance [Docs](https://docs.ape.fi/)
* ApeCoin DAO forum ([AIP-83](https://forum.apecoin.com/t/aip-83-proposal-for-apefi-to-receive-apecoin-token-grant-ecosystem-fund-allocation/7447/52))
* Ape Finance APE [Oracle](https://etherscan.io/address/0xe6189702244CF82444eff2175d7E53951810Fa72#code)
* Ape Finance Timelock ([Timelock7days](https://etherscan.io/address/0x4f45fdb51E942626368F51b89b1E98144eA90376), [Timelock4h](https://etherscan.io/address/0xa5E991d070125c2E1D9893601D2c7F5e61337774#code), and [Timelock3h](https://etherscan.io/address/0x29cE1b3DF9dFefeaF80e964944C0e6dbB9Aa42D7#readContract))
* Ape Finance Developer Fund [Multisig](https://etherscan.io/address/0x02cA76E87779412a77Ee77C3600D72F68b9ea68C#readProxyContract)
* Amount of APE collateral deposited: [apeApe Token](https://etherscan.io/address/0xcaB90816f91CC25b04251857ED6002891Eb0D6Fa#readProxyContract) (ApeERC20Delegator)
* Amount ApeUSD available to borrow: [apeApeUSD Token](https://etherscan.io/address/0xc7319dbc86a121313bc48b7c54d0672756465031/advanced#code) (ApeErc20Delegator)
* Curve Metapool: [apeUSDFRAXBP](https://etherscan.io/address/0x04b727c7e246ca70d496ecf52e6b6280f3c8077d) (pool transaction/rebalances [here](https://etherscan.io/address/0x04b727c7e246ca70d496ecf52e6b6280f3c8077d#tokentxns))
* Ape Finance [Github](https://github.com/ape-fi)
* Ape Finance [Snapshot](https://vote.ape.fi/#/)
* [DeFi Llama](https://defillama.com/protocol/ape-finance?showMcapChart=false)
* ApeFi [Twitter](https://twitter.com/apedotfi)
* ApeFi NFT [Twitter](https://twitter.com/apefinfts)
* [Medium](https://medium.com/@ApedotFi)
* Coingecko [APEFI](https://www.coingecko.com/en/coins/ape-finance)
* Coingecko [ApeUSD](https://www.coingecko.com/en/coins/apeusd)


# Abstract

Ape Finance is a stablecoin lending protocol. It enables ApeCoin (APE) token holders to deposit APE as collateral and borrow ApeUSD against it. ApeUSD is soft-pegged to the US-Dollar. Ape Finance aims to bring DeFi to the BAYC community and unlock capital efficiency, new yield farming opportunities, and other use cases for APE holders. The protocol applies Automated Market Operations (AMO) via a Curve metapool to ensure liquidity and peg stability.


### A quick TL;DR of our findings:



* Ape.fi was created by anonymous developers to enable additional utility for ApeCoin ($APE). ApeCoin is the native ERC-20 token of the ApeCoin DAO. The token was airdropped to BAYB and MAYC NFT holders.
* In May 2022, Ape.fi introduced $ApeUSD, a stablecoin soft-pegged to the US-Dollar, that can only be borrowed against ApeCoin. ApeUSD is also available via a Curve metapool.
* Ape Finance limits the total supply of $ApeUSD (currently 15M) and uses the majority share - that is not borrowed - to provide liquidity to their Curve metapool [apeUSD/FRAXBP](https://etherscan.io/address/0x04b727c7e246ca70d496ecf52e6b6280f3c8077d#readContract). They apply a concept similar to Curve AMOs (Automated Market Operations) pioneered by Frax. So effectively the majority of ApeUSD’s circulating supply is not backed by APE, but deposited into the metapool by Ape Finance.
* The key “trick” behind $ApeUSD’s stability is to maintain the balance inside the metapool, by adding and removing $ApeUSD when an imbalance occurs. The gated access to $ApeUSD via the lending protocol and its limited supply prevent extreme imbalances. For an outsized inflow of $ApeUSD into the pool, it's necessary to borrow $ApeUSD from Ape Finance (which they can control via parameter setting and supply available for borrowing). An outsized inflow of counterparty assets (FRAX or USDC) can also be countered by minting and adding more ApeUSD.
* The protocol also issued its own native token $APEFI, which is used to reward liquidity providers. Other than that, $APEFI only functions as a currency to buy Ape.fi NFTs. Those NFTs are intended to serve as governance tokens. However, as of today, there are no governance processes in place. While in the process of shifting to decentralized governance, the anon team still has full control over the protocol. Furthermore, 97% of $APEFI’s total supply sits in a [multi-sig](https://debank.com/profile/0x02ca76e87779412a77ee77c3600d72f68b9ea68c) controlled by the same group.
* Ape Finance applies a „Protocol-Owned-Farming” strategy. They own most of the Curve metapool and stake those LP tokens to earn liquidity incentivizes. The protocol is rewarded chiefly by accumulating CRV, CVX, and FXS. Some of the rewards are vote-locked in order to further incentivize the metapool.
* Ape Finance has not been audited. The smart contracts are mostly forks of Compound, Cream, and Fixed Forex (by Iron Bank). There seems to be no community activity or governance process that would allow anyone outside the team to influence the protocol on any level.


# Ape Finance - Introduction


## A short retrospect

In April 2021, [Yuga Labs](https://www.yuga.com/) released a series of 10-thousand NFTs called the Bored Ape Yacht Club ([BAYC](https://boredapeyachtclub.com/#/)). The NFT collection became extremely popular very quickly. Many celebrities started to buy them. Soon bored apes would sell for as high as [$3M](https://blockgeeks.com/5-most-expensive-bored-ape-yacht-club-nfts/#:~:text=The%20most%20expensive%20BAYC%20NFT,its%20combination%20of%20rare%20traits.) for one piece. Building upon this success, Yuga Labs released their second NFT collection, the Mutant Ape Yacht Club ([MAYC](https://boredapeyachtclub.com/#/mayc)). And in March 2022, the ApeCoin DAO issued its native token [ApeCoin ($APE)](https://www.coingecko.com/de/munze/apecoin).

15% or 150M of the newly created $APE tokens were airdropped to BAYC and MAYC holders. It should serve as the governance and utility token of the ApeDAO ecosystem.


## Ape Finance Lending Platform

[Ape Finance](https://ape.fi/borrow) (Ape.fi) is a stablecoin lending protocol created by a group of anonymous developers active in ApeCoin DAO, to which they applied for a $250k [grant](https://forum.apecoin.com/t/aip-83-proposal-for-apefi-to-receive-apecoin-token-grant-ecosystem-fund-allocation/7447). The goal of Ape.fi is to unlock capital efficiency for APE holders, by offering them the option to borrow a stablecoin (ApeUSD) against their APE tokens, and earn yield on the borrowed funds.

On a technical level, Ape.fi is mostly a fork of Compound with some elements of Fixed Forex (code written by Andre Cronje). Ape Finance currently only supports one lending pool ($APE) and one borrowing counterpart ($ApeUSD). Unlike Compound’s cTokens (the IOU token that users receive when lending tokens), Ape.fi issues [$apeAPE](https://etherscan.io/address/0xcaB90816f91CC25b04251857ED6002891Eb0D6Fa#readProxyContract) tokens that don’t accrue any interest but only serve as a receipt for the deposited collateral.


## $APEFI and ApeFi NFTs

Ape Finance also issued its own ERC-20 token called [$APEFI](https://www.coingecko.com/en/coins/ape-finance). The token is supposed to further incentivize contributions to the protocol and function as a reward token for liquidity providers. Furthermore, $APEFI serves as the only currency for buying ApeFi NFTs according to their [docs](https://docs.ape.fi/apefi-nfts). These [NFTs](https://twitter.com/apefinfts) are then used for governance (1 NFT = 1 vote). In other words, users have to decide whether they want to keep the financial reward or participate in governance. 

There are, however, some inconsistencies with regard to those plans. At first, the plan was to airdrop 5% of the token to all participants of the [AIP-83](https://forum.apecoin.com/t/aip-83-proposal-for-apefi-to-receive-apecoin-token-grant-ecosystem-fund-allocation/7447) Snapshot vote, which would get Ape.fi the grant from ApeCoin DAO. This was also described in a [blog post](https://medium.com/@ApedotFi/and-then-there-was-defi-39bcd7ef1565), where the distribution of $APEFI was further detailed (5% initial bootstrap, 5% airdrop, 25% contributors, 65% community/treasury).

![tweet-token-distribution](https://user-images.githubusercontent.com/89845409/204541991-6e1f440f-13ab-41ca-835b-fb6d05fbe9d0.png)

(source: [Twitter](https://twitter.com/apedotfi/status/1525029388688842752?s=20&t=_MD08E0P7LQNnZurdMCN9A))

However, ApeCoin DAO never created the Snapshot vote for the grant that Ape.Fi asked for. According to the Ape.Fi team, several attempts for submitting the grant to a vote were ignored. Hence, the team plans to pivot from the ApeCoin ecosystem and aims to enable additional assets to collateralize ApeUSD.

Essentially, the token distribution as described above never took place. At the time of writing this article, 97.25% of all $APEFI tokens still sit in the initial [contract](https://etherscan.io/token/0x4332f8A38f14BD3D8D1553aF27D7c7Ac6C27278D#balances), which is controlled by the team. And ~2% are in a Uniswap V2 pool.

The failed grant petition notwithstanding, the team received initial capital from [machibigbrother.eth](https://app.zerion.io/0x020ca66c30bec2c4fe3861a94e4db4a498a35872/overview) and from an FTX [address](https://etherscan.io/address/0x2faf487a4414fe77e2327f0bf4ae2a264a776ad2) in mid-May. Machibigbrother.eth is one of the largest BAYC holders, and also a contraversial figure, [having been involved with numerous failed crypto projects](https://medium.com/@investigationsbyzachxbt/22-000-eth-embezzled-and-over-ten-projects-failed-the-story-of-machi-big-brother-jeff-huang-a1ad073fcfa8).

In total, their [developer fund](https://app.zerion.io/0x02ca76e87779412a77ee77c3600d72f68b9ea68c/history) received $150k in USDC and 56,250 $APE tokens (worth ~$452k at that time). These funds were used to bootstrap the initial liquidity on Curve and Uniswap:
* First by buying and deploying $100k FRAX with the USDC, and minting and deploying $200k ApeUSD with the APE tokens ([trx](https://etherscan.io/tx/0xdf7abd4000eeadbe30cf55650ef41d15310f6b903be07589d8f24046c8e2636b)).
* Secondly by deploying $50k ApeUSD (swapped using the remaining 50k USDC) and 8.25M APEFI into a Uniswap V2 pool ([trx 1](https://etherscan.io/tx/0xa809822fc74cff198f173ec414dbafeeca2278f38d0f2c1d463f33ecb1051ea5) & [trx 2](https://etherscan.io/tx/0x4bfbaed890e2f1967d66aee0f2e1e1c039357105826d7e54e0e8adfbac26f0ae)).

Ape.Fi governance has not yet realized its stated roadmap. Contrary to statements in the [docs](https://docs.ape.fi/earn/apefi-token), voting is actually done with $APEFI tokens, not with ApeFi NFTs. The team emphasized that the NFT launch and voting will come in December 2022. As yet, Ape.fi’s [Snapshot page](https://vote.ape.fi/#/) has only been used for a few meta-governance votes from ApeCoin DAO.


# $ApeUSD

$ApeUSD is a stablecoin soft-pegged to the US-Dollar. It can either be borrowed by collateralizing ApeCoin ($APE) or traded on Curve against USDC or FRAX. The team is working on enabling two more assets to be used as collateral.

The first mechanism for maintaining $ApeUSD’s peg is over-collateralized borrowing through the lending protocol. The Loan-to-Value (LTV) is currently set to 60%. The second stability mechanism - also enabling arbitrage - is the [ApeUSD/FRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-138/swap) Curve metapool. Ape Finance has full control over the supply of $ApeUSD. They do this by adjusting the total amount of $ApeUSD available for borrowing through their platform (see image below) and by balancing the amount of $ApeUSD in apeUSDFRAXBP metapool.

![apefinance-apeusd-availability](https://user-images.githubusercontent.com/89845409/204542537-b3da9f68-c4d7-4c52-9a69-3c478955d1f1.png)

(source: [Ape.fi](https://ape.fi/borrow))

The max supply of $ApeUSD is currently set to [15M](https://etherscan.io/token/0xfF709449528B6fB6b88f557F7d93dEce33bca78D#balances). This can change though. At the beginning of November 2022, the supply was at 20M. The team can decide to increase/decrease the supply with a 1-day [timelock](https://etherscan.io/token/0xff709449528b6fb6b88f557f7d93dece33bca78d#readContract).

Only [93](https://etherscan.io/token/0xfF709449528B6fB6b88f557F7d93dEce33bca78D#balances) addresses currently hold $ApeUSD, whereby the Curve pool and  [ApeErc20Delegator](https://etherscan.io/address/0xc7319dbc86a121313bc48b7c54d0672756465031/advanced#code) (smart contract containing the borrowable tokens) account for 99.87% of the total supply.


## Minting $ApeUSD

$ApeUSD is minted through the function _mintBorrow. Very little interaction with the protocol is required. To mint $ApeUSD, users only have to select the amount of $APE they deposit as collateral, and define the borrowed amount. After allowing the deposit, the actual depositing and borrowing can be done in one click.

Ape Finance simplifies the lending and borrowing process by batching all steps into one transaction (deposit APE – mint apeAPE – borrow ApeUSD), shown below. The first image shows the flow of a transaction on Etherscan, and the second is a screenshot of the _mintBorrow function from the ApeTokenHelper smart contract.

![apefinance-batch-mint-apeusd](https://user-images.githubusercontent.com/89845409/204543079-8dba82de-5383-45d5-8c0e-25c907b4d83a.png)

(source:[ Etherscan](https://etherscan.io/tx/0xf315b7b80324cd8059d8ec46947abde536d3fb3834df0a93dc0d212424e461fc))


![contractreader-ape-tokenhelper](https://user-images.githubusercontent.com/89845409/204543181-a9e6155d-5167-459a-b736-ce37f3763e13.png)

(source:[ ApeTokenHelper.sol](https://www.contractreader.io/contract/0xCAAA0C316f8b91F25b35E318f83CaaB4ACD327fB))



![contractreader-apeusd-modifier-g](https://user-images.githubusercontent.com/89845409/204543307-2a8715c1-1d98-4e58-b47c-6a6034d3e079.png)

(source: [ApeUSD.sol](https://www.contractreader.io/contract/0xfF709449528B6fB6b88f557F7d93dEce33bca78D))

The image above displays the G (modifier), which allows the gov-account to mint $ApeUSD. This can only be set by Ape Finance multi-sig with a 1-day delay. The contract is a fork of Iron Bank’s fixed forex contract (for more info check the [Fixed Forex Risk Assessment](https://cryptorisks.substack.com/p/asset-risk-assessment-fixed-forex)).

## Protocol-owned Liquidity

Ape Finance applies a [strategy](https://docs.ape.fi/protocol-owned-liquidity) to use the non-borrowed $ApeUSD actively. They base this off Frax’s Curve [AMOs](https://docs.frax.finance/amo/overview), whereby unbacked $ApeUSD are deployed into Curve. The team's justification for this strategy is that the counterpart assets inside the pool (USDC and FRAX) function as indirect backing. 

This is partially true. The protocol has full control over the $ApeUSD supply and its balance against crvFRAX inside the liquidity pool. To achieve this, they use the [StabilizerV3](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#code) contract, which is [controlled](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#readContract) by the team’s [multi-sig](https://etherscan.io/address/0x02cA76E87779412a77Ee77C3600D72F68b9ea68C). The image below shows that the most called function of the stabilizer is for depositing $ApeUSD into the pool.

![tenderly-top-function-calls](https://user-images.githubusercontent.com/89845409/204542664-1238c577-ddb8-4d4e-ab78-cffba1e9a8dd.png)

(Source: [Tenderly](https://dashboard.tenderly.co/Lavi/project/analytics/4b39bfe3-da64-44d9-b2b8-bef59a70bf17?contract=1%3A0xd5bb32d5745af76510ba705d6063dd1f326aaf24))

The team regularly rebalances the amount of $ApeUSD in the pool. Since they also control the amount of $ApeUSD that can be borrowed, nobody can suddenly mint a large amount of $ApeUSD by surprise. Essentially, Ape Finance fully owns at least 50% of the factory metapool (i.e. the $ApeUSD part), hence the comparison to Frax’s AMO strategy. However, in the case of Frax, the protocol owns the counterparty assets in its Curve AMO pool. The collateral ratio (CR) of FRAX is usually between 85% and 92%. Ape.fi on the other hand, only owns [~$1M](https://debank.com/profile/0xd5bb32d5745af76510ba705d6063dd1f326aaf24) in counterparty assets (FRAX & USDC).

A minority of $ApeUSD is actually borrowed by users and thus over-collateralized with APE. At the time of writing this report, ~[275k](https://etherscan.io/address/0xcaB90816f91CC25b04251857ED6002891Eb0D6Fa) $APE tokens were deposited into the protocol, worth roughly ~$1.1M USD. Given the current LTV of 60%, a maximum of ~660k $ApeUSD could be borrowed. However, the Curve pool holds over 2.87M $ApeUSD. The majority of the tokens were deposited into Curve by Ape Finance, using the tactic described above.

In conclusion, Ape Finance's CR is rather low at roughly 60-70% (it used to be below 40% a few weeks ago). Ape.fi does not provide any insights into the actual CR. However, it can be calculated by looking at their TVL on [DeFi Llama](https://defillama.com/protocol/ape-finance?showMcapChart=false) (~$4M) and comparing the circulating $apeUSD supply with the actual backing ($APE) and the counterparty assets in the pool that is owned by the protocol’s stabilizer contract. 

[Side note: The largest depositor of $APE is this [address](https://debank.com/profile/0xf035af8a3dff1f90aeb21f9623589ae1330e9e09). It’s not clear whether the address is somehow related to Ape Finance. However, it regularly interacts with the protocol.]


## Protocol-owned Farming

To sum it up, most $ApeUSD in the Curve pool was deposited by Ape.Fi. The protocol uses the minted - but unused $ApeUSD - that sits in their smart contracts and deploys them into the Curve pool. This AMO-like strategy allows them to earn CRV, CVX, and FXS incentives (they call it [protocol-owned farming](https://docs.ape.fi/protocol-owned-liquidity/protocol-owned-farming)). Almost all liquidity inside the Curve pool is currently staked on [Convex](https://www.convexfinance.com/stake). And roughly half of it is also staked on [Frax](https://frax.convexfinance.com/). The farmed tokens are regularly claimed by their developer fund, swapped for CVX, and re-locked to Convex for vlCVX (see [history](https://debank.com/profile/0x02ca76e87779412a77ee77c3600d72f68b9ea68c/history)).

With the current yield (image below) and at the current $9.4M as base TVL, they can earn roughly $780k in CRV and CVX per year. If the staked LPs on Frax were also owned by Ape.Fi, it would be an additional ~$330k. However, we don’t know that for sure.

![convex-apeusd-yield](https://user-images.githubusercontent.com/89845409/204542832-6d1ee9a2-42a6-4b95-9136-8a4d6114ebdd.png)

(source: [Convex](https://www.convexfinance.com/stake))

Unfortunately for Curve, the token isn’t drawing much trading volume. Most days there is no activity at all. The image below displays the pool's trading volume. There is only sporadic trading, mostly caused by rebalancing activities.

<img width="693" alt="dex-guru-apeusd-trading-volume" src="https://user-images.githubusercontent.com/89845409/204542917-b86d7a89-31e2-4038-b66c-1918494b742f.png">

(Source: [Dex.guru](https://dex.guru/token/0xff709449528b6fb6b88f557f7d93dece33bca78d-eth/history))



# Risk Vectors

Some of the risks are highlighted below.


## Smart Contract Risk

Ape Finance has not been audited by any third party. On their Github [profile](https://github.com/ape-fi/ape-finance/tree/ape/audits), Ape.fi published an [audit](https://github.com/ape-fi/ape-finance/blob/ape/audits/trailofbits-CREAMSummary.pdf) that was performed on Cream Finance by Trail of Bits (Cream also uses the same smart contracts). However, CREAM is notorious for suffering from a few [exploits](https://decrypt.co/84590/cream-finance-suffers-third-hack-losing-over-130-million) in the past.

The team also refers to a [formal verification](https://github.com/ape-fi/ape-finance/tree/ape/spec/certora/contracts) process provided by Certora Proven. Unfortunately, the repositories they linked mostly refer to Compound’s cTokens (which Ape.fi doesn’t use) and some links are many months older than Ape Finance itself. Thus, it’s doubtful that the verification process was actually performed.

All smart contracts used by Ape Finance, - also those that contain the user funds - are essentially forks of Compound, Fixed Forex, or Cream. Some of these platforms have experienced at least one exploit. There is no active bug bounty program. Moreover, there is no fallback or emergency mechanism in case something goes wrong.

When providing feedback to this article, the team highlighted that their first audit is currently underway.


## Custody Risk

All changes over Ape.fi and its protocol-owned liquidity are controlled by a 2-of-4[ multi-sig wallet](https://etherscan.io/address/0x02cA76E87779412a77Ee77C3600D72F68b9ea68C#readProxyContract) ([signer 1](https://etherscan.io/address/0xA0AEda175B9266B8601D33E9Ef31673cC8cc4982), [signer 2](https://etherscan.io/address/0xA17c86900a9Ece9194F65F0C48046fA62b03396b), [signer 3](https://etherscan.io/address/0xdcc406dA85E263641C5692A9749E08998409fbF5), [signer 4](https://etherscan.io/address/0x056a8152C8ea78Ef73EED50CE3b643b35d953379)). Ape Finance could implement changes that can pose a danger to user funds. Some examples of what can be changed:



1. It can alter collateral factors, fees, or the interest rate
2. It can change the oracle implementation affecting the price
3. It can alter the smart contracts and add maliciously-crafted elements
4. It can adjust the available amount of $ApeUSD, impacting the utilization rate

The Ape Finance team and the multi-sig owners are not doxxed. One signer ([apeficaesar](https://twitter.com/ApefiCaesar)) is an active ApeDAO community member, while the other three signers are just EOAs with no activity. In the discussion on the ApeCoin [governance forum](https://forum.apecoin.com/t/aip-83-proposal-for-apefi-to-receive-apecoin-token-grant-ecosystem-fund-allocation/7447), concerns were raised by other community members regarding the anonymity of the team.


## Oracle Risks

Ape Finance uses Chainlink as the price-feed [provider](https://data.chain.link/ethereum/mainnet/crypto-usd/ape-usd) for $APE. The token price is updated every time Chainlink post a new price on-chain. Relying on off-chain price updates prevents $APE price manipulation inside one block (flash loan attacks). Oracle failure would have a small direct effect on the protocol, since a minority of $ApeUSD is collateralized, but such a black swan could irrevocably damage confidence in the peg.


## Depeg Risk

$ApeUSD’s price [history](https://www.coingecko.com/en/coins/apeusd) since inception is relatively stable. As eluded earlier, since $ApeUSD is not collateralized, Ape Finance needs to maintain the balance between tokens inside the pool with its [Stabilizer smart contracts](https://github.com/ape-fi/stabilizer). They simply deposit the unused borrowing power from the lending protocol via the stabilizer contract, which is based on two admin rebalance functions:



1. **depositAndStake**- when the price of $apeUSD  >$1, they can take $ApeUSD from the lending protocol and add to the pool
2. **unstakeAndWithdraw**- when the price of $apeUSD <$1, they can remove $ApeUSD from the pool and repay the debt position

The [StabilizerV2](https://etherscan.io/address/0x8E252A679C87313Ccefc9559F4f1c0e4062390B5#code) smart contract is adapted to the Convex boosting pool strategy.

![etherscan-stabilizer-v2](https://user-images.githubusercontent.com/89845409/204543422-0b1071d2-b101-4d2e-bcf2-77f315accda4.png)

(source: [Etherscan StabilizerV2](https://etherscan.io/address/0x8E252A679C87313Ccefc9559F4f1c0e4062390B5#code) - strategy)

The [StabilizerV3](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#readContract) smart contract is optimized for yield strategies over 3 pools - `apeUSDConvexStakingWrapperFrax`, `apeUSDFraxStaking`, and `apeUSDCurvePool`. This enables farming triple rewards - CRV, CVX, and FXS.

![etherscan-stabilizer-v3](https://user-images.githubusercontent.com/89845409/204543545-122c34ae-ba9e-4410-88f2-c817d066ca8e.png)

(source: [Etherscan StabilizerV3](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#code))

Essentially, the $ApeUSD stability is fully controlled by Ape Finance multi-sig. Currently, the protocol admin has control over almost all $ApeUSD supply ([~99.8%](https://etherscan.io/token/0xff709449528b6fb6b88f557f7d93dece33bca78d#balances)). And with frequent rebalances of $ApeUSD from [ApeUSD/crvFRAX] metapool, the protocol is successfully keeping the peg and the [balance](https://dune.com/queries/1017494/1756776) with the counterpart assets.

Given that $ApeUSD is not really utilized in the open market, there is little risk of external factors impacting the pool balance. Since ApeFi also controls the utilization rate of $ApeUSD within the lending protocol, there is little danger of an unexpected supply increase. The image below displays the correlation of $ApeUSD and the corresponding counterpart assets crvFRAX within the Curve pool.

![dune-apeusdfraxbp-tvl](https://user-images.githubusercontent.com/89845409/204543641-ea37f70c-b138-4bfc-901c-10bfbb84035c.png)

(Source: [Dune](https://dune.com/queries/1017494/1756776))

Depeg risk is more likely to arise as a result of centralization in $ApeUSD's core functionality. Without active and responsible rebalancing executed by the protocol multisig, $ApeUSD would almost certainly depeg.

## Collateral Risk

Regardless of the fact that Ape Finance is largely a Compound fork, the lending protocol is designed to maximally reduce collateral risk. The protocol has only one collateral market ($APE) with the following settings:



* [Loan-to-Value](https://docs.ape.fi/borrow-and-repay/dashboard#maximum-ltv): 60%
* [Liquidation Premium](https://docs.ape.fi/borrow-and-repay/dashboard#liquidation-fee): 10%
* [Close Factor](https://etherscan.io/address/0xDE607fe5Cb415d83Fe4A976afD97e5DaEeaedB07#readProxyContract): 50%
* [Borrowing Interest Rate](https://docs.ape.fi/borrow-and-repay/dashboard/interest-rate-calculations): variable but capped at 10%
* [Borrow fee](https://docs.ape.fi/borrow-and-repay/dashboard#borrow-fee): 0.50%
* [Borrow cap](https://docs.ape.fi/borrow-and-repay/dashboard#total-apeusd-available): 5,000,000 $ApeUSD
* [Oracle price feed](https://docs.ape.fi/borrow-and-repay/borrow/price-oracle): Chainlink with on-chain price updates

All these factors are set to a conservative default setting. In terms of risk parameters, ApeFi is relatively risk-averse.

Apecoin ($APE) as the collateral asset has a large market cap ([$1,058,753,958](https://www.coingecko.com/en/coins/apecoin#markets)) and the token has high liquidity, but mostly on centralized exchanges (Binance, Coinbase, KuCoin, Kraken, and many others), while on decentralized exchanges most liquidity is on Uniswapv3.

![uniswap-apecoin-liquidity](https://user-images.githubusercontent.com/89845409/204543740-fad06e94-9c77-4a52-9a93-abe230fc45ee.png)

(source: [UniswapV3 Analytics](https://info.uniswap.org/#/tokens/0x4d224452801aced8b2f0aebe155379bb5d594381))

However, as emphasized several times, most of the $ApeUSD in circulation is not backed by $APE. This always has to be kept in mind when interacting with the protocol.


# Reflections

Ape.fi leaves the impression of a rather risky protocol that is still mostly under development. This comes from a few observations:

* The project's documentation is clear and informative but isn’t very detailed. In general, their communication and promises are sometimes inconsistent with their execution. For instance: no $APEFI token distribution as promised, rather intransparent funding, governance NFTs are still not active, and general intransparency regarding collateralization of $ApeUSD.
* There seems to be no community or any attempts to build DAO-like structures. Ape.fi has no Discord (only a Telegram chat). There is no forum, no governance process, and the [Snapshot](https://vote.ape.fi/#/) page is only used to vote on ApeCoin DAO meta-governance decisions (with very little participation).
* Influence over the protocol is limited to the initial group of developers. The team controls the multi-sig and has full control over the entire protocol.
* The vast majority of $ApeUSD is minted via the stabilizer contract. However, in contrast to other protocols applying the AMO concept, the majority of the circulating stablecoins are created this way and thus have no $APE as backing. The collateralization ratio of (~60-70%) is recovering, but still rather low. Plus there are no emergency or other stability mechanisms in place.
* As of today, the protocol has not found a clear product-market fit. There’s little native TVL of $APE tokens. There is no use case for $ApeUSD outside the Curve factory pool. Most of the circulating stablecoins belong to the protocol and are used to farm CRV, CVX, and FXS. The protocol’s treasury is rewarded chiefly through their farming activities, but there is no community to profit from those rewards.
* ApeFinance is a fork of forks that has not been audited and they only refer to a CREAM audit. The team stated that they are currently conducting an audit, although this has not been verified.


# ApeFi Gauge Criteria

1. Is it possible for a single entity to rug its users?

Yes, Ape Finance could rug its users. [ApeApe.sol](https://etherscan.io/address/0xcaB90816f91CC25b04251857ED6002891Eb0D6Fa) and [apeApeUSD.sol](https://etherscan.io/address/0xc7319dBc86A121313Bc48B7C54d0672756465031#code) are both controlled by the admin multi-sig (2-out-of-4) with a 2-day timelock.

None of the team members are doxxed and out of four signers, only one can be “verified” as a real user (i.e. a community member with an ENS). The other signing EOAs are probably created only for signing purposes (based on EOA activity).



2. If the team vanishes, can the project continue?

Probably not, because Ape Finance’s multi-sig controls most of the protocol’s key mechanisms (e.g. unitroller markets, maintaining the peg with add/remove actions, $ApeUSD available for borrowing, lending parameters, etc.).



3. Do audits reveal any concerning signs?

No audit was performed on Ape Finance smart contracts directly, although contracts are forks of Compound, Cream, and Iron Bank.

# Risk Team Recommendation

Given the reasonable possibility that a single anonymous actor has the power to rug [ApeUSD/FraxBP] pool LPs, we urge the Ape.fi team to reconfigure the protocol multisig. The unverified owners should be replaced by representatives from external, trusted stakeholders, such as team members from Convex, Curve, Frax, and/or the Crypto Risk team.

**Barring a good faith effort, we believe $ApeUSD does not meet the necessary criteria to have a Curve gauge.**
