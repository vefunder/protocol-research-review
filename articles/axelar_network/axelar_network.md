# Risk Assessment: Axelar 

### Useful Links

#### Basic information:
* Website: https://axelar.network/
* Twitter: https://twitter.com/axelarcore
* Discord: https://discord.com/invite/aRZ3Ra6f7D
* Block Explorer: http://axelarscan.io
* Dev Docs: http://docs.axelar.dev
* GitHub: https://github.com/axelarnetwork
* Audits: https://github.com/axelarnetwork/audits
* Bug Bounty: [Axelar Network Bug Bounties | Immunefi](https://immunefi.com/bounty/axelarnetwork/)
#### Learing materials:
* Introductory materials: [Article](https://medium.com/axelar/a-technical-introduction-to-the-axelar-network-3c4bf9fe4dc3) | [Video](https://www.youtube.com/watch?v=0-Q1mP2vmGE ) 
* Whitepaper: https://axelar.network/axelar_whitepaper.pdf 
* Comparision of AMBs: https://blog.li.fi/navigating-arbitrary-messaging-bridges-a-comparison-framework-8720f302e2aa
#### Project specific:
* axlUSDC Contracts: [Assets | Axelarscan](https://axelarscan.io/assets)
* Satellite (Token Bridge): https://satellite.money/
#### Metrics & Data 
* Validator Security overview: https://observatory.zone/axelar/validators
* Metrics: [Metrica](https://https://app.metrika.co/axelar/dashboard/network-overview?tr=1d) | [Dune](https://dune.com/axelarnetwork/axelar)
* Research Notes: https://docs.google.com/spreadsheets/d/1VaNHKrsYGoQ0a2hU41gfr105k1UpZ0M7-SO1qsURmF8/edit?usp=sharing


### Relation to Curve

Axelar has put forward two successful proposals to implement CRV rewards for gauges, each designated for a specific pool of axlUSDC tokens:

1. The [first proposal](https://gov.curve.fi/t/proposal-to-add-axlusdc-usdc-to-the-gauge-controller/8694), executed February 2023, was for the axlUSDC<>USDC pools on multiple chains, including Polygon, Avalanche, and Fantom, which serve as primary liquidity venues for axlUSDC on the Axelar network. This proposal sought approval for a gauge to incentivize USDC and axlUSDC liquidity on Curve, leveraging the already high trading volumes and native yields of axlUSDC pools.
2. The [second proposal](https://gov.curve.fi/t/proposal-to-add-axlusdc-fraxbp-to-the-gauge-controller/8907), executed March 2023, is for the axlUSDC<>FRAXBP pool on Arbitrum, which serves as the primary liquidity venue for axlUSDC on the Axelar network. This proposal seeks approval for a gauge to incentivize USDC, FRAX, and axlUSDC liquidity on Curve, with the support of the FRAX team.



## Introduction to Axelar
Axelar is a decentralized blockchain network built with Cosmos SDK that uses proof-of-stake. Its infrastructure provides secure cross-chain communication, allowing users to interact with any asset or application on any blockchain. Axelar's architecture offers uniform translation, routing, and governance structure, providing developers with the ability to build on the best platform for their use case while accessing users, assets, and applications in every other ecosystem. Unlike traditional pairwise cross-chain bridges, Axelar securely processes cross-chain messages using a permissionless set of validators which constantly produce blocks by voting on the legitimacy of all messages. Additionally, the Axelar SDKs offer a comprehensive suite of tools and APIs for developing Web3 applications, abstracting away networking and ecosystem-specific deployment considerations.

### Founding Team
The team (according to Linkedin) now has over [50 employees working on Axelar](https://www.linkedin.com/company/axelarnetwork/) and is considered the brain child of Sergey Gorbunov and Georgios Vlachos. 


[Sergey Gorbunov](https://www.linkedin.com/in/sergegorbunov/details/experience/) is a co-founder of Axelar Network and an assistant professor at the University of Waterloo, where he conducts research on cryptography and blockchain technology. Prior to founding Axelar, Sergey served as the Chief Cryptographer at Algorand and founded stealthmine, a blockchain-based data storage and sharing platform. He has also worked as a research assistant at MIT, a course instructor at the University of Toronto, and a crypto researcher at IBM. Sergey has a strong academic background in computer science and extensive experience in the blockchain and cryptography industries.

[Georgios Vlachos](https://www.linkedin.com/in/georgiosvlachos/) is a co-founder of Axelar Network. Prior to founding Axelar, he served as the Head of Mathematics at Algorand, a leading blockchain platform. Georgios holds a Master's degree in Computer Science from MIT where he conducted research on machine learning and data mining. Georgios has a strong background in mathematics and computer science, with a particular focus on algorithms and cryptography. 



### Brief Project History 

Several significant events have shaped Axelar's trajectory and contributed to its success:

* **06/2020**: [Axelar Inc.](https://opencorporates.com/companies/ca/12136040) was incorpoated in Canada by Sergey Gorbunov and Georgios Vlachos.
* **11/2020**: [$3.75M Raise](https://www.coindesk.com/tech/2020/11/13/algorand-linked-axelar-raises-375m-in-seed-funding-to-help-blockchains-communicate/) in a Seed Round from 11 investors, the funding round included participation from Binance X, DCVC (Data Collective), Lemniscap, Divergence Ventures, Waikit Lau, and Naval Ravikant.
* **06/2021**: Axelar raised [$25 million in a Series A funding round](https://www.coindesk.com/markets/2021/07/15/axelar-raises-25m-in-series-a-fundraising-led-by-polychain-capital) led by Polychain Capital, with participation from Blockchain Capital, Divergence Ventures, and others.
* **11/2021**: Axelar Network raised an [undisclosed amount of funding in a stratgic round](https://axelar.network/blog/axelar-announces-strategic-investment-from-crypto-com-capital) from 6 investors in a Venture Round, with Crypto.com Capital as the lead investor.
* **02/2022**: Axelar released its cross-chain communication protocol suite, tools, and APIs to the public. It also raised [$35 million in a Series B funding](https://www.coindesk.com/business/2022/02/15/blockchain-interoperability-network-axelar-raises-25m-at-1b-valuation/) round led by Andreessen Horowitz, with participation from Sequoia Capital, Ribbit Capital, and others. The funding round values the company at $1 billion, making it a unicorn.
* **03/2023**: Launch of [Axelar Virtual Machine](https://www.coindesk.com/tech/2023/02/27/axelar-introduces-virtual-machine-for-developers-building-cross-chain-crypto-apps/), which enables developers to build dApps once and run them on all chains, including EVM, Cairo VM, Cosmos, and other ecosystems.


### Axelar today
Since its public mainnet launch in [February 2022](https://axelar.network/blog/axelar-begins-rollout-of-public-mainnet-launch-for-february-2022-bringing-decentralized-interoperability-to-ethereum-avalanche-terra-moonbeam-and-others), Axelar has facilitated a significant number of cross-chain activities, with a total of 376,547 transactions recorded, including 349,568 asset transfers and 26,979 GMP (general message passing) calls. The total transfer token volume across connected chains amounts to $1,853,069,262. Axelar has connected 34 chains and established 170 cross-chain contracts


|![Supported Networks](https://i.imgur.com/WGWEw9G.png)|
|-------|
|<center>[Source: https://axelarscan.io/]</center>|



The total value locked in assets on the Axelar Network is currently $111.8M, with $31.3M locked in EVM and $81.2M locked in Cosmos.
|![TVL](https://i.imgur.com/ttl95tH.png)|
|-------|
|<center>[Source: https://axelarscan.io/tvl]</center>|


### Exploring Axelar Protocol in more depth: A practical exploration 

A high-level understanding of Axelar is necessary to conceptualise the risk to Curve LPs. To understand [Axelar and the respective protocol components](https://axelar.network/blog/an-introduction-to-the-axelar-network), this section will go over a cross-chain general [message passing request](https://docs.axelar.dev/learn/network/flow) initiated by a dApp or a user.

|![image alt](https://i.imgur.com/QcMgjtP.jpg "Axelar Technology Stack")|
|-------|
|[Source: https://axelar.network/blog/an-introduction-to-the-axelar-network]|

**Step 1 - Message request origination:** A dApp user initiates a request through the Axelar API on any connected chain. The API allows users to send arbitrary data, including token transfers and smart contract calls, to a specified destination chain.

**Step 2 - Gateways:** Gateways are deployed on each connected chain as either a smart contract (EVM chains) or an application with logic and ability to communicate with the Axelar network (Cosmos/non-EVM chains). The Gateway receives messages from the dApp and sends them into the Axelar network for routing to any connected chain. The Gateway is controlled by a key held jointly by all Axelar validators, using a multiparty cryptography scheme. Each validator holds multiple key shares based on the amount of AXL tokens staked. Gateways have two main functions: (1) enabling cross-chain message requests on the source chain, (2) executing messages on the destination chain.

**Step 3 - Relayers:** When a Gateway receives a message, it generates an event, which is picked up by a relayer and submitted for processing. Axelar employs relayers to monitor events produced by Gateways on all connected chains. A free relayer service is operated by Axelar, but anyone can permissionlessly participate as a relayer for the network.

**Step 4 - Validators:** Validators are the Axelar block producers that participate in proof-of-stake consensus and verify the truthfulness of cross-chain events being submitted. Each validator runs a node for the source chain where the event originated and verifies the event by querying their RPC endpoint. Validators are incentivized to run nodes for as many chains as possible, with rewards based on the number of chains they support. The event is validated by the Axelar network, processed by the consensus protocol, and recorded in a block. The cross-chain message is then routed into a queue for the destination chain and ready to be sent to the destination Gateway. 

**Step 5 - Submitting a message to the destination chain:** Axelar reaches consensus on a transaction when a set threshold of Validator key shares have signed. Once authorized, anyone can submit the signed message to the destination chain for processing. Another set of relayer services monitor outgoing transaction queues for approved and signed cross-chain messages, and periodically submits them to external chains. The Gateway on the destination chain receives the approved message and stores the contract call approval with a hash of the payload, which can now be executed at any time. (Again, Axelar provides these relayer services for free, but users can create and use their own relayer services instead.)

**A Note on Gas and executor services:** Axelar offers Gas and Executor services to make executing cross-chain messages easier. The Gas Receiver smart contract allows users to pay for all transaction fees in a one-time payment on the source chain in the native token. The Gas Receiver estimates the total gas cost and converts tokens into AXL, destination-chain tokens, and other required currencies.


## Assets

### Native token: AXL
AXL is the native token of Axelar Network. It can be classified primarily as a [utility and governance token](https://medium.com/@axelar-foundation/an-overview-of-axl-token-economics-4dc701c9054d) and serves four primary functions: 
1. Medium for transaction fees and any other fees for network usage paid by users to the validators
2. Governance over proposals such as a parameter change or protocol upgrade.
3. Delegation and validator rewards to secure the network.
4. Reward ecosystem contributors who further the development and growth of the Axelar network.


### axlUSDC
Axelar's [axlUSDC is a wrapped representation of USDC](https://axelar.network/blog/what-is-axlusdc-and-how-do-you-get-it) bridged from Ethereum to multiple chains supported by Axelar. It allows users to transact USDC across a multitude of ecosystems. USDC is a dollar-pegged stablecoin issued by Circle, a US-based company, and is mainly used on the Ethereum blockchain. However, the axlUSDC allows it to be used on multiple blockchain networks.

Axelar's cross-chain bridges generate axlUSDC by accepting a deposit of USDC at an Axelar Gateway on the Ethereum network and then minting an equivalent amount of axlUSDC on the destination chain. Every unit of axlUSDC represents a unit of USDC that is locked in an Axelar Gateway on the Ethereum network. In essence, Axelar's dynamic validator set, numbering 70 at the time of writing, secures axlUSDC.

Users can acquire axlUSDC in three ways. First, they can swap it via [liquid pairs on DEXs](https://axelar.network/liquidity-pools) that list pairs in axlUSDC, supported by liquidity pools. Second, they can swap it via [Squid](https://app.squidrouter.com/), a cross-chain liquidity router built on Axelar, which provides liquid cross-chain swaps using axlUSDC as a routing asset. Finally, they can mint it via [Satellite](https://satellite.money/), a cross-chain bridge built by Axelar.

### Composable USDC
Circle recently announced a [Cross-Chain Transfer Protocol](https://www.circle.com/en/pressroom/circle-enables-usdc-interoperability-for-developers-with-the-launch-of-cross-chain-transfer-protocol) (CCTP), slated for release Q1 2023 on Ethereum and Avalanche, and later available on Solana and other chains. Axelar will be a partner protocol, enabling [composable USDC](https://www.circle.com/blog/composable-usdc-seamless-multichain-ux-by-axelar) with its General Message Passing (GMP) capability. This allows arbitrary data to accompany a token transfer to enable usecases such as seamless cross-chain swaps, one-click deposits/withdrawals into application-specific blockchains, and cross-chain NFTs. 

#### A Note on USDC
Between 11.03 to 14.03.2023 [USDC depegged below $1](https://twitter.com/DefiIgnas/status/1635980042294669312) due to the failure of Silicon Valley Bank (SVB), which held $3.3 billion in reserves backing the stablecoin's value. The bank's collapse was due to a bank run amid concerns over its financial health. Coinbase and Binance's decision to stop USDC conversions and the rapid depletion of the Curve 3pool contributed to USDC's depeg. The U.S. government took emergency action to [protect all SVB bank depositors](https://www.npr.org/2023/03/13/1163028329/biden-administration-steps-in-to-save-customers-of-silicon-valley-bank), thereby alleviating panic and ultimately bringing USDC back to its peg.

|![USDC depeg](https://i.imgur.com/LGcEvXI.png)|
|-------|
|[Source: https://coinmarketcap.com/currencies/usd-coin/]|

The depeg raised concerns over the trustworthiness of USDC and the risk of contagion from centralized finance (CeFi) on crypto. As a wrapped representation of USDC, axlUSDC is exposed to the risk of the underlying asset in addition to the risks associated with Axelar network itself. 


## Axelar Ecosystem Health Metrics

Consensys [wrote a research piece](https://consensys.net/research/measuring-blockchain-decentralization/) attempting to quantify decentralisation among various layer 1 ecosystems. Its analysis for Axelar provides some useful metrics.


### Block production

|![Block Production](https://i.imgur.com/gt4N24J.png)|
|-------|
|[Source: https://app.metrika.co/axelar/dashboard/network-overview?tr=1M]

This chart displays block production trends on the Axelar Network over the past 30 days. The blue bars represent the number of blocks generated per hour. The network has maintained a stable block production rate of approximately 600 blocks per hour. A decrease in this rate may indicate network congestion. The purple line shows the maximum time it took to produce a block in a given hour, which varied between 6 and 22 seconds during the observed period. The green line represents the average block production time, which remained stable at 6 seconds, in line with the protocol specifications.

Based on the stable block production rate and relatively consistent average block production time between the observed period, it appears that the consensus on the Axelar Network is stable and operating within the expected parameters.


### Function Call Diversity 

|![](https://i.imgur.com/gVSCVVG.png)|
|-------|
|[Source: https://app.metrika.co/axelar/dashboard/network-overview?tr=1M]|

The function calls of the events occurring in cross-chain transfers on Axelar can be defined as follows:  

* **Link:** The process of linking an Axelar address with an Ethereum Virtual Machine (EVM) address, allowing for the transfer of assets between the two chains.
* **SignCommands:** The act of signing a command to initiate a cross-chain transfer, verifying the authenticity of the transfer and ensuring that it is authorized.
* **SubmitSignatureRequest:** The process of submitting a request to initiate a cross-chain transfer, including the signed command and other necessary information.
* **ConfirmDeposit:** The confirmation of a deposit on the source chain, indicating that the assets have been transferred to the Axelar Network and are available for transfer to the target chain.
* **CreatePendingTransfers:** The creation of a pending transfer on the target chain, including the necessary information to complete the transfer.
* **ConfirmERC20Deposit:** The confirmation of a deposit of ERC20 tokens on the source chain, indicating that the tokens are available for transfer to the target chain.
* **ExecutePendingTransfers:** The execution of the pending transfer on the target chain, completing the cross-chain transfer and transferring the assets to the recipient.
* **RegisterChainMaintainer:** The registration of a new chain maintainer, is responsible for maintaining and monitoring the cross-chain transfers on a specific chain, ensuring that they are secure and efficient.

The data above shows the count of different event types occurring in cross-chain transfers on the Axelar Network for the past 30 days. The SubmitSignatureRequest event type is the most frequent, occurring close to 1 million times. The Link event type occurred 32,185 times, while SignCommands occurred 13,260 times, suggesting that a significant number of users are linking their Axelar addresses with EVM addresses and signing commands to initiate cross-chain transfers. The ConfirmERC20Deposit and ExecutePendingTransfers event types occurred less frequently, suggesting that ERC20 token transfers and the execution of pending transfers are less common than other types of events. Finally, the RegisterChainMaintainer event type occurred only 109 times.


[Function call diversity](https://consensys.net/research/measuring-blockchain-decentralization/) refers to the number of different functions that are being called within a software system or application. In the context of an arbitrary message bridge like Axelar, function call diversity is important because it can suggest insights on system robustness, maintainability, and extensibility. There are a range of different event types occurring in cross-chain transfers on the Axelar Network. Overall, the diversity of event types suggests that the platform is versatile and can accommodate a range of different use cases and applications.


### Mining/Staking Diversity & Growth 

|![Overview of Number of Proposed Blocks by Validator](https://i.imgur.com/F6qOeJd.png)|
|------|
|[Source: https://app.metrika.co/axelar/dashboard/network-overview?tr=1M]| 

The chart above displays the number of blocks proposed by validators on an hourly basis over a one-month period. The chart can be useful in identifying issues related to block production by an individual validator, but it may also provide insight into network-wide issues. For example, a rapid color shift from deep purple to grey would suggest that the validator is now producing significantly fewer blocks over a selected time interval. Censorship of a region or client issues may be apparent if many validators stop producing blocks. 

Overall, the chart shows good network stability (reasonably consistent validator performance) over the one-month period with a healthy propogation of blocks.


### Client Diversity

Client diversity refers to [the variety of blockchain clients](https://consensys.net/research/measuring-blockchain-decentralization/) that are supported by a particular blockchain network. It is important for ensuring decentralization and security in the network, as a diverse range of clients prevents any single client or group of clients from having too much control or influence over the network. In the case of Axelar, it supports a variety of clients including Ethereum, Binance Smart Chain, and Polkadot, among others, increases its ability to connect different blockchain platforms and promote interoperability, making it more accessible and versatile for users.


#### Validator Nodes 
|![Geographical Node Distribution](https://i.imgur.com/ZVsOWhv.png)|
|------|
|[Source: https://observatory.zone/axelar/validators]|

The Axelar validator set has a total of 90 nodes distributed across different regions. The Americas region has 22 nodes with a stake of 25.72%, while the Europe region has 57 nodes with a stake of 41.90%. The Asia Pacific region has 3 nodes with a stake of 2.24%, and there are 8 nodes with no specified region, accounting for a stake of 30.14%.

Based on [the data provided](https://observatory.zone/axelar/countries), it appears that Europe has the highest concentration of Axelar validators in terms of both nodes and stake, with 57 nodes and a stake of 41.90%. The Americas also have a significant presence with 22 nodes and a stake of 25.72%. Meanwhile, Asia Pacific only has three nodes and a stake of 2.24%, indicating a lower level of participation in the network from that region. This potentially suggests geographical centralisation.

It is important to note that this data only represents the validators that are currently connected and may not reflect the full distribution of validators over time. Additionally, this data does not necessarily provide information on the specific geographic location of the validators within each region, but rather through which country nodes access the network which can be altered by using VPN or routing through a private server.

#### Internet Service Providers (ISPs)
The [top three Internet Service Providers (ISPs)](https://observatory.zone/axelar/isps) in terms of stake and number of nodes for Axelar are Hetzner Online GmbH, OVH SAS, and Amazon.com, Inc. Hetzner Online GmbH has the highest stake with 19.94% and 25 nodes, followed by OVH SAS with 25.90% and 16 nodes, and Amazon.com, Inc. with 10.03% and 11 nodes.

The concentration of stake and nodes in these top three ISPs represents a centralising force and should be observed with caution. If a large percentage of the network's nodes are controlled by a small number of ISPs, there is a risk of centralization and potential vulnerabilities. For example, if a majority of the network's nodes are hosted by one or two ISPs, those ISPs could potentially collude to manipulate the network's operations. However, it's important to note that the distribution of nodes and stake across different ISPs can change over time, and Axelar is still a relatively young project. In addition, ISP concentration is not meaningfully deviant from the industry standard.

##### A Note on Collusion 
The collusion threshold is a key security feature of the Axelar Network that helps to prevent validators from colluding and compromising the integrity of the network. The threshold is determined by the number of validators required to sign off on a block and is set at two-thirds of the total number of validators weighted by stake. 

As of [September 2022 with Meave Upgrade](https://axelar.network/blog/axelar-implements-quadratic-voting-with-maeve-upgrade), the Axelar Network has introduced [quadratic voting](https://vitalik.ca/general/2019/12/07/quadratic.html), which further enhances the security and decentralization of the network. Currently, the network is secured by 70 active validators and the collusion threshold is set at 2/3 of validators quadratically weighted by stake. At the time of writing a successful collusion would require 35 validators to collude to gain a supermajority. The metrics presented should be evaluated in context of this.

#### Ecosystem

An ecosystem refers to the network of interconnected actors and entities that participate in the network and help to create and maintain value within the network.

##### Network Usage 
|![Network Usage](https://i.imgur.com/M3xHZnc.png)|
|----|
|[Source: https://axelarscan.io/transfers]|

When analyzing the daily transfer and volume statistics of the Axelar Network from January 15, 2022, to February 28, 2023, the impact of Terra Luna becomes quite apparent. The statistics show that the daily transfer and volume on Axelar are now relatively lower by multitudes, which highlights the setback caused by Terra. However, despite this setback, Axelar has demonstrated resilience and stable growth in terms of daily transfer. This growth is particularly noteworthy considering the timing of the market cycle and is a positive sign for the network's future growth and adoption. The setback caused by Terra Luna is also visible in the top path in terms of volume and transfer during this timeframe, which are still dominated by the Luna Terra pairs. 


Statistics on the [General Message Passing](https://axelarscan.io/gmp/stats?fromTime=1679309728011) show that after May 2022 (i.e. after the collapse of Luna Terra), the top chain pairs on the Axelar Network are mainly associated with Polygon, followed by BNB Chain, Moonbeam, Fantom, and Avalanche. While Polygon is the most popular, other chains also have a significant presence, as can be seen by the diverse range of destination contracts on the network. The most popular destinations are Polygon, BNB Chain, and Moonbeam, accounting for 21.64%, 16.37%, and 14.04% of the total messages, respectively. These statistics suggest the diversity and versatility of the Axelar Network, with users utilizing a range of chains and contracts for their cross-chain transfers. It will be interesting to track any shifts in the distribution of messages and destination contracts in the future.

|![GENERAL MESSAGE PASSING](https://i.imgur.com/nZkq3nb.png)|
|-------|
|*[Source: https://axelarscan.io/gmp/stats]*|

Another indicator suggesting the growth of the network is the Growth of Daily Active Addresses (DAA), which have steadily been growing. Daily active addresses is a common metric used to measure the number of unique addresses that were active on a blockchain network during a given day. 

|![DAA](https://i.imgur.com/ymI48qF.png)|
|---|
|[Source: https://app.artemis.xyz/dashboard/axelar]|


##### Developer Activity

Weekly [commit and weekly active developer](https://app.artemis.xyz/developers/Axelar%20Network?includeForks=false) are metrics used to measure the productivity and activity of open-source repositories. Weekly commit refers to the number of commits made by developers in a given week. A commit is the smallest unit of work and can vary in size, but is generally a good indicator of developer productivity. Meanwhile, the number of active developers in a week refers to the number of developers who have made at least one commit during that time period. This metric is useful in tracking the level of developer activity and engagement with the project. Both of these metrics are important in understanding the health and progress of open-source projects, as they provide insight into the level of developer involvement and output.

|![Developer Activity](https://i.imgur.com/4Zw6Z1k.png)|
|--------|
|[Source: https://app.artemis.xyz/developers/Axelar%20Network?includeForks=false]|

Axelar has a total of 56 repositories. The weekly commits have decreased by 61.7% in the last month and 59.1% in the last 3 months. The number of weekly active developers has decreased by 30.8% in the last month and 47.1% in the last 3 months. For Squid, representing the "Sub-Ecosystems" in the Figure above, there are 6 repositories and only 2 weekly commits, which represents an 86.7% decrease in the last month and a 33.3% decrease in the last 3 months. The number of weekly active developers has decreased by 75.0% in the last month and 50.0% in the last 3 months.

While the decline seems extreme this is rather in line with other ecosystems which saw similar percentage changes of drawdowns due to the cyclical nature of the crypto markets.  


### Tokenbased Risks

Token-based risk refers to the potential for loss or negative impact on a blockchain network's native token or other tokens within its ecosystem. This type of risk is inherent in any decentralized system that relies on token economics, where tokens serve as a means of value transfer, governance, and incentivization. This section will explore some aspects of token-based risk in Axelar.

#### Initial Token Distribution of $AXL
The initial token distribution of a token-based project tends to have a degree of stickiness to it. This is particularly true for proof-of-stake systems, where initial token holders have no incentive to redistribute their tokens and promote decentralization since their wealth and staking power are directly correlated.

In the case of Axelar, we can see that approximately 59% of the initial token distribution went towards insiders of the project, such as the company operations, team, and backers, while the remaining 41% was allocated to the community through community programs and sale. This is illustrated in Figure 2, which shows the AXL token allocations at genesis.


|![image alt](https://i.imgur.com/YbLzetz.png "AXL token allocations at genesis")| 
| -------- | 
| <center>*[Fig 2:  AXL token allocations at genesis](https://medium.com/@axelar-foundation/an-overview-of-axl-token-economics-4dc701c9054d)*</center>| 


If we look at the [distribution of bonded validators at the time of the analysis](https://docs.google.com/spreadsheets/d/1VaNHKrsYGoQ0a2hU41gfr105k1UpZ0M7-SO1qsURmF8/edit?usp=sharing), we can see that the total stake ranges from 7.6 million AXEL tokens to 45.7 million AXEL tokens. The voting power of a validator ranges from 1.02% to 6.43%, while the quadratic voting power ranges from 1.32% to 3.32%. The total delegation of validators ranges from 7 to 1,492.

![](https://i.imgur.com/RM3H6Fd.png)

From the data observed, it appears that the AXEL tokens are relatively well distributed among validators, with no single validator holding an excessively large amount of tokens and therefore voting power. As decentralisation is a property for censorship resistance each LP should evaluate for themselves if they believe this is sufficient decentralization in the context of Axelar use cases. 

In addition, progressive decentralization gains believability in the context of Axelar as the Validator concentration improved since the past review by other sources such as this [comparison of Arbitrary messaging bridges by Lifi](https://blog.li.fi/navigating-arbitrary-messaging-bridges-a-comparison-framework-8720f302e2aa).

However, the author of this report cannot conclusively comment on the progression of the current total distribution, as it is difficult to monitor the progression of the token distribution due to the presence on multiple chains and forms (e.g. AXL on Axelar Native Chain, wAXL on multiple external chains).

### Technical Security Risk 

Technical security risk is a critical concern for any blockchain network as it involves potential vulnerabilities in the system's technical infrastructure. In this section, we will examine technical security risks associated with the Axelar network and their potential impact.

#### Contract Architecture Overview
Axelar provides the main smart contracts for EVM [here](https://github.com/axelarnetwork/axelar-cgp-solidity).

**Interfaces:**
a. IAxelarGateway.sol
b. IERC20.sol
c. IERC20BurnFrom.sol
d. IAxelarExecutable.sol

**Base Contracts:**
a. Ownable.sol
b. EternalStorage.sol
c. AdminMultisigBase.sol
d. ERC20.sol

**ERC20 Extensions:**
a. ERC20Permit.sol
b. MintableCappedERC20.sol
c. BurnableMintableCappedERC20.sol

**Axelar Core Contracts:**
a. AxelarGatewayProxy.sol
b. AxelarGateway.sol
c. AxelarAuthWeighted.sol

**Utility Contracts:**
a. ECDSA.sol
b. TokenDeployer.sol
c. DepositHandler.sol
d. AxelarDepositService.sol
e. AxelarGasService.sol

##### Arichtectual Overview description 
- IAxelarGateway.sol, IERC20.sol, IERC20BurnFrom.sol, and IAxelarExecutable.sol are interfaces that define the required functions for the respective contracts implementing them.
- Ownable.sol provides ownership functionality to contracts that inherit from it.
- EternalStorage.sol serves as a storage contract for the proxy.
- AdminMultisigBase.sol is a multisig governance contract used for upgrading implementations through voting.
- ERC20.sol is the base ERC20 contract that other ERC20 extension contracts inherit from.
- ERC20Permit.sol, MintableCappedERC20.sol, and BurnableMintableCappedERC20.sol are extensions of the base ERC20 contract, providing additional functionalities like spending permits, minting with a capped total supply, and burn capabilities.
- [AxelarGatewayProxy.sol](https://etherscan.io/address/0x4F4495243837681061C4743b74B3eEdf548D56A5#code) and [AxelarGateway.sol (Implementation Contract)](https://etherscan.io/address/0xEd9938294aCF9EE52D097133CA2cAafF0C804F16#code) are the core contracts of the Axelar network, with the former implementing the proxy pattern and the latter being the implementation contract that accepts signed commands from Axelar validators.
- [AxelarAuthWeighted.sol](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code) is a weighted multisig authentication contract used by the gateway for verifying messages.
- ECDSA.sol is a utility contract for signature authentication checks.
- [TokenDeployer.sol](https://etherscan.io/address/0xe88Ab68Cd69e92294FcC3BBBD894Fb183197fA39#code) is responsible for deploying BurnableMintableCappedERC20 tokens upon receiving a signed command from the Axelar network.
- DepositHandler.sol is a contract deployed at deposit addresses, enabling the burning/locking of tokens sent by users. It prevents re-entrancy and works in conjunction with the gateway contract.
- AxelarDepositService.sol is a service used to generate deposit addresses for ERC20 tokens, native currency transfers, or unwrapping native currency from wrapped ERC20 tokens.
- [AxelarGasService.sol](https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#code) is a contract that handles cross-chain gas payments. It accepts payments for covering gas costs on the destination chain. Gas payments should occur with the same parameters right before calling callContract or callContractWithToken on the gateway. 


#### Smart Contract Ownership 
Smart Contract Ownership refers to the control over the smart contract code and its execution. In this section, we will discuss the risks associated with smart contract ownership and how they can be mitigated to ensure the security of the network. 

##### AxelarGatewayProxyMultisig.sol
AxelarGatewayProxyMultisig.sol is the central contract of the Axelar network and is owned by a multisignature scheme that is implemented in AdminMultisigBase.sol. This scheme allows multiple addresses to collectively own and control the contract. The contract coordinates various library contracts such as AxelarGateway.sol, AxelarGatewayProxy.sol, and AxelarAuthWeighted.sol. Additionally, it integrates with other library contracts to provide additional functionalities, such as cross-chain gas payments, token deployment, and signature authentication checks.

At present (i.e. epoch 3) the [admin threshold is set to 4](https://etherscan.io/address/0x4F4495243837681061C4743b74B3eEdf548D56A5#readProxyContract#F2) out of 8 addresses. The owner addresses are listed below: 

    
| AxelarGatewayProxyMultisig.sol Owners |
|:--------------------------------------:|
| 0x3f5876a2b06E54949aB106651Ab6694d0289b2b4 |
| 0x9256Fd872118ed3a97754B0fB42c15015d17E0CC |
| 0x5C8EF9ca7b43c93Ac4a146BeF77FAFbc7D3e69B7 |
| 0x1486157d505C7F7E546aD00E3E2Eee25BF665C9b |
| 0x2eC991B5c0B742AbD9d2ea31fe6c14a85e91C821 |
| 0xf505462A29E36E26f25Ef0175Ca1eCBa09CC118f |
| 0x027c1882B975E2cd771AE068b0389FA38B9dda73 |
| 0x30932Ac1f0477Fbd63E4c5Be1928f367A58A45A1 |


In practical terms, this means that the upgrading of contracts is subject to 4 out of 8 Multisig signers. Axelar plans to take further steps in decentralizing the network by having the validator set jointly approve smart contract upgrades. This is a crucial step in achieving a higher level of decentralization and eliminating the need for a governed multisig. The result will be a more decentralized network, offering greater transparency and security to users. Ultimately, Axelar aims to create a fully autonomous network that operates without any central authority or governing body.

##### AxelarAuthWeighted.sol

[AxelarAuthWeighted.sol](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code) is a core contract used by the Axelar network's gateway for verifying messages. It is a weighted multisig authentication contract that allows for multiple signers with different weights to approve a message. The contract is owned by AxelarGatewayProxyMultisig.sol, which is set during contract deployment.

|![AxelarAuthWeighted.sol](https://i.imgur.com/cucphk1.png)|
|---|
|[Source:https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#readContract#F4]|

##### TokenDeployer.sol

TokenDeployer.sol is a core contract of the Axelar network responsible for deploying BurnableMintableCappedERC20 tokens. The contract is owned by the Axelar Token Deployer, which is set as the initial owner in the constructor function of the Ownable contract. 
|![TokenDeployer.sol](https://i.imgur.com/LC3di4t.png)|
|--|
|[Source: https://etherscan.io/address/0xe88Ab68Cd69e92294FcC3BBBD894Fb183197fA39#code]

The onlyOwner modifier ensures that only the contract owner can call certain functions, and the transferOwnership function allows the owner to transfer ownership to another address. Based on the events, it appears that ownership of the contract has not been transferred and still resides with the [Axelar Deployer](https://etherscan.io/address/0xA57ADCE1d2fE72949E4308867D894CD7E7DE0ef2).

|![Events](https://i.imgur.com/5mR1rzZ.png)|
|--|
|[Source: https://etherscan.io/address/0xe88Ab68Cd69e92294FcC3BBBD894Fb183197fA39#events]|


##### AxelarGasService.sol

AxelarGasService.sol is a core contract in the Axelar network that handles cross-chain gas payments. It is [owned by an EOA](https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#readProxyContract#F4). In addition, AxelarGasService.sol uses an [EOA as the gasCollector](https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#readProxyContract#F2), which is responsible for collecting cross-chain gas fees from users and distributing them to validators who provide gas on the other side of the chain. 

|<center>![AxelarGasService.sol](https://i.imgur.com/U64iCbP.png)</center>|
|---|
|[Source: https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#readProxyContract]|


#### Smart Contract Audit Review 

Axelar protocol's smart contracts have been deployed [to multiple testnets for various networks](https://docs.axelar.dev/dev/reference/testnet-contract-addresses), which indicates a commitment to testing and validation. Moreover, the Axelar network has [conducted several audits on its tech stack](https://github.com/axelarnetwork/audits), including their cryptographic library, smart contracts, frontend, and backend code. The audits were conducted by various auditors, such as Cure53, Certik, Oak Security, and NCC, and covered different components of the Axelar protocol. The audits were conducted in different phases, with some being ongoing to ensure that any changes made to the code are reviewed for security. These audits provide strong evidence that the Axelar protocol is adequately audited and secure.

On a high level the findings can be summarised as: 

|<center>![Severity Findings by Auditor across Audit Reports](https://i.imgur.com/YcD88Fh.png)</center>|
|-----|
|[Source: https://docs.google.com/spreadsheets/d/1VaNHKrsYGoQ0a2hU41gfr105k1UpZ0M7-SO1qsURmF8/edit?usp=sharing]|

The rest of this section will review issues the latest audit report for each repository. Only the issues that put user funds at risk and have not been resolved or where it is unclear if it has been resolved will be listed here.



#### Repository | axelar-utils-solidity

###### Ackee Blockchain |2022-08| H1: The forecall and forecallWithToken can be called repeatedly with the same payload

The AxelarForecallable contract has been found to have a high severity issue where the forecall function can be called repeatedly with the same payload. As a result, the check preventing double execution can be bypassed, which can be performed by anyone at any time since forecall is a publicly-accessible function. This can lead to the activation of the payload repeatedly, causing undefined consequences for the system. For instance, an attacker could call the forecall function with the forecaller address set to zeroaddress and execute a specific payload repeatedly. The recommendation is to add a zero-address check for the forecaller address in both functions (forecall and forecallWithToken) to prevent the issue. It is worth noting that the team believes that double execution should not cause any critical scenario. From the audit is unclear if this issue has been resolved. Investigation of the repository has led to this contract being completely removed from the main branch, suggesting the contract is no longer needed or has been absorbed into other contracts. 

#### Repository | axelar-cgp-solidity


###### Chaintroopers |2022-08| C1.1/C1.2: No upper bound and lower bound in one operator's weight at "AxelarAuthWeighted.sol"

The AxelarAuthWeighted.sol contract has a medium severity rating due to the absence of a lower bound and higher bound for the new threshold provided for a set of operators at the "_transferOwnership" function. 

The impact of the absence of an upper bound for the provided weight for an operator, which could allow a malicious operator to take over admin privileges. The impact of the absence of a lower bound for the provided new threshold for a set of operators, which could allow a malicious operator to compromise the gateway by taking over admin privileges. It is recommended by Chaintroopers to validate that the new threshold is at least greater than the maximum weight provided for one of the operators to mitigate this risk. 


|![](https://i.imgur.com/oI7UeOp.png)|
|--|
|[Source: https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code]|

When verifiying the high-lighted code snipped in the audit against the [deployed smart contract](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code). No changes can be observed, suggesting this issue not resolved.

###### Chaintroopers |2022-08| C2.5Insecure error handling of zero addresses at "ReceiverImplementation.sol" and at "DepositReceiver.sol"

The contracts "ReceiverImplementation.sol" and "DepositReceiver.sol" contain insecure error handling for zero addresses. When the provided "refundAddress" is zero, the contracts replace it with "msg.sender". This can lead to incoming funds being transferred to a contract that may not be designed to handle them, potentially resulting in a lock-up of the incoming funds. The recommendation is to verify that the address is not the zero address and then revert the transaction if it is. The issue is marked as LOW, as the external functionalities are mainly designed to be used by the upgradable AxelarDepositService.

An [example from the DepositReceiver.sol](https://github.com/axelarnetwork/axelar-cgp-solidity/blob/main/contracts/deposit-service/DepositReceiver.sol) suggest that the issue still exist.


|![](https://i.imgur.com/sczubTp.png)|
|--|
|[Source: https://github.com/axelarnetwork/axelar-cgp-solidity/blob/main/contracts/deposit-service/DepositReceiver.sol]|

It should be noted [the contract presuably containing](https://etherscan.io/address/0xd883C8bA523253c93d97b6C7a5087a7B5ff23d79#code) the "DepositReceiver.sol" as libary contract is unverfied on etherscan, meaning an on-chain verification was not possible.



#### Repository | axelar-cgp-solidity, axelar-xc20-wrapper

###### Code4rena Contest |2022-07| M-6 : Add cancel and refund option for Transaction Recovery

The AxelarGateway.sol and AxelarGasService.sol contracts lack an option to cancel and refund transactions that have failed or are stuck, which can potentially result in the loss of user funds. The recommendation is to include a cancel option and enable users to receive gas refunds. While Axelar Network acknowledged the issue, they disagreed with the severity, asserting that it is up to cross-chain app developers to implement refund and cancel methods as they are application-specific. The Code4rena Judge rated the issue as of medium severity due to its risk to end-users and the absence of a refund mechanism. Both the sponsor and judge advised end-users to form their own opinion.

#### Repository | axelar-core, axelar-cgp-solidity, axelar-web-app

###### Cure53 |2022-04| AXE-02-016 WP1: Potential Cosmos consensus stall via panic 
This issue is a Medium severity vulnerability and involves the EndBlocker handlers of two modules, x/tss and x/reward, which can trigger panic() calls that could potentially cause the Cosmos network to slow down or even halt. Panic() calls are a mechanism in Go that halts the execution of a program when an unexpected error or state is encountered. If an attacker were to trigger these panic() calls remotely, they could exploit this vulnerability to create a Denial-of-Service scenario on the network. To mitigate this issue, it is recommended to integrate recovery logic to all Cosmos module EndBlocker handlers to prevent the chain stall.

Based on abci.go files in [x/tss](https://github.com/axelarnetwork/axelar-core/blob/main/x/tss/abci.go) and [x/reward](https://github.com/axelarnetwork/axelar-core/blob/main/x/reward/abci.go) modules in the axelar-core repository, the issue has not been fixed yet. It should be noted that this may have been addressed else where in the archtiecture. 

####  Repository | axelar-core, axelar-cgp-solidity, tofn, tofnd, axelarjs-sdk

###### Cure53 |2021-12| AXE-01-006 WP1: Insufficient in-memory protection of secret values (Info) & AXE-01-007 WP1: Leakage of secrets via crypto libraries (Low)

AXE-01-006 and AXE-01-007 are security vulnerabilities discovered in the tofnd-main and tofn repositories. AXE-01-006 concerns the insufficient in-memory protection of secret values, where the mechanism to safely store the password and other secret values in memory only ensures that the memory is zeroed when the affected variables persist out of scope, and thus may result in secrets persisting in memory and leaking to persistent storage. AXE-01-007 concerns the leakage of secrets via crypto libraries, where third-party libraries that do not diligently zeroize secrets handed over to them can lead to leakage, such as in the HMAC implementation in tofn-main/src/crypto_tools/rng.rs, where secret_recovery_key leaks via the Hmac type when the length of the key is less or equal to the output size of the hash function. It is recommended to ensure that all code, including library code, wipes secrets after usage and consider potential scenarios that could incur leakage from memory.

It seems that the problems regarding AXE-01-006 and AXE-01-007 have not been addressed, considering that the [latest commit dates back to 09-2021](https://github.com/axelarnetwork/tofnd/commits/main/src/encrypted_sled/password.rs) and the audit report, which commenced in 12-2021 ([as indicated in the audit table](https://github.com/axelarnetwork/audits)) and concluded in 01-2022 ([as stated in the report footer](https://github.com/axelarnetwork/audits/blob/main/audits/2021-12%20Cure53.pdf))


##### Cure53 |2021-12| AXE-01-009 WP2: Mnemonic compromise constitutes single point of failure (Medium)

The use of a BIP39-mnemonic to generate entropy of 256 bits is a single point of failure and constitutes a medium level risk. This is because an attacker with knowledge of the secret mnemonic would be able to regenerate all previously generated keypairs, as they are all derived from the same seed. This design issue fails to achieve forward secrecy, and compromises the security of all generated keys and associated operations. However, the severity of this issue is mitigated by the requirement for consensus among multiple nodes in the Axelar network, making it difficult for an attacker to achieve tangible harm without compromising the mnemonic of enough nodes to achieve voting power. The affected file is tofnd-main/src/multisig/keygen.rs, where the same mnemonic entropy is used to generate key pairs.

Similar to AXE-01-006 and AXE-01-007, it seems that the problems regarding AXE-01-009 have not been addressed, considering that the [latest commit dates back to the end of 11-2021](https://github.com/axelarnetwork/tofnd/commits/main/src/multisig/keygen.rs) and the audit report, which commenced in 12-2021 ([as indicated in the audit table](https://github.com/axelarnetwork/audits)) and concluded in 01-2022 ([as stated in the report footer](https://github.com/axelarnetwork/audits/blob/main/audits/2021-12%20Cure53.pdf)). This issue should be read in mind with the validator and the threshold level of 35 validators. 


#### Axelar's Bug Bounty Program

[Axelar Immunefi bounty program](https://immunefi.com/bounty/axelarnetwork/) offers rewards ranging from USD $1,000 to a maximum of USD $2,250,000 for critical vulnerabilities, with separate payouts for different threat levels and severity classifications. The program also requires proof of concept for critical blockchain and smart contract bug reports, which is standard practice for most bug bounty programs. Additionally, the program specifies the assets in scope and provides a detailed explanation of the type of vulnerabilities they are looking for, making it easier for bounty hunters to identify potential security issues. However, the program requires KYC for all bug bounty hunters submitting a report, which may be a deterrent for some researchers. Axelar Network's bug bounty program seems to offer reasonable rewards for identifying critical vulnerabilities. This adds additional confidence to the technical robustness of the Axelar Network.

#### A Note on Documentation 

[Axelar's smart contracts](https://github.com/axelarnetwork/axelar-cgp-solidity) are generally easy to find, but it can be challenging to locate all deployed contracts through the Gitbook Documentation. For example, to find the authentication module, I had to go through the AxelarGatewayProxyMultisig contract. It would be more transparent if all contracts were listed in the Gitbook documentation. Axelar has the [contracts deployments available on Github ](https://github.com/axelarnetwork/axelar-cgp-solidity/blob/3e7d6751a3016e4694b1ce643f11530442731e09/info/mainnet.json). Overall, the code documentation is comprehensive and includes a whitepaper, a flow chart within the whitepaper that documents the software architecture, and well-documented source code available on GitHub. Additionally, it is possible to trace the documented software to its implementation in the protocol's source code and development history (e.g. versioning, commits) available.

#### Broader Techical Risk of Arbitary Messaging Bridges (AMBs)

Axelar may experience [liveness issues if validators opt to support only certain chains](https://blog.li.fi/navigating-arbitrary-messaging-bridges-a-comparison-framework-8720f302e2aa). For a new chain to be incorporated, it must receive support from 60% of the validators, based on their quadratic voting power, to run a node for that particular chain. Despite the fact that validators have the option to maintain a specific EVM chain, the majority vote threshold remains at 60% of the quadratic voting power of the total validator set. Therefore, if an EVM chain fails to attract enough supporting validators, it may only affect liveness, not security. Furthermore, on-chain governance can increase these thresholds.

#### A Note on Relayer Services
Axelar runs relayer services which observe all connected chains, and these relayers will pick up the event and submit it to the Axelar network for processing. These relayer services are a free, operational convenience Axelar provides, and can be run by anyone who wishes to create and use their own relayer service instead.

The distribution of currently existing alternatives is unclear in Axelar and may represent a point of centralisation and risk for axelar.  



## LlamaRisk Gauge Criteria
1. **Is it possible for a single entity to rug its users?** No, greatest point of centralisation threshold seem to be the multisig base contract allowing for upgradbility of smart contract. 


4. **If the team vanishes, can the project continue?** The code is well documented and open source. Yet, a fork will be required to maintain smart contract upgradbility. In addition funds seemed to be held with to company and no treasury was idenified.
 
6. **Does the project viability depend on additional incentives?**  No, additional incentives are required for the project to function apart from the programtically encoded inflation distributed to validators.

8. **If demand falls to 0 tomorrow, can the users be made whole?** Yes, user should be able to redeem bridged assets at the source chain by bridging back.

10. **Do audits reveal any concerning signs?** The most concering sign is the upgradability of Smart Contract through AxelarGatewayProxyMultisig.sol, which is controlled by a 4 out of 8 multisig. 


### LlamaRisk Recommendation

Overall, it seems that a single entity cannot rug its users as the lowest point of centralization is the multisig base contract. The project's code is well-documented and open-source, allowing for a potential fork to maintain smart contract upgradability in case the team vanishes. The project's viability does not depend on additional incentives, as inflation distributed to validators is programatically encoded. Users should be able to redeem bridged assets at the source chain by bridging back even if demand falls to zero tomorrow. However, the upgradability of smart contracts through AxelarGatewayProxyMultisig.sol, controlled by a 4 out of 8 multisig, is the most concerning sign revealed by audits. Nevertheless, overall the risk team approves of adding  axlUSDC <> USDC and axlUSDC <> FraxBP Pools to the requested chains. 



