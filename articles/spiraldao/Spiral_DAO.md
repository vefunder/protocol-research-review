# Asset Risk Assessment: SpiralDAO (COIL)

### Analyzing Spiral DAO: A Novel Approach to Yield Farming, Bribe Markets, and veTokenomics

![](https://i.imgur.com/v1kjozY.png)


## Links

- [Website](https://spiral.farm) 
- [Documentation](https://docs.spiral.farm)
- [Twitter](https://twitter.com/Spiral_Dao)
- [Medium blog](https://medium.com/spiral-dao)
- [Discord](https://discord.com/invite/spiraldao)
- [Snapshot](https://snapshot.org/#/spiralgov.eth)


## Relation to Curve

On April 5th, Spiral DAO introduced a [COIL/FRAXBP](https://curve.fi/#/ethereum/pools/factory-crypto-236) V2 factory pool on Curve, which includes COIL, FRAX, and USDC. The pool currently holds approximately $1.8 million in assets was initially [bootstrapped by the SpiralDAO Treasury](https://etherscan.io/tx/0x64a21d845769e774b6a39fa7706a8299d9e24a124c0c739711d1d46af9582d0d). A [gauge proposal](https://gov.curve.fi/t/proposal-to-add-coil-fraxbp-to-the-gauge-controller/9119) was submitted on April 13th. An [on-chain vote](https://dao.curve.fi/vote/ownership/314) concluded on March 23rd in favor of the proposal.

Approximately 8% of the treasury funds are presently allocated to providing liquidity to the COIL/FRAXBP pool. Spiral DAO remains the primary liquidity provider of this pool, and earns over 100% APY by staking their LP token into the COILFRAX-f Gauge. Spiral DAO also participates in Curve Governance, currently holding 1,415,869 sdCRV (Stake DAO). 

This report aims to investigate the inner workings of Spiral DAO, review its design trade-offs, and examine the potential implications in the [ongoing curve wars](https://www.defiwars.xyz/wars/curve) and global DeFi bribes ecosystem.


## Key Findings

* Access control for most system contracts are with the [4-of-7 protocol multi-sig](https://etherscan.io/address/0xF14eFC7E46D57E107dEE97239329Bd7F56361C38). This multi-sig controls critical functionality and is responsible for user deposits in its yield bonding strategy and protecting against an infinite mint of COIL and SPR tokens.
* The COIL/FraxBP pool is almost entirely POL, as the COIL tokenomics incentivize users to stake their COIL for SPR. Spiral DAO uses bribes and its own governance power to increase CRV emissions to its pool, which is currently earning >100% CRV APY on $1.8M of liquidity. 
* The protocol borrows many mechanics from Olympus DAO, a well-proven codebase with numerous forks.
* There is no cap on COIL and SPR tokens; while Spiral DAO aims to grow its Treasury backing quickly enough to offset emissions for stability and sustainability, caution should be exercised due to the model’s reliance on momentum that carries risk of failure.
* COIL and SPR holders depend on the performance of Spiral DAO’s [Treasury](https://debank.com/profile/0xC47eC74A753acb09e4679979AfC428cdE0209639), which is exposed to smart contract risk from several different protocols (Aura, StakeDAO, Convex, Balancer, Curve, Silo etc.).
* SpiralDAO is composed of mostly anonymous contributors who are seasoned DeFi natives and have attracted significant demand through their Initial Treasury Offering (ITO) and have partnered with StakeDAO.
* The governance is reasonably decentralized for an early stage project, but leaves much to be improved. SPR is used in Snapshot voting, multi-sigs are set with reasonable thresholds, and owners of the multi-sigs are all disclosed (though many are pseduonymous). However, several contracts have EOA's in privileged roles and timelocks are not used. 


## Introduction to Spiral DAO

Spiral DAO is a capital-efficient yield aggregator designed to optimize yield farming and bribe markets across applications that employ veTokenomics (ie. Curve and Balancer). Unlike traditional aggregators such as Yearn and Beefy, SpiralDAO retains reward tokens from third-party protocols within the DAO treasury, reducing sell pressure and circulating supply. While the protocol borrows much of its smart contract design from Olympus DAO, it introduces a novel concept of "Yield Bonding". The protocol accepts LP token deposits to farm rewards to its treasury and issues an excess value of its native token to depositors as compensation.

The two-token model of Spiral DAO consists of the COIL and SPR tokens. COIL is an inflationary token primarily useful for staking as SPR. The vast majority of unstaked COIL supply is protocol-owned liquidity (POL) in the Curve and Balancer pools. The rebasing SPR token is a governance token that incentivizes liquidity providers to contribute their yields to the Spiral DAO treasury through Yield Bonding. This structure enables users to obtain SPR governance rights while safeguarding them from COIL’s inflationary nature. By distributing a greater yield in SPR tokens than the rewards attainable via existing protocols, Spiral DAO incentivizes user engagement. It aims to grow its treasury backing fast enough to offset emissions, ensuring the stability and sustainability of the protocol. 

Spiral DAO claims to support the DeFi ecosystem by reducing sell pressure on farming tokens such as CRV and BAL, offering improved risk-adjusted yields for farmers, and optimizing the bribe market. 


### The Launch of Spiral DAO

Spiral DAO was launched in March 2023 through an [Initial Treasury Offering (ITO)](https://spiral.farm/ito), aiming to accumulate a large share of USDC, CRV, BAL, FXS, and SDT tokens. The protocol was bootstrapped by an initial oversubscription auction for 2.6m COIL tokens between April 3rd and 5th. 

The initial timeline for the Spiral DAO launch:

![](https://i.imgur.com/3g1Sq69.png)

Source: [Twitter](https://twitter.com/Spiral_DAO/status/1642530107112955904?s=20)

The [ITO](https://spiral.farm/ito) resulted from the Treasury holding the following position:

* 4,240,516 USDC ([tx](https://etherscan.io/tx/0x3f087003b3cf627620358408e26bae8201532406e0fca93f9aa90f6029b045b1))
* 956,810 CRV ([tx](https://etherscan.io/tx/0xf31380dafd073532509a73427cdfcefb0742e278280a54c2423d8be6a0db5fce))
* 114,797 BAL ([tx](https://etherscan.io/tx/0x897aa59415558ddb2fdfae067683e647a268a5dbe6af5c397d27f04aa37a1e2b))
* 37,391 FXS ([tx](https://etherscan.io/tx/0xa6f1e64828f52105ba627156140fe168156e4f832cbf674600d26805cf3f0604))
* 561,657 SDT ([tx](https://etherscan.io/tx/0xd303b3f1b1e50c4b870b5ac1b3b686da23d5316470ede92e366b5d542f599294))

Unpurchased COIL from the auction was [burned](https://etherscan.io/tx/0xf3e179e213285fca12fc0c5c4e39d7923414a589514f6dd098677752dff9b5e1) and initial liquidity was [seeded](https://etherscan.io/tx/0x64a21d845769e774b6a39fa7706a8299d9e24a124c0c739711d1d46af9582d0d) to the COIL/FraxBP Curve pool with 730,360 USDC and 235,600 COIL.

The protocol then deployed its Protocol Owned liquidity (POL) to various other protocols, including Aura Finance, Convex, Conic, Aave V3, Balancer V2, and Silo Finance. It acquired an initial stake of [955,854 sdCRV](https://etherscan.io/tx/0x1190a7321495a17940674761e67c4faa222514dfecfe9a1f2422bc631b3bc165) to participate in Curve gauge voting. It has since increased its stake to 1,415,869 sdCRV as of this writing.

A quick overview of the current treasury exposure to various protocols can be found on [DeBank](https://debank.com/profile/0xc47ec74a753acb09e4679979afc428cde0209639).

![Screen Shot 2023-05-11 at 10 07 35 AM](https://github.com/vefunder/protocol-research-review/assets/51072084/c0c6404e-02a9-444e-b505-e811a756d1d8)

Source: [DeBank](https://debank.com/profile/0xc47ec74a753acb09e4679979afc428cde0209639)

### Airdrop
To encourage adoption, Spiral DAO plans to hold a two-phase Airdrop:

- Phase 1 was initially planned right after the Initial Auction. It was postponed by a few weeks (via [snapshot vote](https://snapshot.org/#/spiralgov.eth/proposal/0xac38e03dea8ab9d4b1d0d5a98af848e9faa6a67ff63959700f7648f410a8a21a)) to allow sufficient time to prepare the marketing campaign and distribute the tokens over a broader user base. Phase one started on April 29th, with 3% of the initial token supply distributed towards Curve/Convex, Balancer/Aura Finance, FRAX Finance/StakeDAO, and 1,000 DeBank users as DegenScore Beacon NFT owners.
- Phase 2 will be distributed among the most active community participators. 

## Spiral DAO, a new governance token farming primitive
Spiral DAO aims to optimize DeFi ecosystem components such as yield farming, bribe markets, and veTokenomics. Diverging from traditional yield aggregators, Spiral DAO retains third-party protocol reward tokens within its Treasury, mitigating selling pressure and circulating supply. Spiral employs a two-token model with COIL primarily for staking and SPR tokens as rewards, incentivizing liquidity providers to contribute yields to the Treasury through its protocol's owned liquidity (POL) and a concept called "yield bonding."

### The dual COIL/SPR tokens model

#### 1- COIL Token: a share of the Treasury assets

The [COIL token](https://www.coingecko.com/en/coins/spiraldao-coil) is an 18-decimal ERC-20 token representing a user's share in the growing DAO treasury. Native COIL is dispensed at preferential rates as a reward in exchange for other yield-bearing LP tokens (e.g. [B-auraBAL-STABLE](https://etherscan.io/token/0x3dd0843a028c86e0b760b1a76929d1c5ef93a2dd), [pax-usdp3CRV-f](https://etherscan.io/token/0xc270b3b858c335b6ba5d5b10e2da8a09976005ad), etc.). It can be redeemed directly with the protocol's `SpiralRedeem` contract or via a [Balancer 50/50 pool](https://app.balancer.fi/#/ethereum/pool/0x42fbd9f666aacc0026ca1b88c94259519e03dd67000200000000000000000507), with liquidity being provided by Spiral DAO for both.

COIL can be staked to received SPR, which grows periodically via a "share price" logic, meaning that the exchange rate is ever-increasing based on the value of the underlining treasury assets. COIL has an unlimited supply and is highly inflationary. The tokenomics of Spiral DAO addresses dilutionary pressure via wrapped COIL (SPR tokens). This structure protects users from COIL's inflation while offering a derivative outperforming staked and non-staked veTokens. 

#### 2- SPR Token: the yield-bearing asset
SPR tokens are obtained by staking COIL. They represent the governance power of the assets held by Spiral's Treasury. The token is distributed to users that stake their LP tokens on Spiral Dao. SPR can be unwrapped to the native COIL asset via the `SpiralStaking` smart contract at an exchange rate (`index` variable). Spiral DAO plans on having the option to lock SPR in the future to allow for outsized yields in the likes of veTokens.

With time, treasury gains in expected to outpace the SPR emitted:
![](https://i.imgur.com/nHyeYM4.png)
https://docs.spiral.farm/protocol/tokenomics

## Protocol Mechanics
Spiral DAO uses several mechanisms first introduced and popularised by [Olympus DAO](https://www.olympusdao.finance/) (OHM), such as rebasing, treasury management, and bonding. Spiral DAO addresses issues from OHM-like protocols by tackling a key concern: the possibility that the executive team could "slow-rug" and passively underperform, ultimately putting the token value below the Treasury backing.

### Yield Bonding
Yield Bonding works similarly to classic bonding but distributes the native token COIL with additional yield as a discount. The benefits of Yield Bonding include securing new tokens upon minting, easy onboarding for new users, and potentially setting a new reward token standard.

Users receive boosted Spiral (SPR) token yields by staking via Spiral DAO. The additional yield rates are adjusted daily based on the COIL market cap and the Treasury value. The platform has mechanisms to ensure buybacks when the COIL market cap falls below 90% of the Treasury's value, maintaining an incentive to stake.

Both COIL and SPR tokens are inflationary, but the Treasury backing creates a natural bottom line for the tokens via its redemption page. Yield Bonding only works when the COIL market cap is significantly larger than the Treasury value, making it a deflationary measure. Spiral DAO's Treasury grows exponentially, outpacing the natural inflation of the token and corresponding reward tokens.

Spiral DAO aims to resolve issues faced by competitors, such as inflation, cannibalization, lack of steady supply, poor fund utilization, and overexposure to one token. By capturing inflation from relevant protocols and managing rewards to generate additional value, Spiral DAO seeks to rationalize the bribes markets and DeFi in general.

### Rebase
Rebasing is the process of minting new COIL tokens paid to stakers. For Spiral DAO, this is done via `Rebase,` a permissionless function in the `Staking` contract called automatically at each staking/unstaking event without relying on off-chain scripts. This differs from other protocols that may require permissioned actions or off-chain scripts to perform rebases.

![](https://i.imgur.com/DThKgBa.png)
https://etherscan.deth.net/address/0x6701E792b7CD344BaE763F27099eEb314A4b4943

### Redemption and exit liquidity
Spiral DAO aims to offer an additional exit strategy for users through its redemption page; this page uses the USDC treasury reserve and provides users with the option to exit the DAO by redeeming 90% of the Treasury backing value of COIL. 

[`SpiralRedeem`](https://etherscan.io/address/0x4fe67fd442889d158c311de734f45339ed9f3db3) holds a certain amount of Spiral DAO Treasury's USDC, with a `penalty` variable (from 0 to 100%) that can be changed by the owner (Spiral DAO). At the time of writing this report, the contract held 312,000 USDC against a total supply of 4,091,200 COIL, worth approximately $9,8m.

![](https://i.imgur.com/1REuxzT.png)
https://etherscan.deth.net/address/0x4fe67fd442889d158c311de734f45339ed9f3db3

Although it is typically irrational for users to redeem (since it's more advantageous to sell via Curve's [COIL/FRAXBP](https://curve.fi/#/ethereum/pools/factory-crypto-236) Factory pool), the redemption page serves as an ultimate price floor in specific situations, ensuring users have a secure exit strategy.

The Treasury composition of Spiral DAO is said to ensure ample exit liquidity for every user should they choose to leave the protocol, even if the COIL price trades below its backing. The Spiral DAO Treasury is structured to allocate a portion of its reserves to prevent the COIL price from falling more than 10% below its actual backing, enabling the protocol to arbitrage Spiral tokens when their value dips below the fair value. 

### Project Own Liquidity (POL) and Treasury
Spiral DAO's Treasury deploys liquidity across various diversified strategies, using multiple governance tokens and stablecoins to support host protocols and optimize yields for token holders and users. The unique treasury rebalancing mechanism and tokenomics enable Spiral DAO to exploit bribe market inefficiencies, enhancing bribe yields and liquidity depth.

The Treasury Management in Spiral DAO pursues three primary objectives:

* Sustaining market share
* Ensuring balanced Treasury exposure
* Providing fair exit opportunities for users

Spiral DAO strives to increase the market share of relevant tokens continually. For instance, if the DAO attains a 10% market share of a token, it aims to raise its share and prevent it from dropping below 10% unless decided otherwise by the community.

## Rebalancing Mechanism
Spiral DAO's innovative Treasury rebalancing mechanism plays a crucial role in maintaining a balanced Treasury exposure by taking advantage of bribe markets for arbitrage purposes.

For example, if the DAO intends to increase the BAL portion in the Treasury while decreasing CRV, it proceeds as follows:

1. Sell CRV votes in the bribe market to acquire stablecoins
2. Use those stablecoins to buy votes in the Aura/BAL bribe market
3. Allocate the acquired votes to the POL gauge
4. Receive additional BAL/AURA emissions

The rebalancing mechanism is designed to adjust the Treasury's holdings according to market dynamics and the Spiral community's preferred risk profile. Specifically, it increases the exposure to BAL-AURA tokens, thereby relatively increasing their weighting in the Treasury. This process does not reduce the exposure to CRV tokens but rather adjusts the balance in favor of BAL-AURA. This ensures fairness and inherent liquidity for Spiral tokens while adapting to changing market conditions.

![](https://i.imgur.com/ZhNTw4S.png)
https://docs.spiral.farm/protocol/bribes-market

### Treasury composition
Guidelines for future treasury composition were proposed and voted on via a [snapshot vote](https://snapshot.org/#/spiralgov.eth/proposal/0x2c83dbf3dd2a77d9eb71978d8cae41f17567b0213c916bc2970810fbc4671846). A core contributor presented a plan addressing future token weights in the DAO, focusing on two main aspects: 1) setting a target treasury composition to enable rebalancing using various methods, and 2) determining the handling of byproducts and additional rewards, such as GEAR tokens.

The proposal suggests modifying the existing treasury composition over the coming months to increase exposure to CRV/BAL and potentially cultivate external partnerships. Furthermore, the proposal advises adopting a default practice of retaining, rather than selling, other tokens until they account for less than 1% of the Treasury. At this point, a reevaluation would occur. This strategy aims to prevent protocol cannibalization while maximizing potential yields.

The Treasury composition (including Redeem contract), as of early May 2023, is as follows:

![](https://i.imgur.com/qFnzZQC.png)
Source of data: [etherscan](https://etherscan.io/)

The target asset mix to be achieved in the next few months is as follows:
* 45% Stables/USDC
* 20% CRV/CVX
* 15% BAL/AURA
* 6% FXS
* 4% SDT
* 10% Other (ETH + BTC + Partnership assets)

The DAO plans to submit any significant changes to a snapshot vote, whether it involves increasing, decreasing, or maintaining a specific asset exposure. Concerning Spiral's risk framework, they intend to have the following ratios: ~10-15% for potentially risky assets, 20-30% for medium-level risks, and 50-70% for a safe risk profile. Proactive measures are taken, and continuous monitoring is conducted; for instance, Spiral eliminated Conic finance exposure due to its excessive size and experimental nature.

### Other Gauge (Balancer)
Pool: https://app.balancer.fi/#/ethereum/pool/0x42fbd9f666aacc0026ca1b88c94259519e03dd67000200000000000000000507
Gauge Proposal: https://snapshot.org/#/balancer.eth/proposal/0x89cb22ba735a4f5e145648306f381816759fbec5200a4c482b8c6e06c82b9d83

Spiral DAO proposed a new gauge for the COIL/USDC pool on Balancer. The voting concluded with a unanimous decision of 100% support (3.6M veBAL) in favor of the proposal. The quorum requirement of 2M veBAL was met, and the new COIL/USDC pool gauge will be implemented.

### Emissions
Spiral DAO's protocol is designed to incentivize users to maintain their investment by ensuring that, under normal conditions, the value of emitted SPR tokens surpasses the treasury backing. The protocol undergoes periodic weight updates for new SPR token emissions, adjusting them to achieve an emission value equal to 100% of the underlying yields plus 40% of the "additional overValue" parameter. This approach aims to attract more yield farmers by offering a higher APR than a mere 5% discount on SPR tokens.

The selection of the 40% proportion is grounded in achieving a balanced dynamic between the Treasury, which secures approximately 60% of the overvalued COIL market cap, and individual yield stakers, who benefit from 40% of this via additional yield. While this percentage is not immutable and could be altered based on community proposals, the protocol intends to introduce pools without this mechanism. Consequently, the yield boost will only apply to select, reliable assets such as those in the Curve-Bal ecosystem.

To better accommodate emissions across all pools, weight updates for SPR emissions transpire every few days. The calculation of SPR rewards is based on 1 ETH of LP, which bolsters the protocol's robustness amidst TVL fluctuations.

Spiral DAO has implemented various mechanisms to safeguard against potential faulty or malicious behavior during weight updates and intends to introduce additional measures. One such mechanism involves limiting the overall amount of SPR rewards distributed, effectively limiting potential funds misappropriation to approximately $100k worth of SPR.

Occasionally, emissions may exceed the treasury backing. This scenario may materialize if, for instance, the price increases during the day between weight updates while the protocol remains below the treasury backing and emits an equivalent amount in value. It is worth noting that this disproportionality operates bidirectionally, resulting in users receiving reduced dollar value rewards, negating negative impacts.

### Protocol Revenue Model
Spiral DAO's revenue model employs several strategies to generate income. The primary source is yield farming through staking treasury assets like CRV, BAL, CVX, and AURA. Additionally, the protocol takes advantage of arbitrage opportunities in bribe markets to acquire discounted tokens, considered revenue.

![](https://hackmd.io/_uploads/H16UeKrV2.png)

Spiral DAO also plans to participate in other protocols' Initial Liquidity Offerings to diversify its revenue sources and expand into aggregating veCRV boosts. The Treasury receives a 10% fee upon redemption and charges 0.5 per COIL emitted, contributing to the overall revenue.

By exploiting inefficiencies in bribe markets and deploying a sizeable portion of its Protocol-Owned Liquidity (POL), Spiral DAO aims to maximize its revenue streams, benefiting its users. The protocol plans to develop a Dune dashboard to track the efficiency of bribe markets and make informed decisions.

## Stake DAO Partnership
Spiral DAO has partnered with Stake DAO to utilize their liquid lockers for various tokens in the Treasury and include SDT in their Initial Treasury Offering. The collaboration also involves adding a Stake DAO member to Spiral DAO's multi-sig. The primary goal of this partnership is to foster mutual growth within the DeFi ecosystem.

Spiral DAO focuses on accumulating significant shares in DeFi blue chips like CRV, BAL, and FXS through Yield Bonding. Users deposit yield-earning assets and receive COIL in place of their underlying rewards, which are then deposited into yield-earning strategies in the Treasury. By incorporating SDT into their Initial Treasury Offering, Spiral DAO aims to enhance cooperation with Stake DAO and support the growth of the DeFi ecosystem.

As the partnership progresses, both platforms expect to see an increase in SDT supply, which will be used to boost liquid lockers as the treasury value grows. The acquired SDT will be locked, reducing its circulating supply. The proposal for a COIL+FRAXBP Gauge can be [found here](https://votemarket.stakedao.org/advanced/crv/0x0000000895cB182E6f983eb4D8b4E0Aa0B31Ae4c/0
).


## Governance

### Team

While most contributors are anonymous, the team behind the project possesses diverse backgrounds in the crypto industry, including market making on CEXs, quantitative analysis, angel investing, arbitrage, and yield farming. 

Several contributors have been involved in the space since 2012-2013 and have previously built projects such as the [Dracula](https://defillama.com/protocol/dracula) yield farming aggregator (2020). The team also benefits from the advice and guidance of notable figures in the DeFi space, such as the founder of cp0x (validators in multiple DeFi applications) and Sami (Redacted Cartel founder). A core contributor was also involved in a white hat attack in 2021, where over $1m of assets were returned.

We asked Spiral DAO for more information about contributors' roles. We were told that the DAO plans to make its structure more transparent at a later stage. Below is a partial list of contributors based on the latest information obtained:

Contributors:
* VaeVictis
* Cuttlefish
* Ivan

Advisors:
* [Farmer Brown](https://twitter.com/FarmerBrownDeFi) - Advisor
* SuperChad - Advisor
* [Starny](https://starny.eth.limo/)
* [cp0x](https://debank.com/profile/0x6f9bb7e454f5b3eb2310343f0e99269dc2bb8a1d)
* [CIA Officer](https://twitter.com/officer_cia) - Security researcher
* [Sami](https://twitter.com/0xSami_) - Founder of Redacted Cartel
* [Valentin Mihov](https://twitter.com/valentinmihov) - Co-founder of Daedalus


### Gov Process and Voting

Governance proposals can be made on the [Governance channel](https://discord.com/channels/1054724431404089445/1093490553120903261) of the Spiral DAO Discord server. According to guidelines in the [docs](https://docs.spiral.farm/dao/governance-process), a proposal should be active for at least 24 hours before going to a vote. Voting is done [via Snapshot](https://snapshot.org/#/spiralgov.eth) with votes being recognized based on SPR holdings.

As Snapshot votes are non-binding, in most cases the protocol or treasury multi-sig will be responsible for executing the outcome of the vote.


### Multi-sigs

#### [Treasury](https://etherscan.io/address/0xc47ec74a753acb09e4679979afc428cde0209639) (3/6)

The treasury multi-sig manages the protocol treasury:

* [`GnosisSafeProxy.sol`](https://etherscan.io/address/0xC47eC74A753acb09e4679979AfC428cdE0209639): Treasury Multisig - 3-of-6 Gnosis Safe 1.3.0 (behind proxy)
     * VV - [0xC011240FAb026E08EC18E12a506f45b9aaEE84dD](https://etherscan.io/address/0xC011240FAb026E08EC18E12a506f45b9aaEE84dD)
     * Cuttlefish - [0xC541A7b893eFD384d3E0013DfCb3e563a777fDBC](https://etherscan.io/address/0xC541A7b893eFD384d3E0013DfCb3e563a777fDBC)
     * [Farmer Brown](https://twitter.com/FarmerBrownDeFi) - [0xFBD9123f3CA030632C5fC5948dfebb38B0b115f2](https://etherscan.io/address/0xFBD9123f3CA030632C5fC5948dfebb38B0b115f2)
     * SuperChad - [0xDD9bF0A45452a4F22cfd2C963c15B191D97Ce106](https://etherscan.io/address/0xDD9bF0A45452a4F22cfd2C963c15B191D97Ce106)
     * [Starny](https://starny.eth.limo/) - [0x79603115Df2Ba00659ADC63192325CF104ca529C](https://etherscan.io/address/0x79603115df2ba00659adc63192325cf104ca529c)
     * [cp0x](https://debank.com/profile/0x6f9bb7e454f5b3eb2310343f0e99269dc2bb8a1d) - [0x53BD3C14e2a076EB55a7e2BE73Fa0fA7918CF300](https://etherscan.io/address/0x53BD3C14e2a076EB55a7e2BE73Fa0fA7918CF300)

#### [Protocol](https://etherscan.io/address/0xf14efc7e46d57e107dee97239329bd7f56361c38) (4/7)

The Protocol owner multi-sig controls the majority of protocol contracts including `MasterMind`, `SpiralStaking`, and the token contracts:

* [`GnosisSafeProxy.sol`](https://etherscan.io/address/0xF14eFC7E46D57E107dEE97239329Bd7F56361C38): Protocol Multisig - 4-of-7 Gnosis Safe 1.3.0 (behind proxy)
     * VV - [0xC011240FAb026E08EC18E12a506f45b9aaEE84dD](https://etherscan.io/address/0xC011240FAb026E08EC18E12a506f45b9aaEE84dD)
     * Ivan - [0xc0cbd3a183E634f630F4658526A1D8309008c6b8](https://etherscan.io/address/0xc0cbd3a183E634f630F4658526A1D8309008c6b8)
     * [Farmer Brown](https://twitter.com/FarmerBrownDeFi) - [0xFBD9123f3CA030632C5fC5948dfebb38B0b115f2](https://etherscan.io/address/0xFBD9123f3CA030632C5fC5948dfebb38B0b115f2)
     * [cp0x](https://debank.com/profile/0x6f9bb7e454f5b3eb2310343f0e99269dc2bb8a1d) - [0x53BD3C14e2a076EB55a7e2BE73Fa0fA7918CF300](https://etherscan.io/address/0x53BD3C14e2a076EB55a7e2BE73Fa0fA7918CF300)
     * [CIA Officer](https://twitter.com/officer_cia) - [0xB25C5E8fA1E53eEb9bE3421C59F6A66B786ED77A](https://etherscan.io/address/0xb25c5e8fa1e53eeb9be3421c59f6a66b786ed77a)
     * [Sami](https://twitter.com/0xSami_) - [0x2AFb6A75Ee833A84d534C62894BB348234b5d219](https://etherscan.io/address/0x2AFb6A75Ee833A84d534C62894BB348234b5d219)
     * [Valentin Mihov](https://twitter.com/valentinmihov) - [0x3F0c2A4654A63110ED3b15cF9C0fa07476B389cF](https://etherscan.io/address/0x3F0c2A4654A63110ED3b15cF9C0fa07476B389cF)  


## Smart Contracts Overview

### Contract Architecture Overview
Spiral DAO's smart contract architecture is designed to efficiently and securely incentivize users to stake in selected pools while maintaining necessary relative inflation. The architecture includes a `Staking` contract based on Olympus DAO, minting ERC20 tokens 'COIL' and 'SPL' through vaults. The 'MasterMind' contract is a reworked single contract-based yield aggregator supporting various protocols and comprises three distinct roles: `Service`, `Drainers`, and `Owner`.

The `Service` role handles lower-level threats and non-impactful activities, while the `Drainers` can take corresponding rewards (CRV/BAL/other) to the DAO/Treasury address. The `Owner` role, Spiral DAO's multi-sig, can add pools, change delegate-call adapters, and increase withdrawal fees.

`MasterMind` features two types of "Adapters": `XXXAdapter.sol`, consisting of hardcoded routes for zaps and view information of reward tokens for drains, and `XXDelegate.sol`, which holds the code `MasterMind` used for delegate calls of different protocols. The latter is crucial for security reasons. The `Rewarder` contract determines the token distribution for each user based on the allocated rewards per 1 LP token per pool. Reward rates are updated by the admin role, guided by off-chain scripts that calculate the price of Coil, collateral protocol yield, and the required APR to sustain per 1 LP for the target pool in `COIL`.

### Deployed Contracts

The protocol has two tokens and a staking contract for the tokens:

* [`SPR.sol`](https://etherscan.io/address/0x85b6ACaBa696B9E4247175274F8263F99b4B9180): Protocol wrap token (ERC20), 18 decimals
* [`COIL.sol`](https://etherscan.io/address/0x823E1B82cE1Dc147Bbdb25a203f046aFab1CE918): Reward token (ERC20), 18 decimals
* [`SpiralStaking.sol`](https://etherscan.io/address/0x6701E792b7CD344BaE763F27099eEb314A4b4943): Deposit COIL to get SPR (similar to OHM staking with rebase, but no forfeit functionality)

The following contracts are related to the `MasterMind` contract, responsible for managing AMM farming pools and user deposits into those pools:

* [`UpgradableProxy.sol`](https://etherscan.io/address/0xFACE8DED582816E2f2cD4C6cc1cbD1aCCc9df65E): MasterMind Proxy (OpenZeppelin fork)
* [`MasterMind.sol`](https://etherscan.io/address/0x087fd5d07907f864285dbd94acef8cfb5bd26804): Current implementation
* [`ProxyAdmin.sol`](https://etherscan.io/address/0xae3f25462c06d21483738832b7160fae0ebf4f60): Proxy Admin (OpenZeppelin fork) of MasterMind proxy contract
* [`Rewarder.sol`](https://etherscan.io/address/0x72614b5d6F388B089f343723fcc3a5B4Fc22B347): Multi-pool rewarder contract that sets fees and reward rates for depositors. This contract is set by MasterMind.
* [`RewarderVault.sol`](https://etherscan.io/address/0x31878EE03D3DeA8C3a81b78fddc864c0be6f415F): Executes mints for Rewarder with SPR reward token.

Additional functionality include the SpiralSwap router and the SpiralRedeem contract:

* [`Router.sol`](https://etherscan.io/address/0x0340d9491Fe7740aF9c643C3C2b4126d23058C3B): SpiralSwap router is a COIL liquidity aggregator that allows swaps between COIL/SPR and USDC.
* [`SpiralRedeem`](https://etherscan.io/address/0x4fe67fd442889d158c311de734f45339ed9f3db3): Redeem contract allows users to redeem COIL for treasury USDC with a configurable penalty. Penalty is currently 10%



## Access Control

We have reviewed the access control for Spiral DAO's deployed contracts. You can find the [details here](https://docs.google.com/spreadsheets/d/1FWMGGJIasCCaoircLtsBKiG4xnhr4hlf-JHlRSZANaU/edit#gid=0). Below are the key points:

- The majority of contracts are owned by the 4-of-7 protocol multi-sig, which has extensive privileges to upgrade contracts, add and remove privileged addresses, and is generally responsible for critical system functionality.
- It is possible to infinite mint COIL and/or SPR in case of irresponsible or malicious action taken by the owner multi-sig. The protocol multi-sig must ensure that the vault address is not set to an exploitable address. It can call `setVault()` to set an arbitrary address as a designated vault, which then has the power to call `mint()` to the token contract. Currently the `SpiralStaking` and `RewarderVault` contracts are set as vaults, and these have protected conditions for minting.  
- The [Rewarder contract](https://etherscan.io/address/0x72614b5d6F388B089f343723fcc3a5B4Fc22B347) is controlled by an [EOA](https://etherscan.io/address/0x6E2e85Ee5bB7b4a85e904F1e0eD5b9C7b08e5384). It is responsible for setting pool parameters catalogued within the `MasterMind` contract and distributing fees from the `RewarderVault`.
- The `SpiralRedeem` contract is owned by an [EOA](https://etherscan.io/address/0x6E2e85Ee5bB7b4a85e904F1e0eD5b9C7b08e5384), which can set parameters for redeeming COIL for USDC. It can also withdraw all USDC deposited in the contract (The contract is periodically topped up by the protocol treasury as seen [here](https://etherscan.io/tx/0x02849d8f22bd5780de4a49075e07a99243c7b354b1975e2d680340d3e58d97a0)).
- All Spiral farming pool deposits are through the `MasterMind` contract, which is upgradeable by the 4/7 multi-sig. Depositors in the Spiral pools must trust the contract owner to responsibly custody funds in the Spiral farming pools. 

### Timelock
Spiral DAO opts not to use a Timelock for crucial protocol-related matters. Instead, the project employs a 4/7 multi-sig approach involving only two contributors. This decision stems from the challenges associated with gathering all signers for approval and the desire for signers to maintain control and cross-verification of all activities. While this approach may increase efficiency, it is essential to ensure adequate security measures and checks are in place to prevent potential risks.

### Audits
An [audit report](https://github.com/pessimistic-io/audits/blob/main/Spiral%20DAO%20Security%20Analysis%20by%20Pessimistic.pdf
) from Pessimistic was made public in January 2023. The audit of Spiral DAO's smart contracts revealed one critical issue concerning deleting a logic contract via `delegatecall`. Several medium-severity issues were identified, such as outdated addresses, overpowered roles, test issues, incorrect variable values, skipped operators, and inaccurate reward calculations. Additionally, multiple low-severity issues were found. 

Spiral Dao confirmed that all findings had been addressed. Some changes and additions (e.g., the `SpiralRedeem` contract) appear to have been made after the audit. 

## Risk Analysis
Given that Spiral DAO introduces novel mechanics on top of the Olympus DAO model, it is essential to analyze its potential risks.

### Underlining treasury assets
The COIL & SPR tokens represent a share of all Spiral DAO's Treasury assets. As such, we've examined the risks to which token holders are exposed. These tokens carry the associated risks of every asset the Treasury holds, including the potential fluctuations in the value of these assets, exposure to market volatility, and any other risks inherent to the individual assets themselves. 

### Liquid Lockers Depeg
The potential for de-pegging in DeFi liquidity lockers, such as AuraBal, presents a risk that stakeholders must be aware of, as it can disrupt Spiral DAO's underlying holdings. Users should consider the mechanics specific to each asset and understand the implications of a de-pegging event on the Treasury's value. 

### Smart Contract Risks
Smart contract vulnerabilities pose risks for Spiral DAO, especially since it is unclear which contracts were changed post-audit. To counter potential threats, the project offers a bug bounty program with rewards up to $250k or 15% of the affected funds for critical findings. Furthermore, Spiral DAO has recently voted to launch a bounty program with [Hats.finance](https://hats.finance/) with the goal to incentivize responsible vulnerability disclosure for Spiral DAO. As per the [Snapshot vote](https://snapshot.org/#/spiralgov.eth/proposal/0x9852988e836c847d1bf52e303712411a0230af0c900d866f1ce6c93a2dfaa743), the protocol treasury sent 30K SPR, currently valued ~$77k at this [tx](https://etherscan.io/tx/0x5a0d24d172e5e6aa00c848fb7270e8fc22f53e5f35889466d73e1043d4a5a6d1).

### Potential Bank Run and Redeem limitations
While Spiral DAO has implemented a backstop mechanism for redemption, there remains a risk of COIL becoming illiquid. The USDC balance in the SpiralRedeem contract is overseen by the protocol's multi-sig. The majority of the Treasury's assets are liquid, with the exception of approximately $50k in locked SDT. Although COIL's market cap could decline below the Treasury's value, the redemption smart contract serves as a safeguard by ensuring the drop does not exceed 10% of the total.

However, there are certain limitations to the redemption mechanism that should be taken into consideration. First, if a large number of users choose to redeem their COIL tokens simultaneously, the USDC reserve in the SpiralRedeem contract could be depleted, resulting in reduced exit liquidity for remaining users. This scenario could potentially trigger a bank run-like event, where users rush to redeem their tokens before the reserve is exhausted.

Second, the redemption penalty rate, which can range from 0 to 100%, is controlled by the Spiral DAO. While this flexibility allows the protocol to adapt to varying market conditions, it also introduces uncertainty for users regarding the actual redemption rate they will receive upon exit.

To mitigate these risks, it is essential for Spiral DAO to maintain a sufficiently sized USDC reserve in the SpiralRedeem contract, as well as implement clear guidelines on how the redemption penalty rate will be determined and adjusted. 

![](https://i.imgur.com/Bp3vXM3.png)
https://spiral.farm/redeem

## Potential positive impacts
Adding Spiral DAO's liquidity pools to the Curve ecosystem is expected to positive impact. With the small flywheel built into the system, overall bribe yields for all veCRV holders are likely to increase, while sell pressure on CRV should decrease and future emissions buy-pressure should increase. Furthermore, Spiral DAO's distributed solutions and potential to attract many veCRV holders may benefit the future of veCRV wars. Overall, Spiral DAO's liquidity lockers have the potential to be an excellent addition to the Curve ecosystem.

### Conclusion
In summary, Spiral DAO offers an innovative approach to yield farming, bribe markets, and veTokenomics in the DeFi ecosystem. With a robust treasury and attractive yields, it distinguishes itself from traditional yield aggregators by retaining third-party protocol rewards within the DAO treasury. This structure reduces selling pressure and circulating supply while optimizing risk-adjusted yields for farmers and rationalizing the bribe market.

Spiral DAO's unique treasury rebalancing mechanism and tokenomics allow it to capitalize on bribe market inefficiencies, improving bribe yields and liquidity depth. Although the protocol aims for quick treasury growth to offset emissions and maintain stability, users should consider the risks associated with underlying treasury assets, potential bank runs, and smart contract vulnerabilities. Overall, Spiral DAO's innovative approach holds the potential to positively impact the DeFi ecosystem and reshape the future of veCRV wars.
