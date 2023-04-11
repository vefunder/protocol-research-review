# Asset Risk Assessment: axlUSDC (Axelar) 

### A look into the Axelar cross-chain bridge protocol and its multi-chain axlUSDC stablecoin

### TL;DR
Axelar is a cross-chain bridge protocol built on the Cosmos SDK, providing secure cross-chain communication and allowing users to interact with any supported blockchain assets and applications. It offers a variety of tools and APIs for developing Web3 applications, abstracting away from networking and deployment specifics. Since its mainnet launch in February 2022, Axelar has facilitated a significant number of cross-chain activities and supported multiple chains, including Fantom, Polygon, Avalanche, and Arbitrum.

Axelar's axlUSDC is a wrapped representation of USDC, enabling users to transact the stablecoin across various blockchain networks. Users can access axlUSDC through DEX liquidity pools, cross-chain liquidity routers such as Squid, or via Satellite, a cross-chain bridge built by Axelar. The platform aims to enable composable USDC using its General Message Passing (GMP) capability in collaboration with Circle's Cross-Chain Transfer Protocol (CCTP).

Axelar's technical security risk appears to be well-managed, with comprehensive measures to ensure user funds' safety. However, users should remain aware of the potential risks associated with bridge protocols. It is worth noting that the upgradability of smart contracts through AxelarGatewayProxy.sol is controlled by a 4-of-8 multisig, with a recommendation to replace it with a DAO governed by tokenholders in the future. Overall, LlamaRisk supports the continuation of CRV incentives to axlUSDC/USDC and axlUSDC/FraxBP pools on the requested chains.

### Useful Links
Another indicator suggesting the growth of the network is the growth of Daily Active Addresses (DAA), which have steadily been growing. DAA is a common metric used to measure the number of unique addresses that were active on a blockchain network during a given day. The chart below considers an active address as a unique address sending an on-chain transaction to the network over a rolling 24-hour period. 
#### Basic information:
* [Website](https://axelar.network/) 
* [Twitter](https://twitter.com/axelarcore)
* [Discord](https://discord.com/invite/aRZ3Ra6f7D)
* [Block Explorer](http://axelarscan.io)
* [Dev Docs](http://docs.axelar.dev)
* [GitHub](https://github.com/axelarnetwork)
* [Audits](https://github.com/axelarnetwork/audits)
* [Axelar Network Bug Bounties | Immunefi](https://immunefi.com/bounty/axelarnetwork/)
#### Learning materials:
* Introductory materials: [Article](https://medium.com/axelar/a-technical-introduction-to-the-axelar-network-3c4bf9fe4dc3) | [Video](https://www.youtube.com/watch?v=0-Q1mP2vmGE ) 
* [Whitepaper](https://axelar.network/axelar_whitepaper.pdf)
* [Comparision of AMBs](https://blog.li.fi/navigating-arbitrary-messaging-bridges-a-comparison-framework-8720f302e2aa)
#### Project specific:
* axlUSDC Contracts: [Assets | Axelarscan](https://axelarscan.io/assets)
* [Satellite (Token Bridge)](https://satellite.money/)
#### Metrics & Data 
* [Validator Security overview](https://observatory.zone/axelar/validators)
* Metrics: [Metrica](https://https://app.metrika.co/axelar/dashboard/network-overview?tr=1d) | [Dune](https://dune.com/axelarnetwork/axelar)
* [Research Notes](https://docs.google.com/spreadsheets/d/1VaNHKrsYGoQ0a2hU41gfr105k1UpZ0M7-SO1qsURmF8/edit?usp=sharing)


### Relation to Curve

Axelar has put forward two successful proposals to implement CRV gauge rewards for 4 axlUSDC pools on Fantom, Polygon, Avalanche, and Arbitrum:

1. The [first proposal](https://gov.curve.fi/t/proposal-to-add-axlusdc-usdc-to-the-gauge-controller/8694), executed in February 2023, was for the axlUSDC/USDC pools on multiple chains, including [Polygon](https://curve.fi/#/polygon/pools/factory-v2-221/deposit), [Avalanche](https://curve.fi/#/avalanche/pools/factory-v2-82/deposit), and [Fantom](https://curve.fi/#/fantom/pools/factory-v2-85/deposit), which serve as primary liquidity venues for axlUSDC on the Axelar network. This proposal sought approval for a gauge to incentivize USDC and axlUSDC liquidity on Curve, leveraging the already high trading volumes and native yields of axlUSDC pools.
2. The [second proposal](https://gov.curve.fi/t/proposal-to-add-axlusdc-fraxbp-to-the-gauge-controller/8907), executed March 2023, is for the axlUSDC/FRAXBP pool on [Arbitrum](https://curve.fi/#/arbitrum/pools/factory-v2-83/deposit), which serves as the primary liquidity venue for axlUSDC on the Axelar network. This proposal sought approval for a gauge to incentivize USDC, FRAX, and axlUSDC liquidity on Curve, with the support of the FRAX team.


## Introduction to Axelar
Axelar is a cross-chain bridge protocol centered on its native proof-of-stake blockchain built with Cosmos SDK. Its infrastructure provides secure cross-chain communication, allowing users to interact with any asset or application on any supported blockchain. Axelar's architecture offers uniform translation and routing of both tokens and arbitrary messages, allowing developers to connect users, assets, and applications in multiple ecosystems. Axelar securely processes cross-chain messages through a permissionless set of validators that constantly produce blocks by voting on the legitimacy of all messages. Additionally, the Axelar SDKs offer a comprehensive suite of tools and APIs for developing Web3 applications, abstracting away networking and ecosystem-specific deployment considerations.


### Founding Team
The team (according to Linkedin) now has over [50 employees working on Axelar](https://www.linkedin.com/company/axelarnetwork/) and is the brainchild of Sergey Gorbunov and Georgios Vlachos. 

[Sergey Gorbunov](https://www.linkedin.com/in/sergegorbunov/details/experience/) is a co-founder of Axelar Network and an assistant professor at the University of Waterloo, where he researches cryptography and blockchain technology. Prior to founding Axelar, Sergey was part of the founding team at Algorand, leading the cryptography group. He has also spent time at IBM T.J. Watson Research Centre. Sergey has a strong academic background in computer science and extensive experience in the blockchain and cryptography industries.

[Georgios Vlachos](https://www.linkedin.com/in/georgiosvlachos/) is a co-founder of Axelar Network. Prior to founding Axelar, he served as the Head of Mathematics at Algorand. Georgios holds a Master's degree in Computer Science from MIT where he researched machine learning and data mining. Georgios has a strong background in mathematics and computer science, with a particular focus on algorithms and cryptography. 


### Brief Project History 

Several significant events have shaped Axelar's trajectory and contributed to its success:

* **06/2020**: [Axelar Inc.](https://opencorporates.com/companies/ca/12136040) was incorporated in Canada by Sergey Gorbunov and Georgios Vlachos.
* **11/2020**: [$3.75M Raise](https://www.coindesk.com/tech/2020/11/13/algorand-linked-axelar-raises-375m-in-seed-funding-to-help-blockchains-communicate/) in a Seed Round from 11 investors, the funding round included participation from Binance X, DCVC (Data Collective), Lemniscap, Divergence Ventures, Waikit Lau, and Naval Ravikant.
* **06/2021**: Axelar raised [$25 million in a Series A funding round](https://www.coindesk.com/markets/2021/07/15/axelar-raises-25m-in-series-a-fundraising-led-by-polychain-capital) led by Polychain Capital, with participation from Blockchain Capital, Divergence Ventures, and others.
* **11/2021**: Axelar Network raised an [undisclosed amount of funding in a strategic round](https://axelar.network/blog/axelar-announces-strategic-investment-from-crypto-com-capital) from 6 investors in a Venture Round, with Crypto.com Capital as the lead investor.
* **02/2022**: Axelar released its cross-chain communication protocol suite, tools, and APIs to the public. It also raised [$35 million in Series B funding](https://www.coindesk.com/business/2022/02/15/blockchain-interoperability-network-axelar-raises-25m-at-1b-valuation/) round led by Polychain and Dragonfly Capital. Additional investors include Binance, Coinbase Ventures, DCVC, Divergence Ventures, Galaxy, Lemniscap, and North Island Ventures. The funding round values the company at $1 billion, making it a unicorn.
* **03/2023**: Launch of [Axelar Virtual Machine](https://www.coindesk.com/tech/2023/02/27/axelar-introduces-virtual-machine-for-developers-building-cross-chain-crypto-apps/), which enables developers to build dApps once and run them on all chains, including EVM, Cairo VM, Cosmos, and other ecosystems.


### Axelar today
Since its public mainnet launch in [February 2022](https://axelar.network/blog/axelar-begins-rollout-of-public-mainnet-launch-for-february-2022-bringing-decentralized-interoperability-to-ethereum-avalanche-terra-moonbeam-and-others), Axelar has facilitated a significant number of cross-chain activities. See the source below for updated figures on transaction volumes and connected chains.


|![Supported Networks](https://i.imgur.com/WGWEw9G.png)|
|-------|
|<center>[Source: https://axelarscan.io/]</center>|



Axelar provides analytics of the total value locked (TVL) across all supported chains, including assets locked on each source chain and where assets are bridged.
|![TVL](https://i.imgur.com/ttl95tH.png)|
|-------|
|<center>[Source: https://axelarscan.io/tvl]</center>|


### Exploring Axelar Protocol in more depth: A practical exploration 

A high-level understanding of Axelar is necessary to conceptualise the risk to Curve LPs. To understand [Axelar and the respective protocol components](https://axelar.network/blog/an-introduction-to-the-axelar-network), this section will go over the [flow of a cross-chain request](https://docs.axelar.dev/learn/network/flow) initiated by a dApp or a user, whether sending a token or any other arbitrary data.

|![image alt](https://i.imgur.com/QcMgjtP.jpg "Axelar Technology Stack")|
|-------|
|[Source: [Axelar](https://axelar.network/blog/an-introduction-to-the-axelar-network)]|

**Step 1 - Message request origination:** A dApp user initiates a request through the Axelar API on any connected chain. The API allows users to send arbitrary data, including token transfers and smart contract calls, to a specified destination chain.

**Step 2 - Gateways:** Gateways are deployed on each connected chain as either a smart contract (EVM chains) or an application with logic and ability to communicate with the Axelar network (Cosmos/non-EVM chains). The Gateway receives messages from the dApp and sends them into the Axelar network for routing to any connected chain. The Gateway is controlled by a key held jointly by all Axelar validators, using a multiparty cryptography scheme. Each validator holds multiple key shares based on the amount of AXL tokens staked. Gateways have two main functions: (1) enabling cross-chain message requests on the source chain, (2) executing messages on the destination chain.

**Step 3 - Relayers:** When a Gateway receives a message, it generates an event, which is picked up by a relayer and submitted for processing. Axelar employs relayers to monitor events produced by Gateways on all connected chains. A free relayer service is operated by Axelar, but anyone can permissionlessly participate as a relayer for the network.

**Step 4 - Validators:** Validators are the Axelar block producers that participate in proof-of-stake consensus and verify the truthfulness of cross-chain events being submitted. Each validator runs a node for the source chain where the event originated and verifies the event by querying their RPC endpoint. Validators are incentivized to run nodes for as many chains as possible, with rewards based on the number of chains they support. The event is validated by the Axelar network, processed by the consensus protocol, and recorded in a block. The cross-chain message is then routed into a queue for the destination chain and ready to be sent to the destination Gateway. 

**Step 5 - Submitting a message to the destination chain:** Axelar reaches consensus on a transaction when a set threshold of Validator key shares have signed. Once authorized, anyone can submit the signed message to the destination chain for processing. Another set of relayer services monitor outgoing transaction queues for approved and signed cross-chain messages, and periodically submit them to external chains. The Gateway on the destination chain receives the approved message and stores the contract call approval with a hash of the payload, which can now be executed at any time. (Again, Axelar provides these relayer services for free, but users can create and use their own relayer services instead.)

**A Note on Gas and Executor services:** Axelar offers Gas and Executor services to make executing cross-chain messages easier. The Gas Receiver smart contract allows users to pay for all transaction fees in a one-time payment on the source chain in the native token. The Gas Receiver estimates the total gas cost and converts tokens into AXL, destination-chain tokens, and other required currencies.


## Assets

### Native token: AXL
AXL is the native token of Axelar Network. It can be classified primarily as a [utility and governance token](https://medium.com/@axelar-foundation/an-overview-of-axl-token-economics-4dc701c9054d) and serves four primary functions: 
1. Medium for transaction fees and any other fees for network usage paid by users to the validators
2. Governance over proposals such as a parameter change or protocol upgrade.
3. Delegation and validator rewards to secure the network.
4. Reward ecosystem contributors who further the development and growth of the Axelar network.


### axlUSDC
Axelar's [axlUSDC is a wrapped representation of USDC](https://axelar.network/blog/what-is-axlusdc-and-how-do-you-get-it) bridged from Ethereum to multiple chains supported by Axelar. It allows users to transact USDC across a multitude of ecosystems. USDC is a dollar-pegged stablecoin issued by Circle, a US-based company, and is mainly used on the Ethereum blockchain. However, axlUSDC allows USDC to be used across multiple blockchain networks.

Axelar's cross-chain bridges generate axlUSDC by accepting a deposit of USDC at an [Axelar Gateway](https://etherscan.io/address/0x4f4495243837681061c4743b74b3eedf548d56a5) on the Ethereum network and then minting an equivalent amount of axlUSDC on the destination chain. Every unit of axlUSDC represents a unit of USDC that is locked in an Axelar Gateway on the Ethereum network. In essence, Axelar's dynamic validator set, numbering 70 at the time of writing, secures axlUSDC.

Users can acquire axlUSDC in three ways. 

- (1) They can swap it via [axlUSDC pairs](https://axelar.network/liquidity-pools) on various DEX liquidity pools. 
- (2) They can swap it via [Squid](https://app.squidrouter.com/), a cross-chain liquidity router built on Axelar, which provides liquid cross-chain swaps using axlUSDC as a routing asset. 
- (3) They can mint it via [Satellite](https://satellite.money/), a cross-chain bridge built by Axelar.

Contract addresses for axlUSDC on all supported chains can be found [here](https://docs.axelar.dev/resources/mainnet#assets).

### Composable USDC
Circle recently announced a [Cross-Chain Transfer Protocol](https://www.circle.com/en/pressroom/circle-enables-usdc-interoperability-for-developers-with-the-launch-of-cross-chain-transfer-protocol) (CCTP), slated for release Q1 2023 on Ethereum and Avalanche, and later available on Solana and other chains. Axelar will be a partner protocol, enabling [composable USDC](https://www.circle.com/blog/composable-usdc-seamless-multichain-ux-by-axelar) with its General Message Passing (GMP) capability. This allows arbitrary data to accompany a token transfer to enable use-cases such as seamless cross-chain swaps, one-click deposits/withdrawals into application-specific blockchains, and cross-chain NFTs. 

The Axelar team is working closely with Circle to enable seamless functionality of native USDC using Axelar's GMP. They believe it is likely most dApp developers will prefer to support a native version of USDC, where available. Nevertheless, numerous chains supported by Axelar are not included in Circle's CCTP roadmap. On these chains, axlUSDC will persist as a secure, interchain stablecoin for dApps to utilize.

#### A Note on USDC
Between 11.03 to 14.03.2023 [USDC depegged below $1](https://twitter.com/DefiIgnas/status/1635980042294669312) due to the failure of Silicon Valley Bank (SVB), which held $3.3 billion in reserves backing the stablecoin's value. The bank's collapse was due to a bank run amid concerns over its financial health. Coinbase and Binance's decision to stop USDC conversions and the rapid depletion of the Curve 3pool contributed to USDC's depeg. The U.S. government took emergency action to [protect all SVB bank depositors](https://www.npr.org/2023/03/13/1163028329/biden-administration-steps-in-to-save-customers-of-silicon-valley-bank), thereby alleviating panic and ultimately bringing USDC back to its peg.

|![USDC depeg](https://i.imgur.com/LGcEvXI.png)|
|-------|
|[Source: https://coinmarketcap.com/currencies/usd-coin/]|

The depeg raised concerns over the trustworthiness of USDC and the risk of contagion from centralized finance (CeFi) on crypto. As a wrapped representation of USDC, axlUSDC is exposed to the risk of the underlying asset in addition to the risks associated with Axelar network itself. 


### Contract Architecture Overview

Axelar provides the main smart contracts for EVM [here](https://github.com/axelarnetwork/axelar-cgp-solidity).

**Interfaces:**

_Define the required functions for the respective contracts implementing them._
- a. IAxelarGateway.sol
- b. IERC20.sol
- c. IERC20BurnFrom.sol
- d. IAxelarExecutable.sol

**Base Contracts:**
- a. Ownable.sol - provides ownership functionality to contracts that inherit from it.
- b. EternalStorage.sol - storage contract for the proxy.
- c. AdminMultisigBase.sol - multisig governance contract used for upgrading implementations through voting.
- d. ERC20.sol - the base ERC20 contract that other ERC20 extension contracts inherit from.

**ERC20 Extensions:**

_Extensions of the base ERC20 contract, providing additional functionalities like spending permits, minting with a capped total supply, and burn capabilities._
- a. ERC20Permit.sol
- b. MintableCappedERC20.sol
- c. BurnableMintableCappedERC20.sol

**Axelar Core Contracts:**
- a. [AxelarGatewayProxy.sol](https://etherscan.io/address/0x4F4495243837681061C4743b74B3eEdf548D56A5#code) - Core contract of the Axelar network (proxy) that accepts signed commands from Axelar validators.
- b. [AxelarGateway.sol (Implementation Contract)](https://etherscan.io/address/0xEd9938294aCF9EE52D097133CA2cAafF0C804F16#code) - Core contract of the Axelar network (current implementation) that accepts signed commands from Axelar validators.
- c. [AxelarAuthWeighted.sol](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code) - Weighted multisig authentication contract used by the gateway for verifying messages.

**Utility Contracts:**
- a. ECDSA.sol - Utility contract for signature authentication checks.
- b. TokenDeployer.sol - Responsible for deploying BurnableMintableCappedERC20 tokens by a delegate call from the gateway contract when it receives a signed command from the Axelar network.
- c. DepositHandler.sol - Deployed at deposit addresses, enabling the burning/locking of tokens sent by users. It prevents re-entrancy and works in conjunction with the gateway contract.
- d. AxelarDepositService.sol - Service used to generate deposit addresses for ERC20 tokens, native currency transfers, or unwrapping native currency from wrapped ERC20 tokens.
- e. [AxelarGasService.sol](https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#code) - Handles cross-chain gas payments. It accepts payments for covering gas costs on the destination chain. Gas payments should occur with the same parameters right before calling callContract or callContractWithToken on the gateway. 


## Axelar Ecosystem Health and Risk Metrics

Consensys [wrote a research piece](https://consensys.net/research/measuring-blockchain-decentralization/) attempting to quantify decentralisation among various layer 1 ecosystems. Its analysis for Axelar provides some useful metrics.


### Network Usage 

|![Network Usage](https://i.imgur.com/M3xHZnc.png)|
|----|
|[Source: [Axelarscan](https://axelarscan.io/transfers)]|

When analyzing the daily transfer and volume statistics of the Axelar Network from January 15, 2022, to February 28, 2023, the impact of Terra Luna becomes quite apparent. The statistics show that the daily transfer and volume on Axelar experienced a sharp spike caused by the [Terra UST collapse](https://www.coindesk.com/learn/the-fall-of-terra-a-timeline-of-the-meteoric-rise-and-crash-of-ust-and-luna/), showing significant cross-chain activity during the crisis. Despite the setback this event posed to the overall crypto market, Axelar has demonstrated resilience and stable growth in terms of daily transfers. This growth is particularly noteworthy considering the timing of the market cycle and is a positive sign for the network's future growth and adoption. 


Statistics on [General Message Passing](https://axelarscan.io/gmp/stats?fromTime=1679309728011) show that after May 2022 (i.e. after the collapse of Luna Terra), the top chain pairs on the Axelar Network are mainly between Polygon, Moonbeam, and Ethereum. While Polygon is the most popular, there is a diverse range of destination contracts on the network. The most popular destinations are Polygon, BNB Chain, and Moonbeam, accounting for 21.64%, 16.37%, and 14.04% of the total messages, respectively. These statistics suggest the diversity and versatility of the Axelar Network, with users utilizing a range of chains and contracts for their cross-chain transfers. It will be interesting to track any shifts in the distribution of messages and destination contracts in the future.

|![GENERAL MESSAGE PASSING](https://i.imgur.com/nZkq3nb.png)|
|-------|
|[Source: [Axelarscan](https://axelarscan.io/gmp/stats)]|

An additional signal that implies the expansion of the network is the consistent increase in Daily Active Addresses (DAA). DAA is a common metric used to measure the number of unique addresses that were active on a blockchain network during a given day. The chart below considers an active address as a unique address sending an on-chain transaction to the network over a rolling 24-hour period. 

|![DAA](https://i.imgur.com/ymI48qF.png)|
|---|
|[Source: [Artermis Axelar Dashboard](https://app.artemis.xyz/dashboard/axelar)]|


### Validator Performance

The chart below displays the number of blocks proposed by validators on an hourly basis over a one-month period. The chart can be useful in identifying issues related to block production by an individual validator, but it may also provide insight into network-wide issues. For example, a rapid color shift from deep purple to grey would suggest that the validator is now producing significantly fewer blocks over a selected time interval. Censorship of a region or client issues may be apparent if many validators stop producing blocks. 

|![Overview of Number of Proposed Blocks by Validator](https://i.imgur.com/F6qOeJd.png)|
|------|
|[Source: [Metrika Axelar Dashboard](https://app.metrika.co/axelar/dashboard/network-overview?tr=1M)]| 

Overall, the chart shows good network stability (reasonably consistent validator performance) over the one-month period with a healthy propagation of blocks.


### Block production

|![Block Production](https://i.imgur.com/gt4N24J.png)|
|-------|
|[Source: [Metrika Axelar Dashboard](https://app.metrika.co/axelar/dashboard/network-overview?tr=1M)]

This chart displays block production trends on the Axelar Network over the past 30 days. The blue bars represent the number of blocks generated per hour. The network has maintained a stable block production rate of approximately 600 blocks per hour. A decrease in this rate may indicate network congestion. The purple line shows the maximum time it took to produce a block in a given hour, which varied between 6 and 22 seconds during the observed period. The green line represents the average block production time, which remained stable at 6 seconds, in line with the protocol specifications.

Based on the stable block production rate and relatively consistent average block production time during the observed period, it appears that the consensus on the Axelar Network is stable and operating within the expected parameters.


### Collusion Threshold

The collusion threshold is a key security feature of the Axelar Network that helps to prevent validators from colluding and compromising the integrity of the network. The threshold is determined by the number of validators required to sign off on a block and is set at two-thirds of the total number of validators weighted by the stake. 

As of [September 2022 with Meave Upgrade](https://axelar.network/blog/axelar-implements-quadratic-voting-with-maeve-upgrade), the Axelar Network has introduced [quadratic voting](https://vitalik.ca/general/2019/12/07/quadratic.html), which further enhances the security and decentralization of the network. Currently, the network is secured by 70 active validators and the collusion threshold is set at 2/3 of validators quadratically weighted by the stake. At the time of writing, a successful collusion would require 35 validators to collude to gain a supermajority (see [cumulative share % - quadratic](https://axelarscan.io/validators) here for the current threshold). Given this context, it is crucial to evaluate the presented metrics with these factors in mind.


### Validator Nodes 

|![Geographical Node Distribution](https://i.imgur.com/ddVom6l.png)|
|------|
|[Source: https://observatory.zone/axelar/validators]|

At the time of writing, the Axelar validator set has a total of 92 nodes distributed across different regions. The Americas region has 21 nodes with a stake of 22.82%, while the Europe region has 58 nodes with a stake of 63.04%. The Asia Pacific region has 2 nodes with a stake of 2.17%, and there are 13 nodes with no specified region, accounting for a stake of 14.13%. Based on [the data provided](https://observatory.zone/axelar/countries), it appears that Europe has the highest concentration of Axelar validators in terms of both nodes and the stake. This suggests some risk due to geographical centralisation.

It is important to note that this data only represents the validators that are currently connected and may not reflect the full distribution of validators over time. Additionally, this data does not necessarily provide information on the specific geographic location of the validators within each region, but rather through which country nodes access the network. This data can be altered by using VPN or routing through a private server.


### Internet Service Providers (ISPs)

The [top three Internet Service Providers (ISPs)](https://observatory.zone/axelar/isps) in terms of stakes and number of nodes for Axelar are Hetzner Online GmbH, OVH SAS, and Amazon.com, Inc. OVH SAS has the highest stake with 27.81% and 16 nodes, followed by Hetzner Online GmbH with 19.80% and 22 nodes, and Amazon.com, Inc. with 9.98% and 12 nodes.

The aggregation of stake and nodes within the leading three ISPs embodies a centralizing tendency that necessitates vigilant monitoring. The main issues associated with centralization stem from regulatory risks, including the banning of node services and the termination of contracts. Moreover, should a data center cease operations, the network could temporarily experience a substantial loss of nodes, leading to heightened centralization. It is essential to bear in mind that the dispersion of nodes and stakes among different ISPs can vary over time, and Axelar is still an emerging project. Additionally, the concentration of ISPs does not substantially diverge from established industry norms.

### Function Call Diversity 

The data below shows the count of different event types occurring in cross-chain transfers on the Axelar Network for the past 30 days. 

|![](https://i.imgur.com/gVSCVVG.png)|
|-------|
|[Source: [Metrika Axelar Dashboard](https://app.metrika.co/axelar/dashboard/network-overview?tr=1M)]|

The function calls on Axelar can be defined as follows:  

* **Link:** The process of linking an Axelar address with an Ethereum Virtual Machine (EVM) address, allowing for the transfer of assets between the two chains.
* **SignCommands:** The act of signing a command to initiate a cross-chain transfer, verifying the authenticity of the transfer and ensuring that it is authorized.
* **SubmitSignatureRequest:** The process of submitting a request to initiate a cross-chain transfer, including the signed command and other necessary information.
* **ConfirmDeposit:** The confirmation of a deposit on the source chain, indicating that the assets have been transferred to the Axelar Network and are available for transfer to the target chain.
* **CreatePendingTransfers:** The creation of a pending transfer on the target chain, including the necessary information to complete the transfer.
* **ConfirmERC20Deposit:** The confirmation of a deposit of ERC20 tokens on the source chain, indicating that the tokens are available for transfer to the target chain.
* **ExecutePendingTransfers:** The execution of the pending transfer on the target chain, completing the cross-chain transfer and transferring the assets to the recipient.
* **RegisterChainMaintainer:** The registration of a new chain maintainer, is responsible for maintaining and monitoring the cross-chain transfers on a specific chain, ensuring that they are secure and efficient.

The SubmitSignatureRequest event type is the most frequent, occurring close to 1 million times. The Link event type occurred 32,185 times, while SignCommands occurred 13,260 times, suggesting that a significant number of users are linking their Axelar addresses with EVM addresses and signing commands to initiate cross-chain transfers. The ConfirmERC20Deposit and ExecutePendingTransfers event types occurred less frequently. Finally, the RegisterChainMaintainer event type occurred only 109 times.

[Function call diversity](https://consensys.net/research/measuring-blockchain-decentralization/) refers to the number of different functions that are being called within a software system or application. In the context of an arbitrary message bridge like Axelar, function call diversity is important because it can suggest insights on system robustness, maintainability, and extensibility. There are a range of different event types occurring in cross-chain transfers on the Axelar Network. Overall, the diversity of event types suggests that the platform is versatile and can accommodate a range of different use cases and applications.


### Client Diversity

[Client diversity](https://clientdiversity.org/) refers to the presence of multiple software implementations of a blockchain network protocol. This is important in general-purpose blockchains like Ethereum to enhance network security, reliability, and decentralization, as failures associated with the client can potentially disrupt the entire network.

The Axelar-core client can be found [here](https://github.com/axelarnetwork/axelar-core). Due to the app-specific nature (as opposed to a general-purpose blockchain like Ethereum), the Axelar team claims that a multi-client model doesn’t add many benefits, similar to other Cosmos-based chains. Client diversity may be less important due to the network's specialization and focus, and the ability to leverage the underlying security of the Cosmos Hub which has multiple client implementations. 


### Developer Activity

Weekly [commit and weekly active developers](https://app.artemis.xyz/developers/Axelar%20Network?includeForks=false) are metrics used to measure the productivity and activity of open-source repositories. Weekly commit refers to the number of commits made by developers in a given week. A commit is the smallest unit of work and can vary in size, but is generally a good indicator of developer productivity. Meanwhile, the number of active developers in a week refers to the number of developers who have made at least one commit during that time period. This metric is useful in tracking the level of developer activity and engagement with the project. Both of these metrics are important in understanding the health and progress of open-source projects, as they provide insight into the level of developer involvement and output.

|![Developer Activity](https://i.imgur.com/4Zw6Z1k.png)|
|--------|
|[Source: [Artemis Axelar Dashboard](https://app.artemis.xyz/developers/Axelar%20Network?includeForks=false)]|

Axelar has a total of 56 repositories. The weekly commits have decreased by 61.7% in the last month and 59.1% in the last 3 months. The number of weekly active developers has decreased by 30.8% in the last month and 47.1% in the last 3 months. For Squid, representing the "Sub-Ecosystems" in the Figure above, there are 6 repositories and only 2 weekly commits, which represents an 86.7% decrease in the last month and a 33.3% decrease in the last 3 months. The number of weekly active developers has decreased by 75.0% in the last month and 50.0% in the last 3 months.

While the decline seems extreme, this is rather in line with other ecosystems which saw similar percentage changes of drawdowns due to the cyclical nature of the crypto markets.  


### Token-Related Risks

Token-related risk refers to the potential for loss or negative impact on a blockchain network's native token or other tokens within its ecosystem. This type of risk is inherent in any decentralized system that relies on token economics, where tokens serve as a means of value transfer, governance, and incentivization. This section will explore some aspects of token-related risk in Axelar.

#### Initial Token Distribution of $AXL
The initial token distribution of a project tends to have a degree of stickiness to it. This is particularly true for proof-of-stake systems, where initial token holders have no incentive to redistribute their tokens and promote decentralization since their wealth and staking power are directly correlated.

For Axelar, it is observed that around 59% of the initial token distribution was allocated to individuals closely associated with the project, including company operations, team members, and supporters. The remaining 41% was distributed to the community via community programs and sales. The following illustration demonstrates the AXL token allocations at the genesis stage, with a different terminology for insiders.

|![image alt](https://i.imgur.com/YbLzetz.png "AXL token allocations at genesis")| 
| -------- | 
|[Source: [AXL token allocations at genesis](https://medium.com/@axelar-foundation/an-overview-of-axl-token-economics-4dc701c9054d)]| 


If we look at the [distribution of bonded validators at the time of the analysis](https://docs.google.com/spreadsheets/d/1VaNHKrsYGoQ0a2hU41gfr105k1UpZ0M7-SO1qsURmF8/edit?usp=sharing), we can see that the total stake ranges from 64,000 AXL tokens to 45.7 million AXL tokens. The voting power of a validator ranges from .01% to 6.43%, while the quadratic voting power ranges from .12% to 3.32%. The total delegation of validators ranges from 4 to 1,492.

![](https://i.imgur.com/RM3H6Fd.png)

From the data observed, it appears that the AXL tokens are relatively well distributed among validators, with no single validator holding an excessively large amount of tokens and therefore voting power. As decentralisation is a property for censorship resistance, each LP should evaluate for themselves if they believe this is sufficient decentralization in the context of Axelar use cases. 

In addition, there is evidence of progressive decentralisation, as the Validator concentration has improved since this past review: a [comparison of Arbitrary messaging bridges by Lifi](https://blog.li.fi/navigating-arbitrary-messaging-bridges-a-comparison-framework-8720f302e2aa). 


## Technical Security Risk 

Technical security risk is a critical concern for any blockchain network as it involves potential vulnerabilities in the system's technical infrastructure. In this section, we will examine technical security risks associated with the Axelar network, including potential liveness issues, trust assumptions involving access controls, and smart contract security practices.

### Possible Liveness Issues

Axelar may experience [liveness issues if validators opt to support only certain chains](https://blog.li.fi/navigating-arbitrary-messaging-bridges-a-comparison-framework-8720f302e2aa). For a new chain to be incorporated, it must receive support from 60% of the validators, based on their quadratic voting power, to run a node for that particular chain. Despite the fact that validators have the option to maintain a specific EVM chain, the majority vote threshold remains at 60% of the quadratic voting power of the total validator set. Therefore, if an EVM chain fails to attract enough supporting validators, it may affect liveness (although not security). Furthermore, on-chain governance can increase these thresholds.

#### A Note on Relayer Services
Axelar runs relayer services which observe all connected chains, and these relayers will pick up the event and submit it to the Axelar network for processing. These relayer services are a free, operational convenience Axelar provides, and can be run by anyone who wishes to create and use their own relayer service instead.

The distribution of currently existing alternatives is unclear in Axelar and may represent a centralisation risk. An outage of the Axelar relayer service could result in a network-wide outage. 


### Access Control 

Access control refers to the control over the smart contract code and its execution. In this section, we will discuss the risks associated with smart contract ownership and how they can be mitigated to ensure the security of the network. 

#### AxelarGatewayProxy.sol
AxelarGatewayProxy.sol, formerly known as AxelarGatewayProxyMultisig.sol, is the central contract of the Axelar network. It is using library contracts such as AxelarGateway.sol, AxelarAuthWeighted.sol, and  TokenDeployer.sol. Additionally, it is supported by other service contracts to provide other functionalities, such as cross-chain gas payments, and permission-less deposit addresses for cross-chain token transfers.

This contract is owned by a multisignature scheme that is implemented in AdminMultisigBase.sol. This scheme allows multiple addresses to collectively own and control the contract. At present (i.e. epoch 3) the admin threshold is set to [4 out of 8 addresses](https://etherscan.io/address/0x4F4495243837681061C4743b74B3eEdf548D56A5#readProxyContract#F2). The owner addresses are listed below: 

    
| AxelarGatewayProxy.sol Owners |
|:--------------------------------------:|
| [0x3f5876a2b06E54949aB106651Ab6694d0289b2b4](https://etherscan.io/address/0x3f5876a2b06E54949aB106651Ab6694d0289b2b4) |
| [0x9256Fd872118ed3a97754B0fB42c15015d17E0CC](https://etherscan.io/address/0x9256Fd872118ed3a97754B0fB42c15015d17E0CC) |
| [0x5C8EF9ca7b43c93Ac4a146BeF77FAFbc7D3e69B7](https://etherscan.io/address/0x5C8EF9ca7b43c93Ac4a146BeF77FAFbc7D3e69B7) |
| [0x1486157d505C7F7E546aD00E3E2Eee25BF665C9b](https://etherscan.io/address/0x1486157d505C7F7E546aD00E3E2Eee25BF665C9b) |
| [0x2eC991B5c0B742AbD9d2ea31fe6c14a85e91C821](https://etherscan.io/address/0x2eC991B5c0B742AbD9d2ea31fe6c14a85e91C821) |
| [0xf505462A29E36E26f25Ef0175Ca1eCBa09CC118f](https://etherscan.io/address/0xf505462A29E36E26f25Ef0175Ca1eCBa09CC118f) |
| [0x027c1882B975E2cd771AE068b0389FA38B9dda73](https://etherscan.io/address/0x027c1882B975E2cd771AE068b0389FA38B9dda73) |
| [0x30932Ac1f0477Fbd63E4c5Be1928f367A58A45A1](https://etherscan.io/address/0x30932Ac1f0477Fbd63E4c5Be1928f367A58A45A1) |


**In practical terms, this means that the upgrading of contracts is subject to 4 out of 8 multisig signers.** Axelar plans to take further steps in decentralising the network by having the validator set jointly approve smart contract upgrades. This is a crucial step in achieving a higher level of decentralisation and eliminating the need for a governed multisig. The result will be a more decentralised network, offering greater transparency and security to users. Ultimately, Axelar aims to create a fully autonomous network that operates without any central authority or governing body.

![Screen Shot 2023-04-07 at 1 38 37 PM](https://user-images.githubusercontent.com/51072084/230675911-4c73268c-7ce4-4fa3-aaee-a4e9eaf46dc6.png)

Source: [AxelarGatewayProxy upgrade function](https://etherscan.io/address/0xed9938294acf9ee52d097133ca2caaff0c804f16#code)

#### AxelarAuthWeighted.sol
[AxelarAuthWeighted.sol](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code) is a core contract used by the Axelar network's gateway for verifying messages. It is a weighted multisig authentication contract that allows for multiple signers with different weights to approve a message. The contract is owned by AxelarGatewayProxy.sol, which is set during contract deployment.

|![AxelarAuthWeighted.sol](https://i.imgur.com/cucphk1.png)|
|---|
|[Source: [AxelarAuthWeighted.sol](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#readContract#F4)]|

#### TokenDeployer.sol
[TokenDeployer.sol](https://etherscan.io/address/0xe88Ab68Cd69e92294FcC3BBBD894Fb183197fA39#code) is a deployment library that contains the bytecode of the token. The Axelar Gateway is using Solidity delegatecall to utilize the library, and it is actually the gateway contract (AxelarGatewayProxy.sol) that is responsible for the deployment. This means that anyone can hypothetically use TokenDeployer to deploy their own instances of Axelar's token implementation without affecting the gateway contract.

#### AxelarGasService.sol
AxelarGasService.sol handles cross-chain gas payments. It is [owned by an EOA](https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#readProxyContract#F4), which can upgrade the contract.

In addition, AxelarGasService.sol uses an [EOA as the gasCollector](https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#readProxyContract#F2), which is responsible for collecting cross-chain gas fees from users and distributing them to validators who provide gas on the recipient chain. 

|<center>![AxelarGasService.sol](https://i.imgur.com/U64iCbP.png)</center>|
|---|
|[Source: [AxelarGasService.sol](https://etherscan.io/address/0x2d5d7d31F671F86C782533cc367F14109a082712#readProxyContract)]|


### Smart Contract Audit Review 

Axelar protocol's smart contracts have been deployed [to multiple testnets for various networks](https://docs.axelar.dev/dev/reference/testnet-contract-addresses), which indicates a commitment to testing and validation. Moreover, the Axelar network has [conducted several audits on its tech stack](https://github.com/axelarnetwork/audits), including their cryptographic library, smart contracts, frontend, and backend code. The audits were conducted by various auditors, such as Cure53, Certik, Oak Security, and NCC, and covered different components of the Axelar protocol. The audits were conducted in different phases, with some ongoing to ensure that any changes made to the code are reviewed for security. These audits provide strong evidence that the Axelar protocol is adequately audited and secure.

On a high level the findings can be summarised as: 

|<center>![Severity Findings by Auditor across Audit Reports](https://i.imgur.com/YcD88Fh.png)</center>|
|-----|
|[Source: [Google docs sheet](https://docs.google.com/spreadsheets/d/1VaNHKrsYGoQ0a2hU41gfr105k1UpZ0M7-SO1qsURmF8/edit#gid=2025182090)|

The Axelar team does not have a system for tracking resolved issues, making the review process an arduous task. Efforts should be made to imrove transparency in this regard. An overview of notable findings can be found in the Appendix, which includes issues that put user funds at risk and have not been resolved (or where it is unclear if it has been resolved).


### Axelar's Bug Bounty Program

[Axelar Immunefi bounty program](https://immunefi.com/bounty/axelarnetwork/) offers rewards ranging from USD $1,000 to a maximum of USD $2,250,000 for critical vulnerabilities, with separate payouts for different threat levels and severity classifications. The program also requires proof of concept for critical blockchain and smart contract bug reports, which is standard practice for most bug bounty programs. Additionally, the program specifies the assets in scope and provides a detailed explanation of the type of vulnerabilities they are looking for, making it easier for bounty hunters to identify potential security issues. However, the program requires KYC for all bug bounty hunters submitting a report, which may be a deterrent for some researchers. Axelar Network's bug bounty program seems to offer reasonable rewards for identifying critical vulnerabilities. This adds additional confidence to the technical robustness of the Axelar Network.

### Documentation 

[Axelar's smart contracts](https://github.com/axelarnetwork/axelar-cgp-solidity) are generally easy to find, but it can be challenging to locate all deployed contracts through the Gitbook Documentation. For example, to find the authentication module, we had to go through the AxelarGatewayProxy contract. It would be more transparent if all contracts were listed in the Gitbook documentation. Axelar has the [contract deployments available on Github](https://github.com/axelarnetwork/axelar-cgp-solidity/blob/3e7d6751a3016e4694b1ce643f11530442731e09/info/mainnet.json). Overall, the code documentation is comprehensive and includes a whitepaper, a flow chart within the whitepaper that documents the software architecture, and well-documented source code available on GitHub. Additionally, it is possible to trace the documented software to its implementation in the protocol's source code and development history (e.g. versioning, commits).


## LlamaRisk Gauge Criteria

1. **Is it possible for a single entity to rug its users?** 

No. Although there is a significant point of centralisation from the 4-of-8 multisig allowing for upgradability of the smart contracts. 

2. **If the team vanishes, can the project continue?** 

Somewhat. The code is well documented and open source. Validators on the network can autonomously achieve consensus without team involvement. Relayers are a permissionless service. However, a fork will be required to maintain smart contract upgradability. In addition, funds seem to be held with the company and no community-owned treasury was identified.

3. **Does the project viability depend on additional incentives?**  

No. Additional incentives are not required for the project to function apart from the programtically encoded inflation distributed to validators as part of the proof-of-stake consensus mechanism.

4. **If demand falls to 0 tomorrow, can the users be made whole?** 

Yes. axlUSDC is always backed 1:1 with USDC so users should be able to redeem bridged assets to the source chain in any market condition.

5. **Do audits reveal any concerning signs?** 

The most concering sign is the upgradability of smart contracts through AxelarGatewayProxy.sol, which is controlled by a 4-of-8 multisig. 

### LlamaRisk Recommendation

All signs point to Axelar being a protocol that takes security seriously and has taken reasonable measures to ensure safety of user funds. Nevertheless, users should remain aware that bridge protocols have historically been very lucrative targets for hackers, and bridge hacks make up [4 of the 5 top exploits](https://rekt.news/leaderboard/) in the RektHQ leaderboard.

Moving forward, we would like to see more robust access control of the smart contracts by replacing the 4-of-8 multisig with a DAO governed by tokenholders. Overall, LlamaRisk supports the continuation of CRV incentives to axlUSDC/USDC and axlUSDC/FraxBP pools on the requested chains.


## Appendix: Notable Audit Findings

This section will review issues found in the latest audit report for each repository. Only the issues that put user funds at risk and have not been resolved (or where it is unclear if it has been resolved) will be listed here.


#### Repository | axelar-utils-solidity | [Ackee Blockchain](https://raw.githubusercontent.com/axelarnetwork/audits/main/audits/2022-08%20Ackee%20blockchain.pdf)

> ###### Ackee Blockchain |2022-08| H1: The forecall and forecallWithToken can be called repeatedly with the same payload
> The AxelarForecallable contract has been found to have a high severity issue where the forecall function can be called repeatedly with the same payload. As a result, the check preventing double execution can be bypassed, which can be performed by anyone at any time since forecall is a publicly-accessible function. This can lead to the activation of the payload repeatedly, causing undefined consequences for the system. For instance, an attacker could call the forecall function with the forecaller address set to zeroaddress and execute a specific payload repeatedly. The recommendation is to add a zero-address check for the forecaller address in both functions (forecall and forecallWithToken) to prevent the issue. It is worth noting that the team believes that double execution should not cause any critical scenario. From the audit is unclear if this issue has been resolved. Investigation of the repository has led to this contract being completely removed from the main branch, suggesting the contract is no longer needed or has been absorbed into other contracts. 


#### Repository | axelar-cgp-solidity | [Chaintroopers](https://github.com/axelarnetwork/audits/blob/main/audits/2022-08%20Chaintroopers.pdf)

> ###### Chaintroopers |2022-08| C1.1/C1.2: No upper bound and lower bound in one operator's weight at "AxelarAuthWeighted.sol"
> The AxelarAuthWeighted.sol contract has a medium severity rating due to the absence of a lower bound and higher bound for the new threshold provided for a set of operators at the "_transferOwnership" function. 
>
> The impact of the absence of an upper bound for the provided weight for an operator, which could allow a malicious operator to take over admin privileges. The impact of the absence of a lower bound for the provided new threshold for a set of operators, which could allow a malicious operator to compromise the gateway by taking over admin privileges. It is recommended by Chaintroopers to validate that the new threshold is at least greater than the maximum weight provided for one of the operators to mitigate this risk. 


|![](https://i.imgur.com/oI7UeOp.png)|
|--|
|[Source: [AxelarAuthWeighted.sol](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code)]|

When verifiying the highlighted code snippet in the audit against the [deployed smart contract](https://etherscan.io/address/0x228b92510130ec2E09C6d5645039c8cB834aD42d#code). No changes can be observed, suggesting this issue has not been resolved.


> ###### Chaintroopers |2022-08| C2.5: Insecure error handling of zero addresses at "ReceiverImplementation.sol" and at "DepositReceiver.sol"
> The contracts "ReceiverImplementation.sol" and "DepositReceiver.sol" contain insecure error handling for zero addresses. When the provided "refundAddress" is zero, the contracts replace it with "msg.sender". This can lead to incoming funds being transferred to a contract that may not be designed to handle them, potentially resulting in a lock-up of the incoming funds. The recommendation is to verify that the address is not the zero address and then revert the transaction if it is. The issue is marked as LOW, as the external functionalities are mainly designed to be used by the upgradable AxelarDepositService.

An [example from the DepositReceiver.sol](https://github.com/axelarnetwork/axelar-cgp-solidity/blob/main/contracts/deposit-service/DepositReceiver.sol) suggests that the issue still exists.


|![](https://i.imgur.com/sczubTp.png)|
|--|
|[Source: [DepositReceiver.sol](https://github.com/axelarnetwork/axelar-cgp-solidity/blob/main/contracts/deposit-service/DepositReceiver.sol)]|

It should be noted [the contract presumably containing](https://etherscan.io/address/0xd883C8bA523253c93d97b6C7a5087a7B5ff23d79#code) "DepositReceiver.sol" as a library contract is unverified on etherscan, meaning an on-chain verification was not possible.


#### Repository | axelar-cgp-solidity, axelar-xc20-wrapper | [Code4rena](https://code4rena.com/contests/2022-07-axelar-network-v2-contest)

> ###### Code4rena Contest |2022-07| M-6 : Add cancel and refund option for Transaction Recovery
> The AxelarGateway.sol and AxelarGasService.sol contracts lack the option to cancel and refund transactions that have failed or are stuck, which can potentially result in the loss of user funds. The recommendation is to include a cancel option and enable users to receive gas refunds. While Axelar Network acknowledged the issue, they disagreed with the severity, asserting that it is up to cross-chain app developers to implement refund and cancel methods as they are application-specific. The Code4rena Judge rated the issue as of medium severity due to its risk to end-users and the absence of a refund mechanism. Both the sponsor and judge advised end-users to form their own opinion.

#### Repository | axelar-core, axelar-cgp-solidity, axelar-web-app [Cure53](https://github.com/axelarnetwork/audits/blob/main/audits/2022-04%20Cure53.pdf)

> ###### Cure53 |2022-04| AXE-02-016 WP1: Potential Cosmos consensus stall via panic 
> This issue is a Medium severity vulnerability and involves the EndBlocker handlers of two modules, x/tss and x/reward, which can trigger panic() calls that could potentially cause the Cosmos network to slow down or even halt. Panic() calls are a mechanism in Go that halts the execution of a program when an unexpected error or state is encountered. If an attacker were to trigger these panic() calls remotely, they could exploit this vulnerability to create a Denial-of-Service scenario on the network. To mitigate this issue, it is recommended to integrate recovery logic to all Cosmos module EndBlocker handlers to prevent the chain stall.

Based on abci.go files in [x/tss](https://github.com/axelarnetwork/axelar-core/blob/main/x/tss/abci.go) and [x/reward](https://github.com/axelarnetwork/axelar-core/blob/main/x/reward/abci.go) modules in the axelar-core repository, the issue has not been fixed yet. It should be noted that this may have been addressed elsewhere in the architecture. 

#### Repository | axelar-core, axelar-cgp-solidity, tofn, tofnd, axelarjs-sdk

> ###### Cure53 |2021-12| AXE-01-006 WP1: Insufficient in-memory protection of secret values (Info) & AXE-01-007 WP1: Leakage of secrets via crypto libraries (Low)
> AXE-01-006 and AXE-01-007 are security vulnerabilities discovered in the tofnd-main and tofn repositories. AXE-01-006 concerns the insufficient in-memory protection of secret values, where the mechanism to safely store the password and other secret values in memory only ensures that the memory is zeroed when the affected variables persist out of scope, and thus may result in secrets persisting in memory and leaking to persistent storage. AXE-01-007 concerns the leakage of secrets via crypto libraries, where third-party libraries that do not diligently zeroize secrets handed over to them can lead to leakage, such as in the HMAC implementation in tofn-main/src/crypto_tools/rng.rs, where secret_recovery_key leaks via the Hmac type when the length of the key is less or equal to the output size of the hash function. It is recommended to ensure that all code, including library code, wipes secrets after usage and consider potential scenarios that could incur leakage from memory.

It seems that the problems regarding AXE-01-006 and AXE-01-007 have not been addressed, considering that the [latest commit dates back to 09-2021](https://github.com/axelarnetwork/tofnd/commits/main/src/encrypted_sled/password.rs) and the audit report, which commenced in 12-2021 ([as indicated in the audit table](https://github.com/axelarnetwork/audits)) and concluded in 01-2022 ([as stated in the report footer](https://github.com/axelarnetwork/audits/blob/main/audits/2021-12%20Cure53.pdf))


> ###### Cure53 |2021-12| AXE-01-009 WP2: Mnemonic compromise constitutes single point of failure (Medium)
> The use of a BIP39-mnemonic to generate entropy of 256 bits is a single point of failure and constitutes a medium level risk. This is because an attacker with knowledge of the secret mnemonic would be able to regenerate all previously generated keypairs, as they are all derived from the same seed. This design issue fails to achieve forward secrecy, and compromises the security of all generated keys and associated operations. However, the severity of this issue is mitigated by the requirement for consensus among multiple nodes in the Axelar network, making it difficult for an attacker to achieve tangible harm without compromising the mnemonic of enough nodes to achieve voting power. The affected file is tofnd-main/src/multisig/keygen.rs, where the same mnemonic entropy is used to generate key pairs.

Similar to AXE-01-006 and AXE-01-007, it seems that the problems regarding AXE-01-009 have not been addressed, considering that the latest commit dates back to the [end of 11-2021](https://github.com/axelarnetwork/tofnd/commits/main/src/multisig/keygen.rs) and the audit report, which commenced in 12-2021 ([as indicated in the audit table](https://github.com/axelarnetwork/audits)), concluded in 01-2022 ([as stated in the report footer](https://github.com/axelarnetwork/audits/blob/main/audits/2021-12%20Cure53.pdf)). This issue should be read bearing in mind a sizable threshold level of 35 validators. 

