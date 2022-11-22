<!-- Output copied to clipboard! -->

<!-----

You have some errors, warnings, or alerts. If you are using reckless mode, turn it off to see inline alerts.
* ERRORs: 0
* WARNINGs: 0
* ALERTS: 13

Conversion time: 4.171 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β33
* Fri Nov 18 2022 06:14:06 GMT-0800 (PST)
* Source doc: Risk Assessment: ApeUSD V1.0
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!


WARNING:
You have 10 H1 headings. You may want to use the "H1 -> H2" option to demote all headings by one level.

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 1; ALERTS: 13.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>
<a href="#gdcalert7">alert7</a>
<a href="#gdcalert8">alert8</a>
<a href="#gdcalert9">alert9</a>
<a href="#gdcalert10">alert10</a>
<a href="#gdcalert11">alert11</a>
<a href="#gdcalert12">alert12</a>
<a href="#gdcalert13">alert13</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


**Asset Risk Assessment: ApeUSD**

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

Ape Finance is a stablecoin lending protocol. It enables APE token holders to deposit APE as collateral and borrow ApeUSD against it. APE was airdropped to BAYC and MAYC holders by the ApeCoin DAO. Ape Finance is an independent protocol. It aims to unlock capital efficiency, new yield farming options, and other use cases for APE holders.


### A quick TL;DR of our findings:



* Ape.fi was created by a group of anonymous developers to enable additional utility for the ApeCoin (APE). ApeCoin is the native ERC-20 token of the ApeCoin DAO. The token was airdropped to BAYB and MAYC NFT holders.
* In May 2022, Ape.fi introduced $ApeUSD** **- a stablecoin soft-pegged to the US-Dollar, that can only be borrowed against ApeCoin. ApeUSD is also available via a Curve metapool.
* Effectively over 90% of ApeUSD’s supply is not collateralized. Ape Finance limits the total supply of $ApeUSD (currently 15M) and uses the majority share that is not borrowed to provide liquidity to their Curve metapool [apeUSDFRAXBP](https://etherscan.io/address/0x04b727c7e246ca70d496ecf52e6b6280f3c8077d#readContract).
* The key “trick” behind $ApeUSD’s stability is to maintain the balance inside the metapool, by adding and removing $ApeUSD when an imbalance occurs. The gated access to $ApeUSD via the lending protocol and its limited supply prevent extreme imbalances. For an outsized in-flow of $ApeUSD into the pool, it's necessary to borrow $ApeUSD from Ape Finance (which they can control via parameter setting and utilization rate). An outsized inflow of counterparty assets can also be countered by minting and adding more ApeUSD.
* The protocol also issued its own native token $APEFI, which is used to reward LPing ApeUSD and APEFI. Other than that, APEFI only functions as a currency to buy Ape.fi NFTs. Those NFTs should serve as governance tokens. However, as of today, there are no governance processes in place. The anon team has full control over the protocol. Furthermore, 97% of APEFI’s total supply sits in a [multi-sig](https://debank.com/profile/0x02ca76e87779412a77ee77c3600d72f68b9ea68c) controlled by the same group.
* Ape Finance applies a „Protocol-Owned-Farming” strategy. They own over 50% of the metapool and stake those LP tokens to earn liquidity incentivizes. They are rewarded chiefly by accumulating CRV, CVX, and FXS. Some of the rewards are vote-locked in order to further incentivize the metapool.
* Ape Finance has not been audited. The smart contracts are mostly forks of Compound, Cream, and Fixed Forex (by Iron Bank). There seems to be no community activity, governance process, or any kind of emergency mechanism that would allow anyone outside the team to interact with the protocol on any level.


# Ape Finance - Introduction


## A short retrospect

Before we start, let’s quickly recap the timeline of the monkey images. In April 2021, [Yuga Labs](https://www.yuga.com/) released a series of 10-thousand NFTs called the Bored Ape Yacht Club ([BAYC](https://boredapeyachtclub.com/#/)). The NFT collection became extremely popular very quickly. Many celebrities started to buy them. Soon bored apes would sell for as high as [$3M](https://blockgeeks.com/5-most-expensive-bored-ape-yacht-club-nfts/#:~:text=The%20most%20expensive%20BAYC%20NFT,its%20combination%20of%20rare%20traits.) for one piece. Building upon this success, Yuga Labs released their second NFT collection, the Mutant Ape Yacht Club ([MAYC](https://boredapeyachtclub.com/#/mayc)). And in March 2022, the [ApeCoin DAO](https://www.coingecko.com/de/munze/apecoin) issued its native token ApeCoin (APE).

15% or 150M of the newly created APE tokens were airdropped to BAYC and MAYC holders. It should serve as the governance and utility token of the ApeDAO ecosystem.


## Ape Finance and ApeUSD

So far so good. But what would a DAO and its community of APE holders be without a stablecoin? Enter [Ape Finance](https://ape.fi/borrow). Ape.fi for short is a stablecoin lending protocol created by a group of anonymous developers, who were active in ApeCoin DAO and applied for a $250k [grant](https://forum.apecoin.com/t/aip-83-proposal-for-apefi-to-receive-apecoin-token-grant-ecosystem-fund-allocation/7447). The goal of Ape.fi is to unlock capital efficiency for APE holders, by offering them the option to borrow a stablecoin (ApeUSD) against their APE tokens, and earn yield on the borrowed funds.

On a technical level, Ape.fi is mostly a fork of Compound with some elements of Fixed Forex (code written by Andre Cronje). Ape Finance currently only supports one lending pool (APE) and one borrowing counterpart (ApeUSD). Unlike Compound’s cTokens (the IOU token that users receive when lending tokens), Ape.fi issues apeAPE tokens that don’t accrue any interest but only serve as a receipt for the deposited collateral.


## APEFI and ApeFi NFTs

Ape Finance also issued its own ERC-20 token called [APEFI](https://www.coingecko.com/en/coins/ape-finance). The token is supposed to further incentivize contributions to the protocol and function as a reward token for liquidity providers. Furthermore, APEFI serves as the only currency for buying ApeFi NFTs according to their [docs](https://docs.ape.fi/apefi-nfts). These [NFTs](https://twitter.com/apefinfts) are then used for governance (1 NFT = 1 vote). So the token's only utility is to buy governance power. In other words, users have to decide whether they want to keep the financial reward or participate in governance. There are, however, some inconsistencies with regard to those promises.

At first, the plan was to airdrop 5% of the token to all participants of the [AIP-83](https://forum.apecoin.com/t/aip-83-proposal-for-apefi-to-receive-apecoin-token-grant-ecosystem-fund-allocation/7447) Snapshot vote, which would get Ape.fi the grant from ApeCoin DAO. This was also described in a [blog post](https://medium.com/@ApedotFi/and-then-there-was-defi-39bcd7ef1565), where the distribution of APEFI was further detailed (5% initial bootstrap, 5% airdrop, 25% contributors, 65% community/treasury).



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


(source: [Twitter](https://twitter.com/apedotfi/status/1525029388688842752?s=20&t=_MD08E0P7LQNnZurdMCN9A))

In reality, however, the Snapshot vote for the grant never took place, and neither did the token distribution. At the time of writing this article, 97.25% of all APEFI tokens still sit in the initial [contract](https://etherscan.io/token/0x4332f8A38f14BD3D8D1553aF27D7c7Ac6C27278D#balances), which is controlled by the team. And ~2% are in a Uniswap V2 pool. The developers did not adhere to the initial token distribution as detailed in their post.

Even without the grant vote, the team received initial capital from [machibigbrother.eth](https://app.zerion.io/0x020ca66c30bec2c4fe3861a94e4db4a498a35872/overview) and from an FTX [address](https://etherscan.io/address/0x2faf487a4414fe77e2327f0bf4ae2a264a776ad2) in mid-May. In total their [developer fund](https://app.zerion.io/0x02ca76e87779412a77ee77c3600d72f68b9ea68c/history) received $150k in USDC and 56’250 APE tokens (worth ~$452k at that time). These funds were used to bootstrap the initial liquidity on Curve and Uniswap.

[First by buying and deploying $100k FRAX with the USDC, and minting and deploying $200k ApeUSD with the APE tokens ([trx](https://etherscan.io/tx/0xdf7abd4000eeadbe30cf55650ef41d15310f6b903be07589d8f24046c8e2636b)). And secondly, be deploying $50k ApeUSD (swapped using the remaining 50k USDC) and 8.25M APEFI into a Uniswap V2 pool ([trx 1](https://etherscan.io/tx/0xa809822fc74cff198f173ec414dbafeeca2278f38d0f2c1d463f33ecb1051ea5) & [trx 2](https://etherscan.io/tx/0x4bfbaed890e2f1967d66aee0f2e1e1c039357105826d7e54e0e8adfbac26f0ae)).]

Besides the unrealized token distribution, another inconsistency becomes apparent when looking at Ape.fi’s [Snapshot page](https://vote.ape.fi/#/). Voting is actually done with APEFI tokens, not with APEFI NFTs as stated in their [docs](https://docs.ape.fi/earn/apefi-token). What’s more, is that this Snapshot page only ever had a few meta-governance votes from ApeCoin DAO and zero votes that actually affect Ape.fi.


# ApeUSD as an asset

ApeUSD is a stablecoin for metaverse users soft-pegged to the US-Dollar. It can either be borrowed by collateralizing ApeCoin (APE) or bought on Curve with USDC or FRAX.

The first mechanism for maintaining ApeUSD’s peg is over-collateralization through the lending protocol. The Loan-to-Value (LTV) is currently set to 60%. The second stability mechanism - also enabling arbitrage - is the [ApeUSD/FRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-138/swap) Curve [metapool](https://resources.curve.fi/lp/base-and-metapools). Further, Ape Finance has control over the supply of ApeUSD. They do this by balancing between the total amount of $ApeUSD available for borrowing (image below) and the amount of $ApeUSD in [apeUSDFRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-138/deposit) metapool.



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")


(source: [Ape.fi](https://ape.fi/borrow))

The max supply of ApeUSD is currently set to [15M](https://etherscan.io/token/0xfF709449528B6fB6b88f557F7d93dEce33bca78D#balances). This can change though. At the beginning of November, the supply was at 20M. The team can decide to increase/decrease the supply with a 1-day [timelock](https://etherscan.io/token/0xff709449528b6fb6b88f557f7d93dece33bca78d#readContract).

Only [93](https://etherscan.io/token/0xfF709449528B6fB6b88f557F7d93dEce33bca78D#balances) addresses currently hold ApeUSD, whereby the Curve pool and the smart contract that contains the borrowable tokens ([ApeErc20Delegator](https://etherscan.io/address/0xc7319dbc86a121313bc48b7c54d0672756465031/advanced#code)) account for 99.87% of the total supply. The rest is mostly dust.


## Protocol-owned Liquidity

Ape Finance applies a [strategy](https://docs.ape.fi/protocol-owned-liquidity) to actively use the non-borrowed ApeUSD. They claim that the ApeUSD deployed into Curve does not need to be backed by APE, because of the counterpart assets inside the pool (USDC and FRAX), those should function as indirect backing.

This is somewhat true. The protocol has full control over the ApeUSD supply and its balance  against crvFRAX inside the liquidity pool. To achieve this, they use the [StabilizerV3](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#code) contract, which is [controlled](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#readContract) by the team’s [multi-sig](https://etherscan.io/address/0x02cA76E87779412a77Ee77C3600D72F68b9ea68C). The team regularly rebalances the amount of ApeUSD in the pool. Since they also control the amount of ApeUSD that can be borrowed, nobody can suddenly mint an “uncontrollable” amount of ApeUSD to dump. The image below shows that the most called function of the stabilizer is for depositing ApeUSD into the pool.



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")


(Source: [Tenderly](https://dashboard.tenderly.co/Lavi/project/analytics/4b39bfe3-da64-44d9-b2b8-bef59a70bf17?contract=1%3A0xd5bb32d5745af76510ba705d6063dd1f326aaf24))

Essentially, Ape Finance fully owns at least 50% of the factory metapool. As a result, the majority of ApeUSD in circulation is not collateralized. Only a minority of ApeUSD is actually borrowed by users and thus over-collateralized. Let’s take a closer look.

At the time of writing this report, [228,127](https://etherscan.io/address/0xcaB90816f91CC25b04251857ED6002891Eb0D6Fa) APE tokens are deposited into the protocol. That’s worth roughly $672k in USD. Given the current LTV of 60%, a maximum of ~$403k ApeUSD could be borrowed. However, the Curve pool holds over $9M in ApeUSD. Those tokens were deposited into Curve by Ape Finance, using the tactic described above.

[Side note: The largest depositor of APE is this [address](https://debank.com/profile/0xf035af8a3dff1f90aeb21f9623589ae1330e9e09). It’s not clear whether the address is somehow related to Ape Finance. However, it regularly interacts with the protocol.]

In conclusion, Ape Finance’s total TVL of ~$9M - as shown on [DeFi Llama](https://defillama.com/protocol/ape-finance?showMcapChart=false) - is misleading. It includes the ApeUSD that sits in the Curve metapool. Yes, the $9M ApeUSD mostly belongs to Ape Finance. But they are backed by nothing. Ape Finance is minting its stablecoin out of thin air. More than 93% of ApeUSD (and thus Ape.fi’s TVL) is not collateralized.

We are not aware of any other stablecoin protocol applying a similar strategy and it’s out of the scope of this article to explore the possible consequences that this might have.


## Protocol-owned Farming

To sum it up, most ApeUSD in the Curve pool was deposited by Ape.Fi itself and is not collateralized. The protocol uses the minted - but unused ApeUSD - that sits in their smart contracts and deploys them into the Curve pool. This strategy allows them to earn CRV, CVX, and FXS incentives (they call it [protocol-owned farming](https://docs.ape.fi/protocol-owned-liquidity/protocol-owned-farming)). And they use it. Almost all liquidity inside the Curve pool is currently staked on [Convex](https://www.convexfinance.com/stake). And roughly half of it is also staked on [Frax](https://frax.convexfinance.com/). The farmed tokens are regularly claimed by their developer fund, swapped for CVX, and re-locked to Convex for vlCVX (see [history](https://debank.com/profile/0x02ca76e87779412a77ee77c3600d72f68b9ea68c/history)).

With the current yield (image below), they can earn roughly $780k in CRV and CVX per year, if we take the current $9.4M as base TVL. If the staked LPs on Frax were also owned by Ape.Fi, we can add another ~$330k on top. However, we don’t know that for sure. Still, this calculation is conservative, as their yield-generating [TVL](https://defillama.com/protocol/ape-finance¨) used to be much higher, plus most tokens earned are currently comparatively low in price.



<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")


(source: [Convex](https://www.convexfinance.com/stake))

Unfortunately for Curve, the token isn’t causing much trading volume. Most days there is no activity at all. The image below displays the pool's trading volume. There is only sporadic trading, mostly caused by rebalancing activities.



<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.png "image_tooltip")


(Source: [Dex.guru](https://dex.guru/token/0xff709449528b6fb6b88f557f7d93dece33bca78d-eth/history))

In summary, Curve, Convex, and Frax are incentivizing a pool that only benefits one entity, and hardly generates any trading fees or utility for their communities.


## Minting ApeUSD

ApeUSD is minted through the function _mintBorrow. There is very little interaction with the protocol required. To mint ApeUSD, users only have to select the amount of APE they deposit as collateral, and define the borrowed amount.

After allowing the deposit, the actual depositing and borrowing can be done in one click. Below is a screenshot of the Ape Finance lending interface.



<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")


(source:[ Ape.fi website](https://ape.fi/borrow))

Ape Finance simplifies the lending and borrowing process by batching all steps into one transaction (deposit APE – mint apeAPE – borrow ApeUSD), shown below. The first image shows the flow of a transaction on Etherscan, and the second is a screenshot of the _mintBorrow function from the ApeTokenHelper smart contract:



<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")



# (source:[ Etherscan](https://etherscan.io/tx/0xf315b7b80324cd8059d8ec46947abde536d3fb3834df0a93dc0d212424e461fc))


# 

<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image8.png "image_tooltip")



# (source:[ ApeTokenHelper.sol](https://www.contractreader.io/contract/0xCAAA0C316f8b91F25b35E318f83CaaB4ACD327fB))



<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image9.png "image_tooltip")


(source: [ApeUSD.sol](https://www.contractreader.io/contract/0xfF709449528B6fB6b88f557F7d93dEce33bca78D))

The image above displays the G (modifier), which allows the gov-account to mint $ApeUSD. This can only be set by Ape Finance multi-sig with a 1-day delay. The contract is a fork of Iron Bank’s fixed forex contract (for more info check the [Fixed Forex Risk Assessment](https://cryptorisks.substack.com/p/asset-risk-assessment-fixed-forex)).


# Risk Vectors

Some of the risks are highlighted below.


## Smart Contract Risk

Ape Finance has not been audited by any third party. On their Github [profile](https://github.com/ape-fi/ape-finance/tree/ape/audits), Ape.fi published an [audit](https://github.com/ape-fi/ape-finance/blob/ape/audits/trailofbits-CREAMSummary.pdf) that was performed on Cream Finance by Trail of Bits (Cream also uses the same smart contracts). This is a peculiar move. CREAM is notorious for being probably the [most hacked](https://rekt.news/de/leaderboard/) DeFi protocol ever. It’s not clear how referencing an audit that was performed on CREAM should increase confidence in Ape.fi’s security.

The team also refers to a [formal verification](https://github.com/ape-fi/ape-finance/tree/ape/spec/certora/contracts) process provided by Certora Proven. However, the repositories they linked mostly refer to Compound’s cTokens and some links are many months older than Ape Finance itself. Thus, it’s doubtful that the verification process was actually performed.


## Custody Risk

All changes over Ape.fi and its protocol-owned liquidity are controlled by a 2-of-4[ multi-sig wallet](https://etherscan.io/address/0x02cA76E87779412a77Ee77C3600D72F68b9ea68C#readProxyContract) ([signer 1](https://etherscan.io/address/0xA0AEda175B9266B8601D33E9Ef31673cC8cc4982), [signer 2](https://etherscan.io/address/0xA17c86900a9Ece9194F65F0C48046fA62b03396b), [signer 3](https://etherscan.io/address/0xdcc406dA85E263641C5692A9749E08998409fbF5), [signer 4](https://etherscan.io/address/0x056a8152C8ea78Ef73EED50CE3b643b35d953379)). Ape Finance could implement changes that can pose a danger to user funds. Some examples of what can be changed:



1. It can alter collateral factors, fees, or the interest rate
2. It can change the oracle implementation affecting the price
3. It can alter the smart contracts and add maliciously-crafted elements
4. It can adjust the available amount of ApeUSD, impacting the utilization rate

The Ape Finance team and the multi-sig owners are not doxxed. One signer ([apeficaesar](https://twitter.com/ApefiCaesar)) is an active ApeDAO community member, while the other three signers are just EOAs with no activity. In the discussion on the ApeCoin [governance forum](https://forum.apecoin.com/t/aip-83-proposal-for-apefi-to-receive-apecoin-token-grant-ecosystem-fund-allocation/7447), one can also see concerns raised by other community members regarding the anonymity of the team.

All smart contracts used by Ape Finance, - also those that contain the user funds (APE) - are essentially forks of Compound, Fixed Forex, or Cream. Since most of those platforms experience multiple hacks and exploits, caution is highly recommended! Again, the team is entirely anon and not a single audit was performed nor is there a bug bounty program. Moreover, there is no fallback or emergency mechanism in case something goes wrong.


## Depeg Risk

ApeUSD’s price [history](https://www.coingecko.com/en/coins/apeusd) since inception is relatively stable. As eluded earlier, since ApeUSD is not collateralized, Ape Finance needs to maintain the balance between tokens inside the pool with its [Stabilizer smart contracts](https://github.com/ape-fi/stabilizer). They simply deposit the unused borrowing power from the lending protocol, using the stabilizer contract, which is based on two admin rebalance functions:



1. **depositAndStake **- when the price of apeUSD increases over 1$, they can take apeUSD from the lending protocol and add to the pool
2. **unstakeAndWithdraw **- when the price of apeUSD decreases below 1$, they can remove ApeUSD from the pool and repay the debt position

The [StabilizerV2](https://etherscan.io/address/0x8E252A679C87313Ccefc9559F4f1c0e4062390B5#code) smart contract is adapted to the Convex boosting pool strategy.



<p id="gdcalert10" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image10.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert11">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image10.png "image_tooltip")


(source: [Etherscan StabilizerV2](https://etherscan.io/address/0x8E252A679C87313Ccefc9559F4f1c0e4062390B5#code) - strategy)

The [StabilizerV3](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#readContract) smart contract is optimized for yield strategies over 3 pools - `apeUSDConvexStakingWrapperFrax, apeUSDFraxStaking, apeUSDCurvePool `this enables farming triple rewards - CRV, CVX, and FXS.



<p id="gdcalert11" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image11.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert12">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image11.png "image_tooltip")


(source: [Etherscan StabilizerV3](https://etherscan.io/address/0xd5bb32d5745af76510ba705d6063dd1f326aaf24#code))

Essentially, the $ApeUSD stability is fully controlled by Ape Finance multi-sig. At this point, the protocol admin has control over almost all $ApeUSD supply ([~99.8%](https://etherscan.io/token/0xff709449528b6fb6b88f557f7d93dece33bca78d#balances)). And with frequent interventions of adding/removing $ApeUSD from ApeUSD/crvFRAX the metapool, the protocol is successfully keeping the peg and the [balance](https://dune.com/queries/1017494/1756776) with the counterpart assets.

Given that $ApeUSD is not really utilized in the open market, there is little risk of external factors impacting the pool rebalance. Since ApeFi also controls access to its ApeUSD via the utilization rate within the lending protocol, there is little danger of an unexpected supply increase.



<p id="gdcalert12" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image12.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert13">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image12.png "image_tooltip")


(Source: [Dune](https://dune.com/queries/1017494/1756776))


## Collateral Risk

Regardless of the fact that Ape Finance is largely a Compound fork, the lending protocol is designed to maximally reduce collateral risk. The protocol has only one collateral market (APE) with the following settings:



* [Loan-to-Value](https://docs.ape.fi/borrow-and-repay/dashboard#maximum-ltv): 60%
* [Liquidation Premium](https://docs.ape.fi/borrow-and-repay/dashboard#liquidation-fee): 10%
* [Close Factor](https://etherscan.io/address/0xDE607fe5Cb415d83Fe4A976afD97e5DaEeaedB07#readProxyContract): 50%
* [Borrowing Interest Rate](https://docs.ape.fi/borrow-and-repay/dashboard/interest-rate-calculations): variable, 4.10% at the moment, capped at 10%
* [Borrow fee](https://docs.ape.fi/borrow-and-repay/dashboard#borrow-fee): 0.50%
* [Borrow cap](https://docs.ape.fi/borrow-and-repay/dashboard#total-apeusd-available): 5,000,000 $ApeUSD
* [Oracle price feed](https://docs.ape.fi/borrow-and-repay/borrow/price-oracle): Chainlink with on-chain price updates

All these factors are generally set to a conservative default setting. In terms of risk parameters, ApeFi seems to be relatively risk-averse.

Apecoin (APE) as the collateral asset has a large market cap ([$1,058,753,958](https://www.coingecko.com/en/coins/apecoin#markets)) and the token has high liquidity, but mostly on centralized exchanges (Binance, Coinbase, KuCoin, Kraken, and many others), while on decentralized exchanges most liquidity is on Uniswapv3.



<p id="gdcalert13" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image13.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert14">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image13.png "image_tooltip")


(source: [UniswapV3 Analytics](https://info.uniswap.org/#/tokens/0x4d224452801aced8b2f0aebe155379bb5d594381))

However, as highlighted in the beginning, most of the ApeUSD in circulation is not backed by ApeCoins. This always has to be kept in mind when interacting with the protocol.


## Oracle Risks

Ape Finance uses Chainlink as the price-feed [provider](https://data.chain.link/ethereum/mainnet/crypto-usd/ape-usd) for ApeCoin. The token price is updated every time Chainlink post a new price on-chain. Relying on on-chain price updates prevents APE price manipulation inside one block (flash loan attacks), but with trade-off is that the Chainlink price can be different from the global price in some cases.

That “trade-off” does not represent a significant risk, because borrowed $ApeUSD on the lending protocol is over-collateralized with an LTV that is set to a conservative 60%.


# Discussion and Conclusion



1. Is it possible for a single entity to rug its users?

Yes, Ape Finance could rug its users. [ApeApe.sol](https://etherscan.io/address/0xcaB90816f91CC25b04251857ED6002891Eb0D6Fa) and [apeApeUSD.sol](https://etherscan.io/address/0xc7319dBc86A121313Bc48B7C54d0672756465031#code) are both controlled by the admin multi-sig (2-out-of-4) with a 2-day timelock.

None of the team members are doxxed and out of four signers, only one can be “verified” as a real user (i.e. a community member with an ENS). The other signing EOAs are probably created only for signing purposes (based on EOA activity).



2. If the team vanishes, can the project continue?

Probably not, because Ape Finance’s multi-sig controls most of the protocol’s key mechanisms (e.g. unitroller markets, maintaining peg with add/remove actions, ApeUSD available for borrowing, lending parameters, etc.).



3. Do audits reveal any concerning signs?

No audit was performed on Ape Finance smart contracts directly.


# Conclusion

In conclusion, Ape.fi leaves the impression of a high-risk protocol. This comes from a few observations:



* The project's documentation isn't very detailed but it’s clear and informative. However, their communication and promises are different from what is actually happening. That’s usually a red flag.
* There seems to be no community or any attempts to build DAO-like structures. Ape.fi has no Discord (only a Telegram chat). There is no forum, no governance process, and the [Snapshot](https://vote.ape.fi/#/) page is only used to vote on ApeCoin DAO meta-governance decisions (with very little participation). It’s a fully centralized project.
* Nobody outside the initial group of developers has any influence over Ape.fi whatsoever. The multi-sig controlled by the team has full control over the entire protocol. Another red flag.
* The majority of ApeUSD has no backing. The practice they use to keep it stable is questionable and clearly more exploitative to Curve than it is adding value.
* As of today, the protocol has not found product-market fit. There’s very little native TVL of APE tokens. There are no use cases for ApeUSD outside the Curve factory pool. Over 98% of the circulating stablecoins belong to the protocol and most of it is used to actively farm CRV, CVX, and FXS.
* Even though there are no signs that the project’s founders are enriching themselves, the protocol’s treasury certainly is rewarded chiefly through their farming activities. Especially when compared to the value-add they bring to Curve or its partners.
* ApeFinance is a fork of forks that has not been audited and they only refer to a CREAM audit. This is probably the largest red flag.




## Sources
