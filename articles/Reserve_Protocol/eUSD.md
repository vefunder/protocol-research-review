### A look into the protocol for permissionless stablecoin deployment and its eUSD stablecoin

**Useful Links**

- Protocol Website

  - [Reserve Protocol](https://reserve.org/)
  - [Register.app (Explorer/UI for RTokens)](https://register.app/)

- Docs

  - [Reserve Protocol](https://reserve.org/protocol/)
  - [Electronic Dollars (eUSD) Token Details](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)
  - [Electronic Dollars (eUSD) Whitepaper](https://mobilecoin.com/files-uploads/2022/10/MobileCoin_Stablecoin_Whitepaper_Final_Edits.pdf)

- Github

  - [Reserve Protocol](https://github.com/reserve-protocol/protocol)

- Important contracts

  - [eUSD](https://etherscan.io/address/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)
  - [eusdRSR](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8)
  - [RSR](https://etherscan.io/address/0x320623b8e4ff03373931769a31fc52a4e78b5d70#code)

- [Access Control Google Sheet](https://docs.google.com/spreadsheets/d/1LULGdPtJc148l2GgmoGT2Pum-hGOK-UzNlKJHLFbG70/edit?usp=sharing)

- Governance (Multisig address, gov contract, gov reference)

  - [eUSD Guardian (Governor Alexios) (GnosisSafeProxy)](https://etherscan.io/address/0x576ca79e46171a2B3E26F13a4334940eBcD72164#code)
  - [eUSD (Owner Address, TimelockController)](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c#code)

- Market Data (Curve pool, analytics)

  - [eUSD-3CRV Pool](https://etherscan.io/address/0xaeda92e6a3b1028edc139a4ae56ec881f3064d4f)
  - [eUSD-3CRV Gauge](https://etherscan.io/address/0x8605dc0c339a2e7e85eea043bd29d42da2c6d784)

- [MobileCoin Electronic Dollar Auditor](https://auditor.mobilecoin.foundation/)



## Relation to Curve

Reserve has three RTokens in Curve pools with proposed or enacted gauges: eUSD, hyUSD, and ETH+. A [proposal](https://gov.curve.fi/t/proposal-to-add-eusd-fraxbp-gauge/8938) to add the [eUSD+FraxBP](https://curve.fi/#/ethereum/pools/factory-v2-277/deposit) Curve pool to the gauge controller was posted on March 6th, 2023. A second gauge was [proposed](https://gov.curve.fi/t/proposal-to-add-hyusd-eusd-to-the-gauge-controller/9234/3) for the [hyUSD+eUSD](https://curve.fi/#/ethereum/pools/factory-crypto-252/deposit) pool on May 16th. Both proposals passed a DAO vote on [March 13th](https://dao.curve.fi/vote/ownership/293) and [May 31st](https://dao.curve.fi/vote/ownership/336). Most recently, a gauge [proposal](https://gov.curve.fi/t/proposal-to-add-eth-eth-to-the-gauge-controller/9272) was posted for a third RToken pool for the [ETH+/ETH](https://curve.fi/#/ethereum/pools/factory-crypto-256/deposit) pool.

This article aims to provide information relevant for Curve LPs about Reserve Protocol and the mechanics behind RTokens, with particular focus on its flagship stablecoin eUSD ([Electronic Dollar](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)). To unpack eUSD, we’ll start with an overview of [MobileCoin](https://mobilecoin.com/files-uploads/2022/10/MobileCoin_Stablecoin_Whitepaper_Final_Edits.pdf), a protocol for confidential tokens that enables payment on mobile devices. We then introduce [Reserve Protocol](https://reserve.org/en/protocol/), a DeFi platform built on Ethereum that enables the permissionless creation of stablecoins. Finally, we will cover relevant risk parameters for the benefit of LPs exposed to eUSD.

Much of the information in this report is applicable to the hyUSD and ETH+ RTokens, but for the sake of clarity, this report will focus primarily on eUSD.


## MobileCoin Introduction

[MobileCoin](https://mobilecoin.com/files-uploads/2022/09/Mechanics-of-MobileCoin-v0-0-39-preview-10-11.pdf) is a directed acrylic graph (DAG) cryptocurrency blockchain based on the Stellar Consensus Protocol and Monero. It was launched in 2017 with the objective to develop a high-throughput, private and easy-to-use cryptocurrency that could be integrated into mobile apps like WhatsApp or Signal. Because of its focus on privacy, earlier technical developments from 2021 focused on cryptographic concepts like [ring signatures and secure enclaves](https://mobilecoin.com/files-uploads/2022/09/Mechanics-of-MobileCoin-v0-0-39-preview-10-11.pdf).


### eUSD from Conception to Deployment

In 2022, MobileCoin released the [white paper](https://mobilecoin.com/files-uploads/2022/10/MobileCoin_Stablecoin_Whitepaper_Final_Edits.pdf) for Electronic Dollars (eUSD). It would be the first asset to natively use MobileCoin’s confidential tokens functionality, making it the first private digital dollar and circumventing the need to use mixers as has been done on Ethereum and Bitcoin. 

eUSD was deployed as a partnership between MobileCoin and Reserve Protocol to both Ethereum and MobileCoin on [February 24th, 2023](https://medium.com/reserve-currency/introducing-the-electronic-dollar-eusd-cb9d8f50aae), along with a KYC/AML-permissioned bridge. The architecture combines the high security assurances and DeFi composability of Ethereum with the low-fee and privacy feature of MobileCoin. Ethereum secures eUSD's basket of collateral types where core protocol mechanics are governed by a DAO. eUSD can then be bridged to MobileCoin where it can scale as a medium of exchange by being an accessible payments platform optimized for mobile devices. 

### eUSD Bridge

The process for wrapping and unwrapping eUSD between MobileCoin and Ethereum involves a bridge managed by Reserve Protocol using elliptic-curve signatures co-signed by pre-approved liquidity providers. There is a 1:1 relationship between wrapped eUSD tokens on the MobileCoin blockchain and eUSD ERC20 tokens stored in a [Gnosis Safe](https://app.safe.global/balances?safe=eth:0x30DA4EB397215cF407C46854CA7188f4e60F3402) multisig on Ethereum. Minting and burning of wrapped eUSD is [verifiable](https://auditor.mobilecoin.foundation/) on the MobileCoin blockchain, while correspondent deposits and withdrawals from the custodian multisig are verifiable on the Ethereum chain.

While eUSD is intended to be a private digital dollar on the MobileCoin network, its architecture includes a manual, permissioned bridge with core protocol functionality taking place on Ethereum. A primary signer (Reserve) co-signs verified transactions for locking/unlocking and minting/burning on either side of the bridge between MobileCoin and Ethereum. A visual diagram of the process is provided below:

![wrapping_eusd](https://github.com/PaulApivat/temp/assets/4058461/3a6a91c9-6a97-498f-9714-286f68693e42)

[source](https://medium.com/reserve-currency/introducing-the-electronic-dollar-eusd-cb9d8f50aae)

With that context, the next section will introduce the Reserve Protocol and protocol mechanics for eUSD on Ethereum.


## Reserve Protocol Introduction

The Reserve Protocol is a DeFi platform that allows the permissionless creation of assets (called "[RTokens](https://reserve.org/en/protocol/rtokens/)") backed by a user defined basket of ERC20 tokens. Once deployed, anyone can mint the RToken by depositing the specified proportion of all basket assets, and likewise redeemed for a proportional share of the basket assets. RTokens are mintable / redeemable for the ERC20 collateral basket at all times, so long as the Reserve Protocol is fully collateralized. Holders of the Reserve Rights (RSR) governance token are incentivized with yield earnings to stake in various RTokens to provide additional overcollateralization of the RToken backing. 

Newly deployed RTokens can theoretically be pegged to any unit, with existing RTokens being denominated in [US Dollars (e.g. eUSD)](https://register.app/#/overview?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) and [Ether (e.g. ETH+)](https://register.app/#/overview?token=0xE72B141DF173b999AE7c1aDcbF60Cc9833Ce56a8).  The highest marketcap RToken as of June 4th is eUSD (~$17MM), followed by hyUSD (\~$540K) and ETH+ (\~$500K).


### History of Reserve

The Reserve Protocol released their [original white paper](https://reserve.org/assets/files/whitepaper.pdf) in 2018 ([deprecated](https://reserve.org/en/protocol/2018_version/#-version-of-the-reserve-protocol)). Much of the protocol implementation first described in the original paper still exist today, including the OG native stablecoin Reserve Dollar (RSV) and the Reserve Rights governance token (RSR). 

Changes that led to its current iteration of Reserve Protocol involves the following:

- The Reserve Dollar (RSV) was primarily used in the [Reserve Mobile App](https://play.google.com/store/apps/details?id=rsv.walletapp.reserve&hl=es&gl=US)
- Reserve felt that RSV was [too centralized](https://twitter.com/reserveprotocol/status/1549429110811889667?s=20) and would need to be superceded by decentralized RTokens.
     - Reasoning given by Reserve team: _"Centralization of control and liability. It doesn’t make sense economically for any party to take on liability of depegs of underlying. Generally, the economics & incentives didn’t seem scalable. But by leveraging yield bearing collateral and allowing governors to direct yield to various parties, it seems like it has best chance at scaling and lasting into future."_   
- The Reserve Protocol has evolved into a platform to enable the permissionless [creation and governance of many stablecoins](https://register.app/#/deploy) (called “RTokens”). 
- The Reserve Rights (RSR) token facilitates the stability of all RTokens created on the platform.
     - RSR is not used to overcollateralize RSV, and it is has mostly been deprecated in favor of the upgraded RToken platform. 


### RToken Collateral

Reserve Protocol incentivizes RSR staking (and therefore accelerated adoption of the RToken) by generating revenue from the yield-bearing collateral basket. RTokens are typically backed by interest earning receipt tokens, such as deposits in lending protocols like Aave and Compound. Available collateral tokens are included in the [AssetRegistry](https://etherscan.io/address/0x9B85aC04A09c8C813c37de9B3d563C2D3F936162). There are limitations to what can serve as collateral including [non-compatible](https://reserve.org/en/protocol/rtokens/) ERC20 assets like:

- Rebasing tokens
- Tokens that take a fee on transfer
- Tokens that do not expose the decimals() in their interface
- ERC777 tokens which could allow reentrancy attacks
- Tokens with multiple addresses
- Tokens that do not adhere to the ERC20 standard in general

Tokens with any of these limitations will need to be [wrapped into a compatible ERC20](https://reserve.org/en/protocol/rtokens/) to qualify as collateral assets. For example, eUSD uses wrapped Aave deposits (saTokens) rather than the standard aToken to prevent rebasing.

In addition to the basket of ERC20 tokens that back each RToken 1:1, the protocol enables overcollateralization by allowing its native token RSR (Reserve Rights) to be staked on any RToken. RSR holders may choose to stake to earn a share of yield generated by the collateral basket, a configurable parameter specific to each RToken. Staked RSR (stRSR) can be slashed in case of collateral default, as reported by onchain price-feeds that do not rely on governance nor human decision-making. Unstaking involves a cool-down time between 7 and 30 days, during which the protocol retains the right to seize the RSR in the event of default.

An overview of the collateral basket for eUSD, hyUSD, and ETH+ are shown below, along with the revenue split between RToken holders and RSR stakers:

![combined Rtoken baskets](https://github.com/vefunder/protocol-research-review/assets/51072084/df00e74c-c495-4cd9-90c7-804242bf84c3)

Source: [Register App](https://register.app/#/)

Since RTokens generate yield by lending their collateral assets and governance can direct a portion of revenue to RSR stakers, RTokens that generate and share revenue are the ones likely to attract stakers and be [protected](https://reserve.org/en/protocol/overall_design/?search=protected#s-result) by overcollateralization, as opposed to RToken governance that choose not to direct revenue to RSR stakers. Aside from being the [first](https://www.poap.delivery/reserve-eusd) RToken on the Reserve Protocol (not counting its native RSV), Electronic Dollars (eUSD) allocates 100% of its revenue to RSR stakers. This likely contributes to its considerably larger marketcap compared to other RTokens. 


#### Revenue for RToken Holders and RSR Stakers

(Note: Links in this section use eUSD contracts as an example, as it is the predominant RToken and the main focus of this report)

The basic process for sharing revenue begins with the [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4) contract where collateral assets are managed. When the yield-bearing basket increases in value relative to the outstanding RToken supply, the Backing Manager will either mint new RToken or trade profits from its backing for RToken or RSR through the [RToken Trader](https://etherscan.io/address/0x3d5EbB5399243412c7e895a7AA468c7cD4b1014A)/[RSR Trader](https://etherscan.io/address/0xE04C26F68E0657d402FA95377aa7a2838D6cBA6f) contract. Revenue shared with RToken holders is sent to the [Furnace](https://etherscan.io/address/0x57084b3a6317bea01bA8f7c582eD033d9345c2B2) contract where it gradually burns the RToken, increasing the redemption value. Revenue shared with RSR stakers is sent to the [stRSR pool](https://etherscan.io/address/0x18ba6e33ceb80f077deb9260c9111e62f21ae7b8) where it is slowly distributed as rewards to increase the stRSR/RSR value.

Note that it is also possible to share revenue additionally with an arbitrary address, such as to compensate the token deployer. For example, hyUSD makes a 3% allocation to the hyUSD treasury, which they provide an explanation for in their [Reserve proposal](https://forum.reserve.org/t/rfc-introducing-hyusd/404#what-will-the-initial-revenue-distribution-look-like-15).

The following flowchart shows revenue distribution to RToken holders and RSR stakers with a theoretical 40/60 revenue split.

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/9b6070a5-9677-48ba-bbfb-836f3ae58f5f)

Source: [Reserve Docs: Protocol Operations](https://reserve.org/protocol/protocol_operations/)


#### stRSR to Overcollateralize eUSD

eusdRSR stakers earn rewards based on three factors:

- Amount of revenue eUSD generates
- Portion of revenue governance has directed to eusdRSR stakers
- Portion of total eusdRSR staked on eUSD

For eUSD, [100% of revenue earned is directed](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) to eusdRSR stakers. With governance conducted by eusdRSR holders, there is an incentive to keep eUSD safe from unnecessary risk, as their funds are literally at stake. 

In addition to their role in governance, eusdRSR stakers provide overcollateralization to eUSD and are subject to seizure in case the collateral backing for eUSD defaults. For example, [during USDC’s depegging in March 2023](https://medium.com/reserve-currency/eusd-emerges-strong-the-resilience-of-reserve-protocol-during-usdc-depegging-e5a698a990c9), eUSD required emergency action. Since it is 50% backed by USDC (approx 25% saUSDC, 25% cUSDC), the protocol had to sell off their backing for emergency collateral (USDT). Through the process, eusdRSR stakers helped [re-collateralized eUSD](https://www.poap.delivery/reserve-eusd) in defense of its peg. 

In this instance, eusdRSR stakers provided overcollateralization for eUSD as the USDC portion of the backing collateral defaulted. The general process of recapitalizing any RToken is shown below: 

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/12392892-1d1c-4a9a-b503-6a38e0efe1ea)

Source: Reserve Docs: [Recapitalization](https://reserve.org/en/protocol/protocol_operations/#recapitalization)

Unstaking eusdRSR requires a 2 week [delay](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8#readProxyContract) as specified by governance. This is necessary to prevent self interested actors from frontrunning an impending default event and provide additional assurances to anyone with eUSD exposure. During the delay, the staker does not earn rewards to prevent stakers from misusing the staking mechanic by repeatedly [unstaking and re-staking](https://reserve.org/protocol/reserve_rights_rsr/#reserve-rights-staking) into the contract.


#### eUSD Basket Rebalancing

The capitalization and backing of eUSD can be characterized by [two distinct states](https://reserve.org/en/protocol/protocol_operations/?search=redeem#s-result):

- **Fully collateralized**: the [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#code) contract holds the right balance of the collateral tokens to offer 100% redeemability.
- **Fully funded**: there is the right amount of value, but not necessarily the right amount of collateral to offer 100% redeemability.

While the Reserve Protocol aims to be fully collateralized at all times, it won’t always be. For example, if governance decides to change the collateral basket or, in cases of market volatility (see USDC depeg scenario above), emergency collateral has to be swapped in as the defaulting collateral is auctioned off. eUSD may be fully funded (right amount of value), but not be fully collateralized (right amount of collateral tokens). When not fully collateralized, the protocol will attempt to sell off the excess asset until the system is either fully collateralized or RSR is required to recapitalize the system.

Rebalancing during normal operation and recapitalizing during emergencies are conducted through [auctions](https://register.app/#/auctions?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) conducted through [Gnosis Auctions](https://gnosis-auction.eth.link/#/start). The protocol uses a [Gnosis Trade](https://etherscan.io/address/0xAd4B0B11B041BB1342fEA16fc9c12Ef2a6443439) contract to trade against the Gnosis EasyAuction mechanism. Governance can set the auction length that strikes a balance between allowing time for arbitrage and swiftly executing trades. The default value is 15 minutes per auction. 


### RToken Governance

All RTokens launched on Reserve Protocol are governed separately by their respective communities with governance parameters codified upon RToken deployment. For example, eUSD (Electronic Dollars) will make governance choices independent of other RTokens like [RSV](https://register.app/#/overview?token=0x196f4727526eA7FB1e17b2071B3d8eAA38486988) (Reserve Protocol’s native stablecoin), [ETH+](https://register.app/#/overview?token=0xE72B141DF173b999AE7c1aDcbF60Cc9833Ce56a8) (an Ethereum-aligned Liquid Staking Token basket), or [hyUSD](https://register.app/#/overview?token=0xaCdf0DBA4B9839b96221a8487e9ca660a48212be) (a decentralized flatcoin that provides access to DeFi yields which also recently received a [Curve gauge](https://vote.convexfinance.com/#/proposal/0xbdb05edfcbdeececc5331d1194cae539a366cf37b4269faeb08d8305ea09568f)). 

Reserve Protocol has a modified version of the [OpenZeppelin Governor](https://docs.openzeppelin.com/contracts/4.x/api/governance) called [Governor Alexios](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6) which is suggested to RToken deployers by default. However, deployers are able to [specify](https://reserve.org/en/protocol/reserve_rights_rsr/?search=alexios#s-result) governance parameters as they see fit, from single EOA governance to any arbitrary DAO structure. Governor Alexios allows RSR holders to propose, vote and execute proposals. RSR holders can also delegate their voting power to other addresses. 

Governor Alexios is the governance contract implemented by all RTokens relevant to Curve (eUSD, hyUSD, and ETH+). For example, eUSD's governable parameters are viewable [here](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) and are configurable by [eusdRSR](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6#readContract) stakers (RSR stakers on the eUSD RToken):

- Proposal threshold (0.01%)
- Quorum (15%)
- Voting Snapshot Delay (2 days, 14,400 blocks)
- Voting Period (3 days, 21,600 blocks)
- Execution delay (3 days)

Governance contracts significant for eUSD operation include:

- [Timelock contract](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c)
- [Governor Alexios contract](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6)

The Governor Alexios contract, a modified version of [OpenZeppelin Governor](https://docs.openzeppelin.com/contracts/4.x/api/governance), allows [eusdRSR](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8) holders to propose, vote and execute proposals. The TimelockController mediates this process by introducing a timelock once a proposal is approved, adding a delay between approval and execution, giving RToken holders to make a decision before something is changed. The process of approving to execute is 8 days for eUSD (i.e., [voting snapshot delay: 2 days, voting period: 3 days, execution delay: 3 days](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)). 


#### Governance Params: eUSD Collateral Basket

In addition to providing overcollateralization, governance will also define the basket backing eUSD, as well as a list of emergency collateral. 

Here is the configuration of ERC20 tokens that serve as collateral as proposed and voted by eusdRSR holders:

- Static Aave Interest Bearing USDC ([saUSDC](https://etherscan.io/address/0x60C384e226b120d93f3e0F4C502957b2B9C32B15)) (25%)
- Static Aave Interest Bearing USDT ([saUSDT](https://etherscan.io/address/0x21fe646D1Ed0733336F2D4d9b2FE67790a6099D9)) (25%)
- Compound USD Coin ([cUSDC](https://etherscan.io/address/0x39AA39c021dfbaE8faC545936693aC917d5E7563)) (25%)
- Compound USDT ([cUSDT](https://etherscan.io/address/0xf650C3d88D12dB855b8bf7D11Be6C55A4e07dCC9)) (25%)

Source contract: [Basket Handler](https://etherscan.io/address/0x6d309297ddDFeA104A6E89a132e2f05ce3828e07#readProxyContract)

The Prime Basket defines the collateral needed to be deposited for issuance and it consists of an array of triples: <collateral token, target unit, target amount>. The logic is contained in the [basket handler contract](https://etherscan.io/address/0x6d309297ddDFeA104A6E89a132e2f05ce3828e07#code). For example, the interest bearing Aave USDC token (saUSDC) is represented in the array as <saUSDC, USD, 0.25> (see also [bytes32 serialization for USD](https://github.com/reserve-protocol/protocol/blob/master/docs/collateral.md)). In the case where the prime basket is updated by governance due to collateral defaults, the protocol will determine a new reference basket and make changes to the collateral makeup. Finally, the ordered list of [pre-defined emergency collateral](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) consists of pure stablecoins including: USDC, USDT, USDP, TUSD and DAI.

eusdRSR stakers are in charge of registering, unregistering and swapping ERC20 assets as either collateral or revenue assets. This is achieved through governance to interact with the [Asset Registry contract](https://etherscan.io/address/0x9B85aC04A09c8C813c37de9B3d563C2D3F936162#code), with the following functions:

- Register: Add assets to Asset Registry
- swapRegistered: Allows to modify/update details and functionality of previously registered asset
- Unregister: Removes asset from Asset Registry

With eUSD’s prime basket of collateral defined, anyone can deposit the required collateral tokens (i.e., saUSDC, saUSDT, cUSDC, cUSDT) to issue eUSD and conversely, burn eUSD to redeem the collateral tokens. 


#### Advanced RToken parameters

There are a multitude of other RToken parameters as set in the [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#readProxyContract) that regulate mint/redeem action including:

- Trading delays(s) which define how many seconds should pass after the basket has been changed before a trade can be opened. For eUSD, this is set by the [Backing Manager](https://etherscan.io/address/0x6d309297ddDFeA104A6E89a132e2f05ce3828e07#code) contract to [2 hours](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) or 7,200 seconds
- Backing buffer (%) as collateral tokens appreciate, eUSD can be minted whenever the correct ratio of collateral tokens is gathered, providing revenue capture. Collateral tokens get sent to the [Revenue Trader](https://etherscan.io/address/0x3d5EbB5399243412c7e895a7AA468c7cD4b1014A#code) contract to mint additional eUSD that can be used as yield for [eusdRSR](https://etherscan.io/address/0x18ba6e33ceb80f077DEb9260c9111e62f21aE7B8) stakers. This is currently set to [0.01%](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) 
- Max trade slippage (%) is the maximum deviation from oracle prices that any trade the protocol can clear. Maximum trade slippage permits additional price movement beyond worst-case oracle pricing. The setting for eUSD is [1%](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) 
- Minimum trade volume represents the smallest amount of value worth executing a trade for; eUSD [minimum trade volume](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) is set to $1,000 
- RToken Maximum trade volume is the maximum sized eUSD trade allowable for Reserve Protocol operations (e.g. trading through Auctions), currently set to [1e29](https://etherscan.io/address/0x3d5EbB5399243412c7e895a7AA468c7cD4b1014A#readProxyContract#F2)
- Auction Length(s) is determined by the [Broker](https://etherscan.io/address/0x90EB22A31b69C29C34162E0E9278cc0617aA2B50#code) contract and sets how long auctions stay open for. If set too low, arbitrageurs won’t have enough time to complete arbitrage loops; if set too high, fewer auctions will fill due to volatility risk. Currently, this is set to [900 seconds](https://etherscan.io/address/0x90EB22A31b69C29C34162E0E9278cc0617aA2B50#readProxyContract) [(15 minutes)](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) for eUSD 


### Stablecoin Peg Mechanisms

eUSD is designed to trade at $1.00 reflecting the market value of the entire collateral basket while 100% of revenue from earned interest is directed by governance to go towards eusdRSR stakers. Any deviation from $1.00 is designed to get arbitraged toward the reference price.

This will happen through issuance and redemption mechanisms. The eUSD RToken contract has specific functions to regulate the process of issuance and redemption. Issuance throttle limits how much eUSD can be issued, to limit value extraction in case of an exploit. After a large issuance, the issuance limit ‘recharges’ to the defined maximum. The redemption throttle works similarly where the protocol tries to ensure the net redemption for eUSD never exceeds an hourly limit. The specific parameters for eUSD are as follows: 

- Issuance throttle ([1,000,000 eUSD](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) maximum amount per hour) 
- Issuance [throttle rate](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) at 2.5% of eUSD supply
- Redemption throttle ([1,500,000 eUSD](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) maximum amount per hour)
- Redemption [throttle rate](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#readProxyContract) at 5.0% of eUSD supply
- [Source](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)

The following section provides case scenarios for when eUSD deviates from peg:

**Scenario 1: eUSD trading below peg**

Assume that eUSD is currently trading at $0.95 cents. An arbitrageur notices the price difference and decides to buy eUSD at the discounted price. The vast majority of liquidity for eUSD is in the [eUSD/FraxBP](https://etherscan.io/address/0xaeda92e6a3b1028edc139a4ae56ec881f3064d4f) Curve pool, so eUSD is most likely to be sourced there.

After buying eUSD, the arbitrageur would then redeem it for the basket of collateral tokens backing its peg (see Prime basket). The redemption throttle limits the maximum amount that can be redeemed to 1,500,000 eUSD or 5% of the supply in a given hour. If the redemption throttle/throttle rate has been triggered, the arb may revert for up to an hour until the throttle becomes inactive (This is an unlikely scenario meant to mitigate losses during a potential exploit).

The arb would redeem a proportional share of the eUSD basket ie. $0.25 saUSDC, $0.25 saUSDT, $0.25 cUSDC, $0.25 cUSDT per 1 eUSD redeemed. This would net the arb a 5% profit - exchange fees and gas cost.

Overall, arbitragers would help eUSD regain its peg by buying the stablecoin at a discount, redeeming it for the underlying collateral, and thereby reducing the supply of eUSD in circulation until its price returns to the peg.

**Scenario 2: eUSD trading above peg**

If eUSD is trading above peg at $1.05 cents, arbitragers would take advantage of this price discrepancy by minting new eUSD and selling on the market (likely into the Curve eUSD/FraxBP pool) until regaining the peg value of $1.00 USD.

Arbitragers would first attain the underlying eUSD collateral tokens by swapping and wrapping to the correct propotion of the basket. They would then deposit these collateral tokens to mint eUSD at the 1:1 peg value. The arbitragers would then sell the eUSD back on the market as long as it is trading above peg.


### Token Distribution 

As of June 7th, five entities account for [99% of eUSD supply](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#balances) on Ethereum:

![Screen Shot 2023-06-07 at 9 18 54 AM](https://github.com/vefunder/protocol-research-review/assets/51072084/1ebeabc8-c3ab-40fe-bc4a-d055fa28f1f4)

Source: [Etherscan: eUSD Tokenholders](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#balances)

- ~6,100,000 eUSD in [eUSD/FraxBP](https://etherscan.io/address/0xaeda92e6a3b1028edc139a4ae56ec881f3064d4f) Curve pool

- 5,990,000 eUSD in a [2-of-4 Multisig](https://etherscan.io/address/0x3c1d46c769f3c09b22c7e9c7144eb392102088b7)

  - This is the multisig of funds held on the RPay app. [One of the signers](https://etherscan.io/address/0x1871Cf6F04bD5dC3414B6EB0130B09712a6CF2F1) is also on Reserve’s [Slow Wallet Multisig](https://etherscan.io/address/0xA7b123D54BcEc14b4206dAb796982a6d5aaA6770#code). 

- ~5,400,000 eUSD in an [EOA](https://etherscan.io/token/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F?a=0x79e76c14b3bb6236dfc06d2d7ff219c8b070169c) that appears to do arbitrage operations.

- 510,002 eUSD in a [2-of-2 multisig](https://app.safe.global/balances?safe=eth:0x30DA4EB397215cF407C46854CA7188f4e60F3402) 
     - This multisig is [verifiable](https://auditor.mobilecoin.foundation/) as the MobileCoin bridge multisig managed by Reserve (There is a correspondent 510,002 eUSD on MobileCoin). This [2-of-2 multisig](https://etherscan.io/address/0x30da4eb397215cf407c46854ca7188f4e60f3402) is itself controlled by 2 additional multisigs. These two additional multisigs are held by a 1-of-3 and 2-of-3 multisigs themselves. 

- ~500,000 eUSD in [eUSD/hyUSD](https://etherscan.io/address/0x8a8434a5952ac2cf4927bbea3ace255c6dd165cd) Curve pool


### Market

Electronic Dollars (eUSD) trading is currently limited to decentralized exchanges. [Coingecko](https://www.coingecko.com/en/coins/electronic-usd#markets) lists the DEX 4swap with trading pairs eUSD/USDC and eUSD/MOB, although liquidity these is very low. Onchain data suggests most of the trading activity is on Curve within the [eUSD/FraxBP pool](https://curve.fi/#/ethereum/pools/factory-v2-277/deposit) consisting of eUSD, FRAX, and USDC.

The swap pairs have shown up in different pair patterns on DEXes as depicted here:

![eUSD_dex_pairs](https://github.com/PaulApivat/temp/assets/4058461/aad97558-97e9-4e09-b24f-acef0380ddc6)

[Query source](https://dune.com/queries/2452911/4041758)

Date and amount traded between the pairs can be queried in the table below:

![eUSD_dex_trade](https://github.com/PaulApivat/temp/assets/4058461/a1186d17-9233-45eb-90a9-40698da2ae4d)

[Query source](https://dune.com/queries/2458380/4042115)

eUSD only recently had its first transaction on Ethereum as of [February 23, 2023](https://etherscan.io/tx/0x949e6329b55a89b0874ef99256048ffde458bc4f2236e0e87331321a13a8bef8), the starting date from which we can check Coingecko for its price stability. For the most part, eUSD has maintained peg during its short life, with the exception of [March 11th](https://medium.com/reserve-currency/eusd-emerges-strong-the-resilience-of-reserve-protocol-during-usdc-depegging-e5a698a990c9), when USDC depegged bringing nearly all stablecoins in DeFi off their respective peg. Fortunately, [eusdRSR stakers](https://www.poap.delivery/reserve-eusd) were able to help eUSD regain peg as emergency collateral (USDT) was swapped in. 

![eUSD_price_stability](https://github.com/PaulApivat/temp/assets/4058461/d899bf32-ae97-464a-a447-e9e64aaf0391)

[Coingecko: Electronic USD price chart](https://www.coingecko.com/en/coins/electronic-usd)

Since the first time eUSD was minted for its underlying collateral basket (saUSDC, saUSDT, cUSDC, cUSDC) on [February 23, 2023](https://etherscan.io/tx/0x949e6329b55a89b0874ef99256048ffde458bc4f2236e0e87331321a13a8bef8), eUSD supply has expanded to $18.6MM by June 7th. A notable uptick in total supply occurred on March 23, 2023 after the passing of the [Curve gauge vote](https://etherscan.io/tx/0x5708af06b4f1514bc64dcfd9c8cee14c2aa0f5c1c7e2de914d1c2bdfa2726d31). 

![Screen Shot 2023-06-07 at 9 05 51 AM](https://github.com/vefunder/protocol-research-review/assets/51072084/63be7c65-6f19-472a-9ec8-c94229eec9ca)

[Query source](https://dune.com/queries/2612639/4334228)

The following chart shows RSR that has been staked on eUSD as eusdRSR over time. We see the largest spike in staked eusdRSR on March 23rd, following the addition of the Curve gauge. 

![eusdRSR_staked_ts](https://github.com/PaulApivat/temp/assets/4058461/7a520b58-3168-42b9-b33c-d7020686658b)

[Query source](https://dune.com/queries/2465756/4055736)

For other additional charts showing the state of Electronic Dollars, we have created a [Dune Dashboard](https://dune.com/paulapivat/llama-risk-assessment-electronic-dollar-eusd).


## Risk Vectors

### Smart Contract Risk

[Trail of Bits](https://github.com/reserve-protocol/protocol/blob/master/audits/Trail%20of%20Bits%20-%20Reserve%20Org%20-%20Final%20Report.pdf) submitted a security assessment on August 11, 2022. The audit found five high, two medium, three low severity and five informational issues. The high severity issues are summarized in the table below. The team has taken action to mitigate severity around Access Control (see Centralization Risk), but it is unclear how the other four high severity issues were addressed. 

High severity issues found:

- Lack of a two-step process for contract ownership changes
- All auction initiation attempts may fail (see [EasyAuction](https://etherscan.io/address/0x0b7fFc1f4AD541A4Ed16b40D8c37f0929158D101#code) contract)
- All attempts to initiate auction of defaulted collateral tokens will fail (affects Recapitalization strategy and [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#code) contract)
- An RSR seizure could leave the stRSR contract unusable
- System owner has excessive privileges. The owner of the [Main](https://etherscan.io/address/0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a#code) contract has excessive privileges (see next section for mitigation actions).

A [Code4Arena audit](https://github.com/reserve-protocol/protocol/blob/master/audits/Code4rena%20Reserve%20Audit%20Report.md) was conducted in April, 2023 and two high and twenty seven medium severity issues were found. The status of the two high severity issues is provided here:

- Adversaries can abuse a quirk of Compound redemption to manipulate the underlying exchange rate and maliciously disable cToken collaterals. | [Mitigation confirmed](https://github.com/code-423n4/2023-02-reserve-mitigation-contest-findings/issues/35)
- Basket range formula is inefficient, leading to unnecessary haircuts for the protocol. | [Not fully mitigated.](https://github.com/code-423n4/2023-02-reserve-mitigation-contest-findings/issues/49) 

[Ackee Blockchain](https://github.com/reserve-protocol/protocol/blob/master/audits/Ackee%20-%20abch-reserve-protocol-report-1.1.pdf) also provided an audited report to the Reserve Protocol on October 7, 2022 finding three medium issues and six warnings. The issues have either been acknowledged or fixed. 

The Reserve Protocol has been audited by [Solidified](https://github.com/reserve-protocol/rsr-mainnet/blob/master/audits/solidified/Audit%20Report%20-%20Reserve%20Token%20%5B3%20Jan%202022%5D-2.pdf) as of January 4, 2022. There were no critical, major or minor issues found. The one issue around missing input address validation had been resolved. While code complexity was at a medium level, code readability, documentation and test coverage were all high. 

[Halborn](https://github.com/reserve-protocol/protocol/blob/master/audits/Halborn%20-%20Reserve_Protocol_Smart_Contract_Security_Audit_Report_Halborn_Final.pdf) also conducted a Smart Contract Security Audit from August 28th, 2022 - October 10th, 2022 and did not find any critical flaws in the protocol. Any security risks found were mostly addressed by the Reserve team. 

Finally, there is an on-going bug bounty program by [Immunefi that has been live since April 27, 2023](https://immunefi.com/bounty/reserve/). The bounty program is offering $100,000 to a staggering $5,000,000 for disclosure of critical level bugs. 

In summary, the Reserve Protocol team has subjected themselves to several rounds of audits. Two of the auditors found several high severity issues. It is recommended the team takes steps to publicly report their progress in addressing these issues.


### Centralization Risk

Reserve Protocol uses Role Based Access Control (RBAC) to mitigate potential centralization risk. These are core [system states and roles](https://reserve.org/en/protocol/system_states_roles/) in Reserve Protocol’s RToken governance system. 

- Owner: The top level role that can grant/revoke roles to any address, pause/unpause the system, freeze/unfreeze the system, set governance parameters and upgrade smart contracts.
- RToken Pauser: can pause and unpause an RToken’s (i.e., eUSD) system in case of an emergency such as a Chainlink feed failing. There can be multiple Pausers. Pausing means RToken issuance, un-staking RSR, withdrawing RSR, trading and RToken melting are disabled 
- Short Freeze: can freeze an RToken’s system for three days, generally assigned to an entity that can spot bugs and react swiftly. There is some tolerance for false positives, though less than the Pauser role. There can be multiple Short Freezer, as is the case with [eUSD](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F), as well as robot-controlled entities. 
- Long Freeze: can freeze an RToken’s system for one week. This role should be optimized for no false positives, and is expected to act slowly and surely. There are fewer Long Freezers, with TimelockController being one of two for eUSD.

The [Main contract](https://etherscan.io/address/0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a#code) is central to the functioning of [eUSD](https://etherscan.io/address/0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F#code). The Main contract is linked to other contracts essential to various protocol operations including: Asset Registry, Backing Manager, Basket Handler, eUSD, eusdRSR, Broker, Furnace, Distributor, RToken Trader and RSR Trader (see [Access Control sheet](https://docs.google.com/spreadsheets/d/1LULGdPtJc148l2GgmoGT2Pum-hGOK-UzNlKJHLFbG70/edit?usp=sharing)). 

There are certain contracts that have ownership and/or permissions over Main, shown below. Critical ownership privileges are granted to the TimelockController contract for the Governor Alexios contract and additional roles are spread out to two  EOAs and two multisig wallets.

- Pausers who can emergency pause eUSD: [1-of-3 msig](https://etherscan.io/address/0x7f9ffa8dea49647725ca6ce621e03aa20401ff63), [Timelock contract](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c), [EOA1](https://etherscan.io/address/0x4bad40e0d92ebc2620a0d5aff7887c7f2e67fdd8), [EOA2](https://etherscan.io/address/0xe45c3179b135288dd8e1e3c20eafebb2b2e7d771), [EOA3](https://etherscan.io/address/0x0d88776dd9a654cfe9c67b5b1d9ce2fddd815a34)
- Short Freezers who can temporarily freeze eUSD for 3 days: [Timelock contract](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c), [1-of-3 msig](https://etherscan.io/address/0x4986164f2fb9bb898b19382b1e835cd9c81eba46), [EOA1](https://etherscan.io/address/0x4bad40e0d92ebc2620a0d5aff7887c7f2e67fdd8), [EOA2](https://etherscan.io/address/0xe45c3179b135288dd8e1e3c20eafebb2b2e7d771), [EOA3](https://etherscan.io/address/0x0d88776dd9a654cfe9c67b5b1d9ce2fddd815a34)
- Long Freezers who can temporarily freeze eUSD for a week: [Timelock contract](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c), [2-of-3 msig](https://etherscan.io/address/0x0173e2965c1aec9d395eb14e3407ef95c2e1a47d)

![Permissions_over_Main](https://github.com/PaulApivat/temp/assets/4058461/99dd225f-a4de-40bc-9176-7a055d223297)

[TimelockController](https://etherscan.io/address/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c#code) holds a host of [permissions on Main](https://pod.xyz/podarchy/0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a?lens=permissions&node=0x7697aE4dEf3C3Cd52493Ba3a6F57fc6d8c59108a) and plays a high authority role such as RToken [Pauser, Short Freeze and Long Freeze](https://reserve.org/en/protocol/system_states_roles/) ([source](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F)). 

In an attempt to balance decentralization with precautionary levers, the eUSD team has assigned two entities (Alexios Governor and a 2-of-3 Multisig) [permission over TimelockController](https://pod.xyz/podarchy/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c). Alexios Governor has the TIMELOCK_ADMIN_ROLE and the multisig has the CANCELLER_ROLE. This allows the multisig to revert a potentially malicious governance action. Entity relationships are shown here:

![Permissions_over_TimelockController](https://github.com/PaulApivat/temp/assets/4058461/d053e783-7a6f-44b1-9aae-fa1adf3aa8f2)

As the top decision maker, the Owner role should be reserved for a decentralized governance smart contract, as it is with the [Governor Alexios](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6) contract for eUSD. However, each RToken has a unique governance configuration, so each must be considered in isolation. Users should not assume that because eUSD has taken precautionary measures against centralization risk that another RToken will have the same assurances.

Although Governor Alexios allows eusdRSR holders to directly participate in governance, there are privileges granted to the [2-of-3 Multisig](https://etherscan.io/address/0x576ca79e46171a2B3E26F13a4334940eBcD72164#code) that shares [power over the TimelockController](https://pod.xyz/podarchy/0xc8Ee187A5e5c9dC9b42414Ddf861FFc615446a2c) (see: [Access Control](https://docs.google.com/spreadsheets/d/1LULGdPtJc148l2GgmoGT2Pum-hGOK-UzNlKJHLFbG70/edit?usp=sharing)). We were unable to find documentation on this address, although the Reserve team clarified to us that the multisig comprises members of the Reserve and Mobilecoin team, and its authority is limited to preventing malicious governance proposals. The role of this address should be clearly described in the documentation if not done so already.

It is the opinion of this author that the Reserve Protocol team has taken step to address this potential centralization risk by distributing permissions over Main, RToken_Pauser, Short_Freeze and Long_Freeze roles, across an additional four externally owned addresses (EOAs) and an additional two multisig natures wallets as depicted below (see [Access Control sheet](https://docs.google.com/spreadsheets/d/1LULGdPtJc148l2GgmoGT2Pum-hGOK-UzNlKJHLFbG70/edit?usp=sharing)). While certain addresses have significant privileges and control over protocol operations and governance, the system has been designed to mitigate potential centralization risk with a diversity of non-overlapping externally owned accounts (EOAs) and multisig wallets serving as a check and balance on power. Should any of the multisig signers disappear, there is sufficient distribution of permissions such that the protocol could continue functioning. 

![mitigate_centralization_risk](https://github.com/PaulApivat/temp/assets/4058461/4d87cc85-d659-4035-87a5-8b8ab357e7ce)


### Collateral Risk

The Electronic Dollar (eUSD) is 100% backed by a basket of yield bearing stablecoins during normal operation (cUSDC, cUSDT, saUSDC, saUSDT) with [emergency collateral](https://register.app/#/settings?token=0xA0d69E286B938e21CBf7E51D71F6A4c8918f482F) of pure stablecoins (USDC, USDT, USDP, TUSD, DAI) that can be exchanged for prime basket assets in times of emergency. Additionally, eUSD is overcollateralized with staked RSR (eusdRSR). As of this writing, the eUSD supply is 20% overcollateralized with RSR. 

Although eUSD backing is entirely made up of stable assets, certain circumstances can cause insolvency, necessitating the use of RSR to recapitalize the outstanding supply. This has indeed happened once in eUSD's short history, during the USDC depeg on March 10th, 2023. Reserve was fortunate that this black swan event occurred before the introduction of Curve gauge incentives and a consequent supply expansion. The total marketcap of eUSD was only ~1,000,000 at the time, and the shortfall required $32,000 worth of RSR to recapitalize. 

![Screen Shot 2023-06-08 at 10 27 15 AM](https://github.com/vefunder/protocol-research-review/assets/51072084/b5ec7ada-999a-45dd-8b9b-28f373dcfd1b)

The recapitalization process involved three steps:

1) [Tx](https://etherscan.io/tx/0xaa8eef1a5810dd66e224a822cc3baa406ce63c2635dce343ede4bf2adbdf6a1c) | Call manageTokensSortedOrder() to the [Backing Manager](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4 ). This is the contract that handles the eUSD collateral basket. It transfers 7,997,787 RSR from the [stRSR](https://etherscan.io/address/0x18ba6e33ceb80f077deb9260c9111e62f21ae7b8) contract, socializing losses to RSR stakers. Funds are transfered to the [Gnosis Trader](https://etherscan.io/address/0x547e5af53d8aad34361fa75ac29ccc027be68075) contract that handles auctions with [Gnosis EasyAuction](https://etherscan.io/address/0x0b7ffc1f4ad541a4ed16b40d8c37f0929158d101).
2) [Tx](https://etherscan.io/tx/0x14a4a3a56ea17fd86c39b4be0766084057c92985562ab71933a1bc3dd4474d36) | A highest bid for the 7,997,787 RSR is made for 32,400 USDT.
3) [Tx](https://etherscan.io/tx/0x69a6aaa14bb34a2176332ddde5870e5ae52ef347fb637612a3b53956e679b565) | settleTrade() is called to the Backing Manager, claiming the 32,400 USDT to the Backing Manager.

Interestingly, the address who placed the bid for RSR back in March 22nd has not claimed the funds and they still reside in the EasyAuction contract. This suggests perhaps that this was an internal bailout by either a team member or Reserve investor not necessarily motivated by profit opportunity from the action. 

The event serves to demonstrate the performance of the recapitalization mechanic in prod and highlight possible issues that may arise during a larger scale event. The system may be too slow to react in transitioning to emergency collateral, and in the case of temporary depegs, may result in uneccesary losses by triggering changes to the basket at the most inopportune times. The auction process may be too slow during times of high volatility and network congestion, which are times when recapitalization is most likely to become necessary. Market participants may front run an expected recapitalization event, making it more difficult to raise necessary funding from RSR. 

It is unclear whether RSR can scale as overcollateralization protection as eUSD marketcap expands. RSR has a relatively thin market, with only $1.3MM in the [RSR/FraxBP](https://curve.fi/#/ethereum/pools/factory-crypto-136/deposit) pool. Furthermore, a major event that slashes RSR stakers may deteriorate community morale to the point that all RTokens face difficulty in recovering and attracting stakers in the future. The protocol mechanic is certainly a comforting feature, but the effectiveness at scale remains to be seen.

As an additional point, as identified in the audit report from [Code4Arena](https://github.com/reserve-protocol/protocol/blob/master/audits/Code4rena%20Reserve%20Audit%20Report.md), there is a quirk in Compound that could maliciously disable cToken collaterals in eUSD (currently: cUSDC and cUSDT). There is always compounded collateral risk when making use of yield bearing assets by introducing exposure to external protocols like Compound and Aave. 

Governance can change the basket of assets backing eUSD, which can expose users to risk from the protocol shifting the allocation and potentially becoming undercollateralized. Users furthermore must remain aware of changes to the backing based on governance decisions to make informed decisions on their risk appetite.


### Oracle Risk

The Reserve Protocol uses only [Chainlink](https://github.com/reserve-protocol/protocol/blob/master/contracts/plugins/assets/OracleLib.sol) price feeds with no apparent backup. The Backing Manager contract has [maxTradeSlippage](https://etherscan.io/address/0xF014FEF41cCB703975827C8569a3f0940cFD80A4#code), setting maximum deviation from oracle prices that a trade can clear at. Moreover, the protocol has the pause, short and long freeze functions to mitigate any extensive oracle failure situation. 

The oracle is significant because it informs the system of collateral default, which will cause the system to sell collateral for emergency collateral alternatives or rebalance the collateral backing.


## LlamaRisk Gauge Criteria

**Centralization Factors**

1. Is it possible for a single entity to rug its users?

No. eUSD has put a DAO contract ([Governor Alexios](https://etherscan.io/address/0x7e880d8bD9c9612D6A9759F96aCD23df4A4650E6)) governed by eUSD stakers as the central governing entity. There is additionally a  diverse set of EOAs and multisigs that share precautionary permissions over the protocol (e.g. Pauser role). Users should be aware, however, that every RToken can establish its own governance, which could offer a different level of user assurances.  

2. If the team vanishes, can the project continue?

Yes. The Reserve Protocol platform is designed such that anyone can permissionlessly create a new RToken through a system of factory smart contracts. The Electronic Dollars (eUSD) project is governed by eUSD stakers who can change parameters and upgrade contracts for eUSD.

**Economic Factors**

1. Does the project's viability depend on additional incentives?

No. The project is currently viable without incentives, but a deeper Curve pool that combines eUSD with other stablecoins could further strengthen its peg.  

2. If demand falls to 0 tomorrow, can all users be made whole?

Yes. Electronic Dollars (eUSD) is currently sufficiently overcollateralized such that users can be made whole if demand fell to 0. The process of redemption is permissionless, including unwrapping the underlying collateral tokens (Aave and Compound deposits).

**Security Factors**

1. Do audits reveal any concerning signs?

Audits revealed a number of high severity issues, but Reserve appears to place a high priority on security. They have undergone multiple audits and have an active bug bounty program with a maximum $5MM payout. Although the code and protocol design is fairly complex, the team has made efforts to ensure high readability and clarity in their documentation and test coverage. 


## Risk Team Recommendation

(Don't worry about this section in the first draft, we will discuss together and with the protocol team to determine our final recommendation)

