# Overview - Bitcoin on Ethereum

### Only ~1% of the BTC supply is bridged to Ethereum and almost 98% of them is wBTC. The utility on ethereum for BTC defines the amount of BTC bridged. The dashboard helps to gauge this utility.

## Index

- BTC and Bridged-BTC Supply
- Centralized vs Decentralized variants of bridged BTC
- wBTC
- Ren sun setting
- MultiBTC
- Liquidity Provisioning of bridged BTC on Curve
- Using the Dashboard - [BTC on Ethereum Detailed Dash by LlamaRisk Team](https://dune.com/diligentdeer/btc-on-ethereum-updated)

## BTC and Bridged-BTC Supply

The current supply of BTC as of 25th January 2023 is 19,272,338 and the current BTC on Ethereum (wBTC, hBTC, bBTC, sBTC, renBTC, multiBTC, pBTC, tBTC, TBTC, imBTC, oBTC) supply is almost 194,607 which is 1% of the total BTC supply on the Bitcoin chain.

The only reason to bridge BTC from Bitcoin network to Ethereum network is utility, may it be depositing it in the liquidity pools to get yields or depositing it as a collateral to borrow tokens while enjoying the BTC price appreciation or depositing it to bridge to other chains. The proportion of bridged BTC to ethereum to the BTC supply peaked in the mid of May 2022 at 1.8% (currently ~1%).

![](https://lh6.googleusercontent.com/5iPktxLNoHCeut9SUXzYy4xxowRYJSCwHM5gsYRZxacJwjOAPPvAP-V7QrZeN3uDfP1-g4-MuLBJ0pklUcVHUfxgxlfmnDm84nS4uFT2ZyKhHOs_aRZHy8EgZ04hUj8FW22MLu8U6Yi9oml3iw8aF2aEVkd-faCYvYKHWD7BWiluTKhQ9En-xASDGP5uaw)

Source: [Dune Query](https://dune.com/queries/1840330/3027253) by @hildobby

## Centralized vs Decentralized variants of bridged BTC

With every negatively impacting event in DeFi after the Terra collapse the supply of bridged BTC reduced and users started moving towards centralized entities that provided bridge service.

Considering WBTC, HBTC, BBTC as centralized and sBTC, renBTC, tBTC, TBTC, pBTC, oBTC, multiBTC as decentralized the proportion changed from almost 92% centralized to 98.3% centralized between mid May 2022 and January 2023. In the same period the overall bridged BTC supply dropped 44%.

![](https://lh3.googleusercontent.com/ahsZmUCT4HoUVK38kj7nTKGhgR0W5gGszXfnDq-M6UIFUiimFna8ghkm8E6JH4Pxz5_acKf1Z5cgaN6TIZ1lRlKq2ISmC6Q2DLufpqX-ChkcjHpRGZOtvq_SI38VKz8cG0yvanogsuYLtpOu_G02bJOi7pS97WJIfPEU45dg7vBkLAR1ozbFJ9WLcRgk3Q)

Source: [Tokenized BTC Issuance (Centralized vs Decentralized)](https://dune.com/queries/1824464/3033066)

## wBTC

wBTC supply peaked before the Terra collapse and at that time it held a share of almost 83% of all the bridged BTC on ethereum which is 92.2% as of 25 January 2023. 

![](https://lh4.googleusercontent.com/ApfzF6UAAXeWpzTDu12WuA0LmeQNkNkLJRngEOIT2zfuy9MnLJfDhb1hvc0FEBUSMCZiGeQXRXPDjD6Qz_HvXqP2zdt9JMxKepZXBL1HhImK0ZGeTpP78XhOqwTatV9Wl2B6rlWcB-k_yStlRnZHmC9s8Ry-rK299QnXeULzDEC1VB9o-sK5d7peOiA98g)

Source: [wBTC Supply and Holders Data [v2-5]](https://dune.com/queries/1910355/3146248)

With decreasing wBTC supply and increasing wBTC holders, it can be said that the holding is shifting towards the holders holding less quantities of wBTC, subsequently it can also be said that the whales have showed lesser engagement after May 2022.

## Ren sun setting

Ren team made an announcement on 19th November 2022 to sunset Ren 1.0 network to fully break any possible ties with Alameda. Before the announcement, renBTC supply was almost 2400 (18th November 2022).

![](https://lh4.googleusercontent.com/1YmihM_5lY_wMwuBREn0MUbt8OnqC53N36n-SXdn9UGQOO7vkkhCcVjzYz85Xploi-icCNgmC7iaq7_pdCE2vX-q8kg-MDkJ8_jgNa3RmGHZlwgxrmipLezcE8sNu_HDNfNRMybXJoY9GSIOsbrS1Wdh4Swvyxbu9lUlK5HNVd2TwS3QDGIzbSehCAjJoQ)

Source: Medium article -[Moving on from Alameda](https://medium.com/renproject/moving-on-from-alameda-da62a823ce93)

Since 18th November 2729.6 renBTC have been burned (bridged back) and still have 421 renBTC yet to be burned. Theoretically this creates a demand for the BTC which was burned but this might have changed based on the market outlook. 

Multichain's multiBTC is trying to capture the same demand that was created due to the depreciation of the Ren network.

Also one of the OG LlamaRisk team member, @wormholeoracle have [proposed to kill gauges for all metapools containing renBTC.](https://gov.curve.fi/t/proposal-to-kill-gauges-for-all-metapools-containing-renbtc/8679)

TL;DR of the proposal:

- All metapool gauges containing renBTC, as Ren Protocol will be shutting down soon. This includes pBTC1, pBTC2, ibBTC, oBTC, tBTC2, and BBTC.

- pBTC has a new metapool with sBTC2.

- Threshold Network (TBTC) has a new metapool with sBTC2.

- BoringDAO failed to respond to get a new metapool. 

- Binance team is not motivated to support the BBTC product.

- BadgerDAO is in the process of deprecating the ibBTC product.

- Most importantly, **LlamaRisk members would like to assist teams with an existing BTC gauge on pool migration.** 

## MultiBTC

MultiBTC was created on 16th December 2022 and the multiBTC-sBTC2 pool was successfully voted to add a gauge controller ([Vote](https://dao.curve.fi/vote/ownership/243)).



> In light of the upcoming sunset of renBTC, we want to deploy a metapool that consists of 50% sbtc2 and 50% multiBTC. multiBTC is a derivative of BTC, that is fully backed with BTC and can be redeemed on the Bitcoin Mainchain in a decentralized and trustless way, even if the wrapping function is deprecated.
> 
> Source: [Proposal to add multibtc3CRV (multiBTC-sbtc2) to the Gauge Controller](https://gov.curve.fi/t/proposal-to-add-multibtc3crv-multibtc-sbtc2-to-the-gauge-controller/4711)



High CRV rewards attracted huge liquidity in the [multiBTC-sBTC2 pool](https://curve.fi/#/ethereum/pools/factory-v2-245/swap). With added utility, renBTC holders have shifted to using multiBTC, almost 82% of multiBTC supply is held by the past renBTC holders.



![](https://lh6.googleusercontent.com/-bEAeP7vQKw5ggaERcO6xWIEqpkey4j8fDegxHmonteMsDSePFEQZ5TTKZRm_kaiUdNz9QqK8zDmB9p4aJNYZIMDk4QiMJMzOnnO8uAJq2U8uz_xYYbMJjnzVyi6WL-1YjjPDEYcduyifM9wGrOv9vWK9QmObbX0YNVvxZR7kO_pLGoeUOl-E2EQ8vvxWw)

Source: [RenBTC holders&#x27; behavior [wBTC multiBTC sBTC renBTC]](https://dune.com/queries/1926960/3178641)

![](https://lh3.googleusercontent.com/sOPoUhv9mS6R2fZsfuRgpi4Ib6YDNxVFfdSY05HDrE-UvAIdLY92cGVkiGB4oARShc0YxLeWFVAuFF6SuVB9UdJkxllONNTBpCRz5QR1C3HcKsisClGHM8U6WkHY8krfw6ki2qjyW0SyMXTa0hzwz2ryNnvXW4q7KmEDr7RxIYc4NRrr5dweMSSAhktGSQ)

Source: [RenBTC holders&#x27; behavior [wBTC multiBTC sBTC renBTC]](https://dune.com/queries/1926960/3178899)

## Liquidity Provisioning of bridged BTC on Curve

One of the biggest liquidity pool on Curve Finance holding bridged BTC is tricrypto2 (wBTC-wETH-USDC). Providing liquidity in such a pool can be considered as a index token with stable exposure to the prices of the participating token. Moreover liquidity providers gets emissions and swap fees as yield.

It is recommended to check the past performance and the peg of the bridged BTC in the liquidity pool before providing liquidity. By providing liquidity to any liquidity pool, the LP is taking a risk exposure linked to the participating assets in the pool.

## Using the Dashboard

Dashboard: [BTC on Ethereum Detailed Dash by LlamaRisk Team](https://dune.com/diligentdeer/btc-on-ethereum-updated)

**Token liquidity pool specific sections show:**

- Liquidity pool asset TVL overtime.
- Liquidity pool asset proportion over time.
- Pool activity for participating tokens.
- Top depositors and withdrawals with amounts in the last 1 month.

**Token-specific sections show:**

- The distribution and top holding based on wallet type (contract/wallet).
- Minting and Burning activity for the last 1 year.
- Top minters and burners for the last 1 month.
- Overall token holding in contracts vs wallet over time.

There is also a small section showing the behavior of renBTC holders which can be used to check how much of the demand got filled in with the bridged BTC after the ren network depreciation.



 
