
# Cross-Chain Gauges & Ecosystem Assessment: Multichain

A deep dive into cross-chain router Multichain, its complex ecosystem, and how Curve profits from its novel technology.


# Useful Links



* [Multichain website](https://multichain.org/) / [Docs](https://docs.multichain.org/getting-started/introduction) / [GitHub](https://github.com/anyswap) / [Security Model](https://docs.multichain.org/getting-started/security/security-model) / [Bug Bounty](https://docs.multichain.org/getting-started/security/bug-bounty-immunefi) / [Medium](https://medium.com/multichainorg) / [Coingecko - $MULTI](https://www.coingecko.com/en/coins/multichain)
* Multichain Dashboards - [Tokens](https://scan.multichain.org/#/tokens) / [Network](https://scan.multichain.org/#/network) / [Dashboard](https://scan.multichain.org/#/dashboard) / [dApp](https://app.multichain.org/#/pool)
* Multichain anyCall - [Whitepaper](https://drive.google.com/file/d/1NFFFecAjStbGMyvJVDez3xmsGSHYvNYv/view) / [Gitbook](https://multidao.gitbook.io/anycall/) / [Audit Reports](https://github.com/anyswap/Anyswap-Audit)
* Articles:
  * LI.FI Blog - [Multichain - A Deep Dive](https://li.fi/knowledge-hub/multichain-a-deep-dive/)
  * Publish0x - [MultiChain Deep Dive](https://www.publish0x.com/lifi/multichain-a-deep-dive-all-you-need-to-know-about-multichain-xelworv)
  * TheDefiant - [Questions on Multichain Funds](https://thedefiant.io/questions-on-multichain-funds)
* Etherscan:
  * Multichain [Deployer 1](https://etherscan.io/address/0x4464fc279180045b1f57beacfa0d82e9cd4235cd), [Deployer 2](https://etherscan.io/address/0x49addc76569402cf893c015ac69a177aaf4e822b), [Deployer 3](https://etherscan.io/address/0x8132d337977f8d145a7b9cfac393df123c0f0e43), [Deployer 4](https://etherscan.io/address/0xfa9da51631268a30ec3ddd1ccbf46c65fad99251)
  * anyCall [anyCall v6 Proxy](https://etherscan.io/address/0xc10ef9f491c9b59f936957026020c321651ac078#contracts) / [Contract Reader](https://www.contractreader.io/contract/0xC10Ef9F491C9B59f936957026020C321651ac078)
  * Curve [Root Gauge Factory](https://etherscan.io/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#_blank)
  * Curve [Gauge Controller Contract](https://etherscan.io/address/0x2F50D538606Fa9EDD2B11E2446BEb18C9D5846bB#code)
* Curve: 
  * Github - [Cross-chain Gauges Factory](https://github.com/curvefi/curve-xchain-factory)
  * Medium - [Curve anyCall](https://medium.com/multichainorg/curve-x-anycall-cross-chain-gauges-e657c1c6e5b5)
  * Forum - [Multichain Gauges](https://gov.curve.fi/t/cip-62-63-multi-chain-gauges-to-spread-the-crv-love-on-side-chains-and-l2s/1747) (CIP 62/ CIP 63)
  * Dev docs - [Gauges for EVM chain](https://curve.readthedocs.io/dao-gauges-sidechain.html)
* Tweet thread highlighting Multichain concerns - [Barket.eth](https://twitter.com/bkiepuszewski/status/1572537869536989184)


## TL;DR of our Findings



* Multichain is a cross-chain router protocol (CRP) that provides the infrastructure for arbitrary cross-chain interactions. The protocol enables cross-chain interoperability of tokens, NFTs, and general data across multiple EVM and non-EVM blockchains.
* To secure cross-chain interactions, Multichain relies on a trusted set of external validators - the SMPC network. SMPC is composed of 21 nodes (SMPC 2.0 in beta has 39) which independently mint and manage part of the private key (TTS algorithm). They need to collectively reach a consensus to verify transactions.
* When it comes to cross-chain value transfers, Multichain is a hybrid bridge solution. First, it uses a “lock-and-mint” bridging concept. Funds sit in escrow EOA’s (MPC address) on Ethereum for every destination chain. Secondly, the protocol uses a liquidity network with liquidity pools for every token. LP shares for pools are represented by ERC-20 anyTokens (anyDAI, anyUSDC, anyETH, etc.).
* Using different bridging solutions for over 82 different blockchains comes with some trade-offs. One of them is liquidity fragmentation or consequently a lack of liquidity. In the worst case, if there is not enough liquidity, users receive a share of the liquidity pool in the form of anyTokens. These represent a claim to a share of the pool's liquidity should it become available again.
* Liquidity inefficiencies became visible after critics claimed that user funds were transferred from an escrow wallet and rehypothecated. This led to the conclusion that the SMPC nodes are tightly coordinated, and that they can control user funds. Moreover, it revealed that not all tokens in circulation (incl. [IOU](https://en.wikipedia.org/wiki/IOU) tokens) are also backed by funds in escrow.
* For cross-chain data transfers, Multichain built the anyCall application. anyCall is a cross-chain messaging tool that can send cross-chain messages and call contracts from one chain to another. Curve was one of the first protocols to deploy anyCall for its cross-chain gauges.
* Multichain was audited several times. All relevant products had several audits by reputable auditors. There is also an active bug bounty program and additional security measures.
* No severe risks were discovered in relation to Curve’s cross-chain gauge implementations.


# Introduction

Multichain is a cross-chain router protocol that provides the infrastructure for arbitrary cross-chain interactions. The project was launched as a DEX named Anyswap in July 2020. In late 2021, the protocol [pivoted](https://medium.com/multichainorg/multichain-is-one-year-old-d00d7b7f2694) and started to solely focus on interoperability between blockchains. It rebranded to Multichain and raised $60M in funding. The [financing round](https://medium.com/multichainorg/multichain-previously-anyswap-raises-a-60m-financing-round-led-by-binance-labs-b94c65afd989) was led by Binance Labs, joined by Sequoia China, IDG Capital, Three Arrows Capital, DeFiance Capital, Circle Ventures, and the Tron Foundation among others.

Since rebranding, Multichain has experienced strong growth and set an impressive pace for releasing new products and features. Consequently, Multichain is now recognized as a [leading provider](https://cointelegraph.com/news/leader-in-cross-chain-releases-tool-to-easily-call-contracts-between-any-network) of cross-chain interoperability solutions. The protocol connects over 80 chains with nearly 3,000 crypto bridges (i.e. unique tokens) and has more than 50 project integrations. Multichain is one of few cross-chain protocols to bridge EVM-compatible chains (e.g. Ethereum, Polygon, Optimism, etc.), with non-EVM chains (e.g. Bitcoin, Near, Aptos, etc.).

Moreover, the protocol takes a market-leading position in terms of TVL ([$1.8B](https://scan.multichain.org/#/charts/charts-home)) and total volume transferred ($96.3B), according to their own dashboard. The image below provides an overview of Multichain’s impressive performance metrics (for an updated view click the link below).

![multichain-dashboard](https://user-images.githubusercontent.com/89845409/216052723-9abae189-1880-4dca-8306-61551ba121d4.png)

(source: [Multichain Dashboard](https://scan.multichain.org/#/dashboard))

Since its existence, Multichain has suffered from two smaller exploits. One in July 2021, where Multichain’s router was [exploited](https://medium.com/multichainorg/anyswap-multichain-router-v3-exploit-statement-6833f1b7e6fb) for $2.3M USDC and $5.5M MIM. And another [exploit](https://www.coindesk.com/business/2022/01/20/multichain-hack-worsens-as-loss-of-funds-reaches-3m-report/) occurred in January 2022. This time, the attacker was able to steal roughly $3M. The hack occurred after a vulnerability in Multichain’s bridge became public. Both vulnerabilities were remedied by the team pretty quickly. And in August last year, Multichain published an extensive [list](https://medium.com/multichainorg/detailed-disclosure-of-multichain-security-policy-bde0397accf5) of measures put in place to improve its security. More on that later.


# The Multichain Product Suite

So far Multichain has released four main products, and one network (SMPC) to support the product's security. Below is a high-level overview of all relevant components.



* **The SMPC Network** - SMPC stands for Secure Multi-Party Computation network. The network is the core technology that stabilizes and secures all of Multichain’s products. It consists of 21 nodes that are responsible to sign transactions, deploy new bridges, and secure the decentralized management accounts (i.e. where the native tokens are stored). Each node of the network holds a piece of the private key that is used for signing transactions. So nodes can only sign transactions collectively. Essentially, the MPC network is a security feature based on the [GG20 algorithm](https://docs.multichain.org/getting-started/how-it-works) that allows the operation of all of Multichain's products in a somewhat decentralized manner without having to rely on EOA’s or multi-sigs.
* **The Bridge** - Multichain uses a lock and mint mechanism to bridge assets between two chains. Each [bridge](https://docs.multichain.org/getting-started/how-it-works/cross-chain-bridge) constitutes a link between two blockchains. To transfer an asset, the native token is locked on the origin chain in Multichain’s MPC smart contracts (secured by the 21 nodes). Once locked, a wrapped version of the token is minted on the destination chain. For instance, DAI is locked on Ethereum into the MPC, and a wrapped-DAI version is minted on Fantom. To reverse the process (i.e. to bridge back the asset), the wrapped version is burned and the locked token is released on the origin chain. To mint these wrapped tokens, Multichain deploys their AnyswapV6ERC20 contract on each destination chain. It’s a superset of the standard ERC20 contract that only allows the MPC network to mint tokens.
* **The Cross-chain Router** - The [router](https://docs.multichain.org/getting-started/how-it-works/cross-chain-router) allows asset transfers between two or more chains. The bridge mentioned above is limited to assets that are native to the origin chain but don’t yet exist on the destination chain. Certain assets, however, are already deployed to several chains. For instance USDC. The bridge cannot mint a wrapped version of USDC to a chain where USDC is already deployed. The router, on the other hand, is designed to bridge these pre-existing assets, by using liquidity pools. Anyone can provide liquidity to Multichain’s pools. The router ensures cross-chain transfers, by allocating pool shares to users accordingly. In cases where Multichain has deployed the AnyswapV6 contract, the router can also be used to transfer assets between contracts. This reduces the liquidity required. For example, if a bridge is deployed from chain A to B, the router can help to transfer assets to chain C, as long as the asset is not native to chain C. This allows a generalization of bridges so that assets are not required to return to their source chain before being sent to another chain.
* **AnyCall** - AnyCall is a cross-chain messaging protocol that enables users to send messages and call contracts from chain A to chain B. AnyCall uses a trigger and a call API that correspond with each other. Messages are transmitted across chains via the SMPC network. This allows users to call a smart contract function on chain A to trigger an event on chain B. Curve uses AnyCall to determine boosted CRV rewards to veCRV holders across multiple chains (more on that later). AnyCall is also integrated into the bridge and the router to enable functions that trigger the bridging of assets.
* **NFT Bridge and Router** - Multichain offers the same products listed above also for NFTs, i.e. ERC721 and ERC1155 token standards. However, in this report, the focus will be on ERC20 implementations.

In summary, Multichain is a hybrid bridge. It uses the classic lock-and-mint model (used by most bridges) in combination with liquidity pools using the router. It’s also a hybrid in terms of on- and off-chain components. The SMPC network constitutes the off-chain trust layer. It’s responsible for validating transactions. The on-chain layer consists of all other components, such as the smart contracts for the bridges or the trigger and call API used by anyCall.

Besides the above products, Multichain provides additional infrastructure and tools.



* The Multichain [explorer](https://scan.multichain.org/) gives access to live data on successful and pending transactions.
* The [network dashboard](https://scan.multichain.org/#/network) is an overview of all SMPC nodes and their status.
* Multichain also provides [charts](https://scan.multichain.org/#/charts/charts-home) offering live data on transaction volume and TVL.
* The recently launched [Co-Mint](https://medium.com/multichainorg/co-mint-multichains-universal-cross-chain-bridge-48da559a96b7) feature allows multiple bridges to co-mint the same assets.


## Secure Multi-Party Computing (SMPC) Network

The off-chain component is the network that transmits the messages. As mentioned above, SMPC consists of 21 [nodes](https://scan.multichain.org/#/network), which are open for public participation. MPC (Multi-Party Computing) is a method where a single private key is subdivided and encrypted across several nodes, making it impossible for bad actors to access or reveal the private key by reverse-engineering transactions. According to Multichain, the nodes are run by different organizations, institutions, and individuals. However, many of them are members of the Multichain team.

Together, the MPC network controls all EOAs (containing user funds). Further, they control all transfers of assets between chains, sign transactions, and transmit cross-chain messages. The nodes control the private keys to perform all these actions. For the signatures, the protocol implemented a [TTS](https://docs.multichain.org/getting-started/how-it-works) (Threshold Signature Schemes) algorithm. Each node selected from the network independently receives part of the private keys (to an EOA) that is required for signing transactions. This TSS is denoted as (t, n), where for n possible signatories a minimum of t - (n/2+1≤ t ≤ n) is required. Only “t” colluding players can forge the signature. In other words, validating transactions on the SMPC network requires a majority of nodes to sign the transaction.

Essentially, the security of Multichain is in the hands of the 21 nodes, therefore requiring that a majority of the network (11) is always honest.

## anyCall

anyCall is a collection of on-chain and off-chain components that work together to perform cross-chain messaging. The on-chain components consist of the anyCall and anyExec smart contracts, and the API.

The anyCall contract resides on the source chain storing information such as instructions, data, and events. These need to be executed on the destination chain. The anyExec contract contains instructions that understand the source chain’s contract standards. It makes the information compatible with the destination chain where the information is used to execute the respective functions. anyCall plugs into an API to access the contracts on the source chain and the anyExec contract on the target chain.

![multichain-gitbook-anycall-v7-workflow](https://user-images.githubusercontent.com/89845409/216053625-ae5217f1-effa-4052-9887-d12628e9f393.png)

(source: [MultiDAO Gitbook](https://multidao.gitbook.io/anycall/the-latest-version/v7))



# Multichain’s Integrations

Multichain has a market-leading position in terms of asset bridging and routing. Over 3000 unique bridges have been deployed, whereby Fantom, Ethereum, and Binance (BNB) hold the most [TVL](https://defillama.com/protocol/multichain). The most active [user addresses](https://dune.com/Howard_Peng/multi-chain-on-chain-data) are on BNB, Ethereum, and Polygon. With around [$1.8B](https://scan.multichain.org/#/charts/charts-home) in TVL, Multichain almost makes it into the top 10 of protocols by TVL (Compound is currently ranked [#10](https://defillama.com/) with $1.89B TVL).

The majority of funds are on Fantom ([~43%](https://scan.multichain.org/#/charts/charts-home)). Fantom was one of the first bridges supported by Multichain and many tokens on Fantom consider Multichain bridged tokens as the “standard token”. For instance, nearly 100% of all [USDC](https://ftmscan.com/token/0x04068da6c83afcfa0e13ba15a6696662335d5b75#readContract) on Fantom was transferred via [Multichain](https://scan.multichain.org/#/tokens) (see image below). 

![ftmscan-usdc-bridged](https://user-images.githubusercontent.com/89845409/216052841-28cbebaa-856e-4be3-bc96-2f71878d8ecf.png)

(source: [ftmscan](https://ftmscan.com/token/0x04068da6c83afcfa0e13ba15a6696662335d5b75#readContract))

The same is true for $MIM on [Avalanche](https://snowtrace.io/token/0x130966628846bfd36ff31a822705796e8cb8c18d), $DAI and $USDT on Fantom, and many other [tokens](https://scan.multichain.org/#/tokens) that use Multichain. Thus, Multichain is a systemically important technology for many tokens on several EVM-compatible chains.


## What TVL is correct?

The numbers above should be taken with a grain of salt. First, one has to agree on the correct way to measure the TVL. This can be tricky for a hybrid bridge like Multichain, where the lock-and-mint method is used in combination with a liquidity network. All “external” data providers report significantly lower TVL. According to DeFi Llama, Multichain's TVL is [$809M](https://defillama.com/protocol/multichain)- $1B lower than what Multichain reports. L2Beat reports even less ([$594M](https://l2beat.com/bridges/tvl)). Cryptoflows finds only [$370M](https://cryptoflows.info/bridges) in TVL, and Messari comes up with a non-identical number ([$805M](https://messari.io/protocol/multichain)).

These data providers don’t claim to be up-to-date. DeFi Llama did [sunset](https://twitter.com/DefiLlama/status/1608948358579617794?s=20&t=c_UX5Y94fDz6GipRKnx4Kw) their bridge dashboard due to high maintenance efforts. Notably, Multichain’s own TVL dashboard asserts more than double what any other source reports.

This results from applying a different method for measuring TVL. Multichain’s bridges store native tokens, usually on the respective source chain. The protocol also has liquidity pools on destination chains. On top of that are the synthetic wrapped tokens and the anyTokens (IOUs minted via the Anyswap bridge contract). Multichain accounts all of these tokens toward its TVL. One could argue that Multichain thus shows the “available liquidity” within its ecosystem. However, it’s questionable whether this equates to TVL (i.e. value locked by users). Put differently, Multichain conflates user deposits with outstanding IOU tokens. This can result in double accounting.

Given the high number of deployed bridges, chains, liquidity pools, and EOAs that store the native assets, it becomes a mammoth task to calculate Multichain's actual TVL. However, it’s clearly lower than figures reported on their website.


## Increasing Complexity

Multichain is already a very large and complex ecosystem. To complicate matters further, the Multichain’s SMPC network does not always burn its minted tokens. This problem is well summarized in a thread from bartek.eth.

![twitter-bartek-thread](https://user-images.githubusercontent.com/89845409/216052949-b3eeaf56-c6fa-4b2c-984f-f8c6bac50c6c.png)

(source: [Twitter](https://twitter.com/bkiepuszewski/status/1572537821487058944))

To better understand the situation, it helps to differentiate between the three token types. Using $DAI and the Fantom bridge as an example, the following tokens are used:



1. The original Ethereum-based DAI held in escrow on Multichain’s [MPC Fantom bridge](https://etherscan.io/address/0xC564EE9f21Ed8A2d8E7e76c085740d5e4c5FaFbE) contract.
2. The “[wrapped-Fantom-DAI](https://ftmscan.com/address/0x8d11ec38a3eb5e956b052f67da8bdc9bef8abf3e#readContract)”. All Fantom-based DAI can only be minted via Multichain. In theory, Fantom-DAI is only minted when someone deposits DAI into the MPC bridge on Ethereum (and vice-versa).
3. Lastly, there is [anyDAI](https://etherscan.io/token/0x739ca6d71365a08f584c8fc4e1029045fa8abc4b#balances) that can have two functions. (1) It can be minted to a chain where DAI already exists. Here anyDAI is used by the router and is exchangeable for “real” DAI via Multichain's liquidity pools. (2) It also functions as an IOU token. This can happen when there is not enough liquidity to send the user actual DAI. In this case, users receive anyDAI, which represents a claim to a share of the liquidity pool.

In short, the issue described by Bartek is that Multichain allegedly transferred the underlying Ethereum-based DAI in escrow to provide liquidity elsewhere. Essentially, the network is moving user funds without their permission. The story was also picked up by [The Defiant](https://thedefiant.io/questions-on-multichain-funds). According to Multichain, this was done to efficiently distribute liquidity within the Multichain ecosystem. They also ensure that the funds are safe and enough DAI is still available. However, in addition to moving DAI, Multichain didn’t burn the corresponding wDAI on Fantom. In other words, the MPC network can circumvent the burn function.

Instead, the transferred DAI was used to mint anyTokens elsewhere, to enable more liquidity. At the time of Bartek’s tweet, this led to around $25M of unbacked anyDAI controlled by the SMPC network by this [address](https://etherscan.io/address/0x5e583b6a1686f7bc09a6bba66e852a7c80d36f00). The same address also holds $55M anyUSDC. This further complicates an already complex ecosystem. Moreover, these “empty” tokens are accounted toward Multichains TVL.

In response, Multichain released an [article](https://multichainorg.medium.com/go-deep-into-multichain-cross-chain-mechanism-4a47836be250) a few days after Barteks’ discovery. The blog post agrees with the problem statement, that outstanding wrapped-tokens cannot be larger than collateral. It goes on to confirm that there is enough real DAI in escrow to match the outstanding wrapped-DAI (see the image below from the article).

![multichain-medium-transparency-article](https://user-images.githubusercontent.com/89845409/216053071-1377ad7e-f481-42b1-a1b5-43c89962a275.png)

(source: [Multichain Blog](https://multichainorg.medium.com/go-deep-into-multichain-cross-chain-mechanism-4a47836be250))

As seen on the left side, the outstanding “wrapped-Fantom-DAI” was $99M. The article claims that the right side is backing these tokens (in total $100M). The largest sum is held in the Ethereum [Fantom bridge MPC](https://etherscan.io/address/0xC564EE9f21Ed8A2d8E7e76c085740d5e4c5FaFbE). As stated in the article, the reserves match the outstanding Fantom-DAI. However, the article neglects to consider the outstanding anyDAI.


## Diving Deeper

A second [article](https://medium.com/multichainorg/report-of-five-main-tokens-on-multichain-3c6fbcf5749d) from November 2022 was released a few weeks later. It tries to further shed light on the discussion. This time the blog post lists all outstanding wrapped-DAI positions. Including those on other chains such as Kava or Moonriver. Similar to the first article, the image supposedly “proves” that all wrapped-DAI within the Multichain ecosystem is backed by real DAI.

![multichain-medium-dai-underlying](https://user-images.githubusercontent.com/89845409/216053119-ae320fb8-a721-42c1-a90c-9c539b73a3fe.png)

(source: [Medium](https://medium.com/multichainorg/report-of-five-main-tokens-on-multichain-3c6fbcf5749d))

The left side lists all minted wrapped-DAI on all available chains. Roughly $106M in total. Correspondingly, the right side shows $106M DAI in escrow. However, this list applies an accounting method that does not necessarily reflect the full picture. For instance, the second line from the top right did not contain $28.5M “real” DAI. On the contrary, it contained [$28.5M Fantom-based](https://ftmscan.com/address/0xd652776de7ad802be5ec7bebfafda37600222b48) DAI, which was minted by the Multichain bridge. Moreover, the article neglects to consider all circulating anyDAI. Thus, the outstanding supply is actually much higher than $106M.

Our [research](https://docs.google.com/spreadsheets/d/1EEQkwUe9HSjE1rKXkFJNAcWChKbGV5Aqq-yYh2uZ5tA/edit#gid=0) shows that this is still the case. Currently, there is over $31.6M in outstanding anyDAI and over $85.6M wrapped-DAI throughout various chains. In total, we found ~$117M in circulating DAI (wDAI + anyDAI). This corresponds with the TVL indicated on the Multichain [website](https://scan.multichain.org/#/charts/charts-home). On the other hand, we could only confirm ~$65M DAI in escrow- a ~$52M difference. Note that we neglected a number of dust wallets, so the actual amount in escrow might be slightly higher. 

After consulting with the Multichain team, it became clear that the difference is due to Multichain owning some of the outstanding DAI. First, most anyDAI in circulation belongs to Multichain (it’s not user-owned IOUs). Therefore, Multichain understandably does not count the ~$25M toward outstanding positions. However, it requires users to trust that Multichain will never convert the outstanding anyDAI into real DAI. Secondly, the ~$28.5M DAI in the Fantom liquidity [pool](https://ftmscan.com/address/0xd652776de7ad802be5ec7bebfafda37600222b48) is considered “shared liquidity”. That is liquidity belonging to the bridge and the router simultaneously, which is owned by Multichain. 

In summary, we can conclude that the outstanding DAI positions are higher than the actual DAI in escrow, although it seems that the majority is owned by Multichain. Essentially, users have to trust the Multichain team that their internal accounting is correct and that they don’t convert outstanding DAI positions. Despite hosting several public dashboards, none enable full transparency on this matter. The team mentioned that they are working on creating one.

It’s worth mentioning that this report only took a closer look at DAI. Multichain has thousands of other token bridges. Users are encouraged to take a closer look at the liquidity situation when using these bridges.


# Cross-chain Curve Gauges

Curve leverages three Multichain products to enable cross-chain gauges: The AnyCall app, the cross-chain bridge, and the router. (see [proposal](https://gov.curve.fi/t/cip-62-63-multi-chain-gauges-to-spread-the-crv-love-on-side-chains-and-l2s/1747)).

For cross-chain gauges, CRV gauge emissions are minted on Ethereum and transferred weekly to alt chains. The cross-chain gauge logic consists of four contracts: The [Root Liquidity Gauge](https://github.com/curvefi/curve-xchain-factory/blob/master/contracts/implementations/RootGauge.vy) and [bridger](https://github.com/curvefi/curve-xchain-factory/tree/master/contracts/bridgers) contracts deployed on Ethereum, and the [Child Liquidity Gauge](https://github.com/curvefi/curve-xchain-factory/blob/master/contracts/implementations/ChildGauge.vy) and [Child Liquidity Gauge Factory](https://github.com/curvefi/curve-xchain-factory/blob/master/contracts/ChildGaugeFactory.vy) deployed on the alt chain.

Curve uses Multichain's anyCallProxy to automate gauge deployment across chains and bridge emissions when requested from the alt chain. Additionally anyCall can be used to assign cross-chain boosted CRV rewards based on relative veCRV owned by pool LPs, similar to boosting functionality on mainnet. This funcionality is not currently available from the Curve UI, and has so far seen limited use.

The image below shows an example of a cross-chain gauge setup. From Fantom (child gauge) to Ethereum (root gauge).

![multichain-medium-curve-anycall-gauges](https://user-images.githubusercontent.com/89845409/216053224-f8e81a96-e2e4-49c3-8d96-e804651bec7e.png)

(source: [Medium](https://medium.com/multichainorg/curve-x-anycall-cross-chain-gauges-e657c1c6e5b5))


## How it works

Suppose a user provides liquidity to a Tricrypto Curve pool on Fantom. In order to process CRV rewards, the following steps take place:


1. A new epoch week begins and a call is made to the `user_checkpoint` function at the `RootGauge` Fantom Tricrypto contract on Ethereum. This calculates the CRV gauge emissions allotted to the gauge from the previous week. ([tx](https://etherscan.io/tx/0x2ec5247479cd0e9d0e5162f0bef4de43fc588d51fdcc840397fe4a33d9195068/advanced)) 


![Fantom-RootGauge-user_checkpoint](https://user-images.githubusercontent.com/51072084/217114790-5ac3a60d-c937-4cec-b625-bc343c8ac7fd.png)

(source: [Fantom TriCrypto RootGauge](https://etherscan.io/address/0x319e268f0a4c85d404734ee7958857f5891506d7/advanced#code) Etherscan)


2. `transmit_emissions` mints and transfers the CRV to the Multichain Fantom Bridge via the `bridger` contract.

![Fantom-RootGauge-transmit_emissions](https://user-images.githubusercontent.com/51072084/217122636-e42cea44-e2b2-4852-a507-b3f6fea93a1e.png)

(source: [Fantom TriCrypto RootGauge](https://etherscan.io/address/0x319e268f0a4c85d404734ee7958857f5891506d7/advanced#code) Etherscan)


3. Multichain bridge transmits the message across chains via the SMPC network. Wrapped CRV is minted to the Fantom `ChildGauge`, and is transferred to the `ChildGaugeFactory`. ([tx1](https://ftmscan.com/tx/0x8d0f9fdc3f92a382b704f1ed41a5843807b8f27e797de47d64bb738bdb39d004), [tx2](https://ftmscan.com/tx/0x97839cf6ce72a39d833a2db11f089e92001dc3ba2b16ffe3e5012329f5308fd6))

![Fantom-ChildGaugeFactory-mint](https://user-images.githubusercontent.com/51072084/217117844-54462adc-b4fb-4b99-85f8-09aff71fec17.png)

(Source: [Fantom Tricrypto ChildGaugeFactory](https://ftmscan.com/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#code) Etherscan)


4. Wrapped CRV resides in the `ChildGaugeFactory` contract, which acts as a pseudo-CRV minter where LPs can call `mint` to claim their accumulated CRV rewards.


Curve doesn’t use Multichain for all chains, as L2's have native bridges. For sidechains and alt L1's, this is the architecture as described in the [Curve Xchain Factory](https://github.com/curvefi/curve-xchain-factory) github repo.


# Multichain Security Measures

To ensure the security of the network, Multichain adopted several [security](https://docs.multichain.org/security/security-model) measures. All smart contracts were [audited](https://github.com/anyswap/Anyswap-Audit/find/master) by several auditors such as Certik, Verichains, SlowMist, BlockSec, Dedaub, PeckShield, and Trail of Bits. In addition, Multichain is building an “academic alliance” with global cryptography experts. The audits relevant for Curve cross-chain gauges are:



* The AnyCall [audit](https://github.com/anyswap/Anyswap-Audit/blob/master/SlowMist/SlowMist%20Audit%20Report%20-%20AnySwap%20AnyCall%20App.pdf) by SlowMist. The auditor found two issues (i.e. a missing event record and a risk of excessive authority).
* The router V7 [audit](https://github.com/anyswap/Anyswap-Audit/blob/master/BlockSec/blocksec_audit_multichain_routerv7_v1.0-signed.pdf) by BlockSec. The auditor found three medium issues (i.e. potential reentrancy in balance calculation, incorrect logic in the _retrySwapinAndExecute function, and an externally controllable retry in the _retrySwapinAndExecute function).

Multichain also had audits for the cross-chain bridge. One by TrailofBits ([audit](https://github.com/anyswap/Anyswap-Audit/blob/master/TrailOfBits/Anyswap-CrossChain-Bridge-TrailofBits-Audit-Final%20Report.pdf)) and one by SlowMist ([audit](https://github.com/anyswap/Anyswap-Audit/blob/master/SlowMist/AnySwap%20CrossChain-Bridge%20Security%20Audit%20Report.pdf)). These were extensive audits that covered the entire cross-chain bridge security. Trail of Bits found a total of 19 vulnerabilities. Most of them are related to cryptography. SlowMist discovered seven issues, of which two were high-risk issues, one low-risk issue, and four recommendations. Both high and low-risk vulnerabilities were fixed by the Multichain team.

In addition, Multichain hosts a [bounty program](https://docs.multichain.org/getting-started/security/bug-bounty-immunefi). According to their documentation, they offer bounties between $500 and $1M in proportion to the severity of the discovered vulnerability.

Multichain further installed a [security fund](https://medium.com/multichainorg/multichain-security-fund-f7016aabf7ac). In March 2022, the protocol established a fund into which 10% of Multichain’s transaction fees are being redirected. This fund will be mainly used to compensate users in case of potential losses caused by vulnerabilities in the Multichain system. A potential vulnerability analysis and compensation plan would be managed by the Multichain community. However, the security fund is not public. 


# Risk Vectors


## Technology and Smart Contracts Risk

Multichain has suffered from two exploits. While they were not insignificant, the stolen amount was rather “small” when compared to other bridge exploits. Rekt.news leaderboard [infamously](https://rekt.news/de/leaderboard/) displays that bridges are an appealing target for hackers. The largest exploits have all been bridges. Multichain is the largest cross-chain protocol that hasn’t suffered from a major exploit yet.

Multichain's code was audited by several auditors, including TrailOfBits, PeckShield, and Slow Mist. This strongly reduces the risk of faulty smart contracts. However, as with all protocols, there is always the possibility that bugs remains undiscovered. Given the complexity of bridge operations, there is always a technology risk beyond general smart contract bugs. These risks relate to software failure, human error, spam, and malicious attacks that can possibly disrupt Multichain's operations.


## Governance Risk

Currently, there is no direct governance risk. Multichain hints here and there that it aims to move toward a DAO. However, besides the introduction of [veMULTI](https://medium.com/multichainorg/vemulti-proposal-stake-multi-get-multichain-fees-rewards-d8d13b9e20cb), there are few signs of an active DAO, in the sense that the protocol is steered by anybody other than the team. For instance, the [governance forum](https://multichaindao.org/) is not in use. Discord has no governance channel. There is no on-chain governance tool, and [Snapshot](https://snapshot.org/#/multichaindao.eth/) had only a few signaling votes. In conclusion, control over the protocol is in the hands of the 21 SMPC nodes and the team.


## Custody and Network Risk

As highlighted above, Multichain’s MPC network controls the protocol. This puts the custody and network risk into the hands of the 21 nodes. In theory, it is possible that a majority of nodes maliciously collude and assemble a private key. They could drain user funds or otherwise harm the network. As discovered by Bartek, the MPC network is already moving funds. As the number of nodes increases, such an attack will become increasingly more difficult to coordinate. However, the fact that the MPC nodes can move DAI for liquidity purposes shows that the team has a strong influence over the network.

In conclusion, users have to rely on the reputation of the node operators. A list of all nodes is public on the Multichain [website](https://scan.multichain.org/#/network). Many are part of the team, which is mostly doxxed. From the perspective of Curve, the risk is reduced to the weekly CRV gauge rewards.


## anySwap Risk to Curve Governance

Curve is rather limited in its cross-chain governance features, including ramping A, changing fees, eventually being able to modify ema params, gauge ownership, implementations etc. While anySwap could be employed to enable such governance features (and indeed these are features Curve will require to be a truly cross-chain platform), it is not advisable to do so. Where anySwap has been integrated, for cross-chain rewards boosting, the risk is siloed by the limited number of CRV bridged each week. In the worst case, a bad blockhash could give someone 100% boosted CRV for a short duration, but all other pool behaviors would be unaffected. The Curve team and LlamaRisk agree that the security guaruntees of anySwap are not sufficient to allow it unbridled custody of Curve's governance across all chains. (Thanks to Curve dev @Skellet0r for his perspective on this subject)

# Discussion



1. Is it possible for a single entity to rug its users?

No, users can only be rugged by a coordinated attack of SMPC node validators. Currently, we can't state that this type of attack is impossible, because of [past events](https://thedefiant.io/questions-on-multichain-funds). The protocol used escrow funds to transfer liquidity without the user’s consent. However, a rug is highly unlikely. The team is doxxed and it still requires more than just the team to collude (i.e. 11 out of 21 nodes). In addition, the new fastMPC is planned to replace the SMPC network. fastMPC comes with a higher number of nodes (40+), which will increase the decentralization of the network.



2. If the team vanishes, can the project continue?

From a technical perspective: Yes, the project can continue without the team. But it can't continue without its validators. The SMPC network plays a crucial role in protocol operations. From an organizational perspective: There is no functioning governance system in place. Despite some recent attempts to transition to a MultiDAO, the team is still the focal point leading the project.



3. Do audits reveal any concerning signs?

No, there were no concerning signs discovered. Multichain had several public audits, which were performed by reputable auditors like CertiK, PeckShield, and TrailofBits, among others. All revealed vulnerabilities were fixed by the Multichain team.


# References



* [Multichain website](https://multichain.org/) / [Docs](https://docs.multichain.org/getting-started/introduction) / [GitHub](https://github.com/anyswap) / [Security Model](https://docs.multichain.org/getting-started/security/security-model) / [Bug Bounty](https://docs.multichain.org/getting-started/security/bug-bounty-immunefi) / [Medium](https://medium.com/multichainorg)
* Coingecko - [$MULTI](https://www.coingecko.com/en/coins/multichain)
* Multichain Medium - [Transparency is all that matters](https://multichainorg.medium.com/go-deep-into-multichain-cross-chain-mechanism-4a47836be250)
* Multichain Medium - [Open to Transparency](https://medium.com/multichainorg/report-of-five-main-tokens-on-multichain-3c6fbcf5749d)
* Multichain Medium - [Multichain is one year old](https://medium.com/multichainorg/multichain-is-one-year-old-d00d7b7f2694)
* Multichain Medium - [Disclosure of Security Policy](https://medium.com/multichainorg/detailed-disclosure-of-multichain-security-policy-bde0397accf5)
* Multichain Dashboards - [Tokens](https://scan.multichain.org/#/tokens) / [Network](https://scan.multichain.org/#/network) / [Dashboard](https://scan.multichain.org/#/dashboard) / [dApp](https://app.multichain.org/#/pool)
* Multichain - [anyCall whitepaper](https://drive.google.com/file/d/1NFFFecAjStbGMyvJVDez3xmsGSHYvNYv/view)
* Multichain - [anyCall website](https://anycall.multichain.org/)
* Multichain Gitbook - [anyCall](https://multidao.gitbook.io/anycall/) 
* Multichain Github - [Audit Reports](https://github.com/anyswap/Anyswap-Audit)
* Multichain Exploiter [1](https://etherscan.io/address/0x4986e9017ea60e7afcd10d844f85c80912c3863c), [2](https://etherscan.io/address/0x7e015972db493d9ba9a30075e397dc57b1a677da), [3](https://etherscan.io/address/0xb4f89d6a8c113b4232485568e542e646d93cfab1), [4 (Whitehat)](https://etherscan.io/address/0xfa2731d0bede684993ab1109db7ecf5bf33e8051), [5](https://etherscan.io/address/0x123edfc3795958147958d68162fd134c6a8c69e0), [6](https://etherscan.io/address/0xb5c827fdbbee6f6e9df3a5cb499aedf5927de1b8)
* Multichain [anyCall Workshop](https://www.youtube.com/watch?v=gK7QPDb1PRc) by Moralis
* TheDefiant - [Questions on Multichain Funds](https://thedefiant.io/questions-on-multichain-funds)
* LI.FI Blog - [Multichain - A Deep Dive](https://li.fi/knowledge-hub/multichain-a-deep-dive/)
* LI.FI Blog - [Navigating Arbitrary Messaging Bridges](https://blog.li.fi/navigating-arbitrary-messaging-bridges-a-comparison-framework-8720f302e2aa)
* LI.FI Podcast - [The Crypto Mustard (Multichain)](https://www.youtube.com/watch?v=bmZgG0VDBPc)
* Cointelegraph - [Tool to call Contracts](https://cointelegraph.com/news/leader-in-cross-chain-releases-tool-to-easily-call-contracts-between-any-network)
* Publish0x - [MultiChain Deep Dive](https://www.publish0x.com/lifi/multichain-a-deep-dive-all-you-need-to-know-about-multichain-xelworv)
* Etherscan - Multichain [Deployer 1](https://etherscan.io/address/0x4464fc279180045b1f57beacfa0d82e9cd4235cd), [Deployer 2](https://etherscan.io/address/0x49addc76569402cf893c015ac69a177aaf4e822b), [Deployer 3](https://etherscan.io/address/0x8132d337977f8d145a7b9cfac393df123c0f0e43), [Deployer 4](https://etherscan.io/address/0xfa9da51631268a30ec3ddd1ccbf46c65fad99251)
* Etherscan - [anyCall v6 proxy](https://etherscan.io/address/0xc10ef9f491c9b59f936957026020c321651ac078#contracts) / [Contract Reader](https://www.contractreader.io/contract/0xC10Ef9F491C9B59f936957026020C321651ac078)
* Etherscan - [Root Liquidity Gauge factory](https://etherscan.io/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#_blank)
* Etherscan - [EOA bridge wrapper](https://etherscan.io/address/0xd754492dc12fdeA93C49E88C0437aCe2e7Cd1C14#code)
* Etherscan - [CRV minter smart contract](https://etherscan.io/address/0xd061D61a4d941c39E5453435B6345Dc261C2fcE0#code)
* Etherscan - [Staking rewards smart contract](https://etherscan.io/address/0x99ac10631f69c753ddb595d074422a0922d9056b#code) (Synthetix fork)
* Etherscan - [Gauge controller contract](https://etherscan.io/address/0x2F50D538606Fa9EDD2B11E2446BEb18C9D5846bB#code)
* Curve Medium - [Curve anyCall](https://medium.com/multichainorg/curve-x-anycall-cross-chain-gauges-e657c1c6e5b5)
* Curve Forum - [Multichain Gauges](https://gov.curve.fi/t/cip-62-63-multi-chain-gauges-to-spread-the-crv-love-on-side-chains-and-l2s/1747) (CIP 62/ CIP 63)
* Curve Github - [Cross-chain gauges factory](https://github.com/curvefi/curve-xchain-factory)
* Curve Docs - [Gauges for EVM Sidechains](https://curve.readthedocs.io/dao-gauges-sidechain.html#)
* Child Gauge Factory - [Fantom](https://ftmscan.com/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#code), [Polygon](https://polygonscan.com/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#code), [Optimism](https://optimistic.etherscan.io/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#code), [Arbitrum](https://arbiscan.io/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#code), [Avalanche](https://snowtrace.io/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#code), [Gnosis](https://gnosisscan.io/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5#code), [Moonbeam](https://moonscan.io/address/0xabc000d88f23bb45525e447528dbf656a9d55bf5)
* Twitter -[ Barket.eth](https://twitter.com/bkiepuszewski/status/1572537869536989184)
