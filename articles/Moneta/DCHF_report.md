# Asset Risk Assessment: DeFi Franc (DCHF)


#### A look into Moneta DAO and its Swiss franc stablecoin DCHF


### Useful Links

* [Protocol Website](https://www.defifranc.com/)
* [App](https://app.defifranc.com/)
* [Docs](https://docs.defifranc.com/)
* [GitHub](https://github.com/monetadao)
* [Deployed Smart Contracts](https://docs.defifranc.com/contracts)
* [Access Control Google Sheet](https://docs.google.com/spreadsheets/d/1B3Y2HIc9BNz5iVQPA-cw_GORxubk8MwLNCjYwIatVes/edit?usp=sharing)
* Market Data (Curve pool, analytics)
* Governance
  * [DAO Website](https://monetadao.com/) 
  * [Multisig address](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69)
  * [gov forum](https://commonwealth.im/moneta-dao/discussions)


## TLDR

This report delves into the DeFi Franc (DCHF) stablecoin (pegged to the Swiss Franc), and its Moneta DAO governing body. Specifically, we analyze the risks that the [DCHF-3CRV](https://curve.fi/#/ethereum/pools/factory-crypto-116/deposit) pool poses to LPs. MonetaDAO has proposed to add a gauge for this pool, and the governance proposal can be found [here](https://gov.curve.fi/t/proposal-to-add-dchf-3crv-pool-to-the-gauge-controller/8701). 

* **Custody risk**: The [AdminContract](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254) gives the owner privileges that could potentially impact user funds, primarily by adding collateral types and oracle pricefeeds. Although contracts are non-upgradeable, a [4-of-6 multi-sig](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69) owns all relevant contracts including [AdminContract](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code), [DFrancParameters](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code), and the [DCHF token](https://etherscan.io/address/0x045da4bFe02B320f4403674B3b7d121737727A36), leading to concerns about centralization. In response, the team has recently [improved their multi-sig](https://etherscan.io/tx/0x410dc68a309a665272a1a0e30f9b332af24206cbe23cd3eb314596f01465fea3) by adding a Curve team member and increasing the threshold from 3 to 4 signers.
* **Governance Risk**: The team has recently launched the Moneta DAO. DCHF, while a fork of the governance-free Liquity Protocol, is itself governed by the Moneta DAO. Nevertheless, there is limited bottom-up community engagement, with the core team coordinating recent activities (i.e., formation of community grants committee), raising the question of how much decentralization is actually achieved. 
* **MON Token distribution**: There is a need for updated messaging and documentation as the team has conflicting statements about the role of MON token holders. There are also governance risks regarding the distribution of MON tokens. Although the distribution is public and transparent, there may be questions about allocation decisions made before the formation of the DAO.

Overall, Llama Risk believes the protocol risks are on par with other early-stage DeFi protocols. The Moneta team has made a good faith effort by improving security in the Owner multi-sig and moving toward decentralized governance. It's our opinion that the protocol meets requirements to receive a Curve gauge.


## DCHF - Introduction

### History & Context

DeFi Franc (DCHF) is an overcollateralized stablecoin pegged to the Swiss franc, created on September 25, 2022 by [Moneta DAO Deployer](https://etherscan.io/address/0x7d7711efd844e5e204df29dc3e109d1af95a801c) ([source](https://etherscan.io/tx/0x7d6ba3055e572fffda740ee48482517061c98b7b04ec79ea08478280010070f1)). On January 25, 2023, a proposal to add the DCHF+3CRV pool to the Gauge Controller was made by Andrés Soltermann, founder of [Grizzly.Fi](https://blog.grizzly.fi/team-of-grizzly/) and current DCHF core team member ([source](https://www.linkedin.com/in/andr%C3%A9s-soltermann-379aa3174/?originalSubdomain=ch)). Grizzly.Fi is a liquidity mining aggregator with presence on Binance Smart Chain ([source](https://defillama.com/yields?project=grizzlyfi)) and on Ethereum ([source](https://app.grizzly.fi/)).

DCHF initially launched with governance controlled solely by the Moneta team. The native MON token was used only for distributing protocol fees to stakers, and used in a liquidity mining scheme. DCHF transitioned into a DAO governance model with the launch of Moneta DAO on February 5, 2023 ([source](https://commonwealth.im/moneta/discussion/9887-hello-world)). MON token holders govern the DeFi Franc protocol through a dual-governance structure and have proposed initiatives such as a Liquidity Strategy, expanded signers on the Moneta DAO multi-sig, and a Community Grants Program. 

A Curve DAO vote took place from March 20, 2023 to March 27, 2023 to add the DCHF gauge, but it did not pass quorum ([source](https://dao.curve.fi/vote/ownership/300)). The team has stated an intention to attempt another vote to add a gauge to the DCHF-3CRV pool, citing the transition to DAO governance and improved multi-sig parameters as reasonable steps taken to alleviate concerns of custody risk.


### Comparison to Liquity

The DeFi Franc (DCHF) protocol is a fork of [Vesta Finance](https://vestafinance.xyz/), which is itself a fork of [Liquity](https://www.liquity.org/). The foundational architecture of the protocol family involves an over-collateralized, crypto-backed stablecoin, and a philosophical preference for decentralized collateral and governance-minimization. The protocols have many common features, including stability pools that facilitate liquidations, universal redemption (anyone owning the protocol stablecoin can redeem for system collateral), and a recovery mode that programmatically protects against system insolvency in case the Total Collateral Ratio (TCR) falls below 150%.

While DCHF has the same basic peg mechanics as Liquity, there are some significant differences. For instance: 

* DCHF is pegged to the value of one Swiss franc (CHF) instead of one US Dollar.
* DCHF allows two collateral tokens: ETH and wBTC, with capability to add new collateral types, instead of just ETH.
* DCHF is governed by the Moneta DAO through its native MON token, while Liquity is governance-free.
* DCHF currently has system access control granted to a 4-of-6 multi-sig, while Liquity has no such access control.


### Protocol Mechanics

When opening a position, users can lock ETH and/or wBTC into the protocol to borrow against, with a minimum collateral ratio of 110%, which creates a price floor and ceiling through arbitrage opportunities. DCHF can be redeemed for ETH or wBTC at face value (1 DCHF for 1 Swiss Franc’s worth of ETH / wBTC) whether DCHF is below or above peg. The user can add collateral, repay debt, or close out the position at any time.

There are several parameters, adjustable through the [dfrancParameters](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code) contract, that a user experiences upon opening a position. The DeFi Franc (DCHF) protocol currently requires a minimum debt of 2,000 DCHF, and will charge a algorithmically-determined borrowing fee. It also sends 200 DCHF to the (GasPool)[https://etherscan.io/address/0xc9a113c35f961af3526e6f016f6df9da0a4c7bfa#code] contract as a "Liquidation Reserve". This reserve counts toward the overall debt and is used to pay liquidator gas fees in case the position becomes eligible for liquidation, but is otherwise refunded upon position close.  


#### Dynamic baseRate

The protocol maintains its peg to the Swiss franc through redemption and borrow fees, and a stability pool. Borrow and redemption fees adjust algorithmically to regulate borrowing and redemption velocity, in what is calculated as the "baseRate". The baseRate increases when DCHF is redeemed, and decays over time according to a predefined `DECAY_FACTOR`, with an upper an lower bound of 0.5% to 5%.

Higher redemption volumes imply DCHF is below peg. Therefore, borrow and redemption fees are increased to discourage borrowing and redemption of DCHF for ETH/wBTC. Both forces combine to push the peg towards 1 DCHF = 1 CHF ([source](https://docs.defifranc.com/in-depth/price-stability-and-redemptions)).

The baseRate is calculated as follows (from the Liquity whitepaper):

![Screen Shot 2023-04-13 at 3 58 02 PM](https://user-images.githubusercontent.com/51072084/231900851-0b8db7de-010b-4696-9df7-1c7616e34117.png)

Source: [Liquity Whitepaper](https://docsend.com/view/bwiczmy#)

Here’s an example of where 1 DCHF is trading at 0.95 CHF (below peg). Assuming 1,000 DCHF supply and 1,500 ETH collateral at a 150% collateral ratio and a 1% baseRate:

* A user borrowing 100 DCHF pays 1 CHF in borrow fees (1% of 100 DCHF)
* 50 people redeeming 10 DCHF each results in 50% of DCHF supply redeemed, causing baseRate to increase from 1% to 1.5%
* Higher baseRate increases borrowing costs, limiting DCHF supply in the market and expected to push its value towards peg
* Redemption fees, calculated as (baseRate + 0.5%) * ETH drawn, incentivize holding DCHF instead of redeeming, reducing excessive redemption
* Increased demand for DCHF due to these mechanisms may increase its price towards the 1 CHF peg.
[Source](https://docs.defifranc.com/in-depth/price-stability-and-redemptions)


#### Stability Pool

The Stability Pool is crucial for maintaining protocol solvency. There is a unique pool for each collateral asset and each one accepts DCHF deposits. The DCHF supplies liquidity for liquidation events and allows depositors to deposit from their contribution. When a debt position is liquidated, the remaining debt is burned from the Stability Pool while the collateral for the debt position is transferred to the pool. Depositors gain a proportionate share of liquidated collateral in exchange for a proportionate share of their DCHF deposits over time. This ensures a healthy collateral ratio is maintained to back the supply of DCHF. 

Positions are liquidated below 110% collateral ratio (CR), and due to the 10% liquidation fee, Stability Providers typically will experience a net gain. It may be possible that Stability Pool depositors lose money, in cases where liquidation occurs below a 100% CR. Even in normal conditions, the pool provider may wish to actively reduce exposure to volatile collateral, if they believe prices will continue to decline.

Anyone can initiate a liquidation, and they will receive 200 DCHF as compensation for gas costs + .5% of the position collateral.


#### Recovery Mode

When the Total Collateral Ratio (TCR) of the DeFi Franc protocol falls below 150%, Recovery Mode is activated, allowing for the liquidation of all positions with a collateral ratio (CR) below 150%. There are also additional parameters on borrowing: borrowing can only occur if the position CR would be >150% and the borrow fee is set to 0%. This is intended to encourage borrowers to increase the TCR above 150% and for DCHF holders to replenish the Stability Pool. Details on the liquidation mechanics during Recovery Mode are available in the [documentation](https://docs.defifranc.com/in-depth/recovery-mode). 

The chart below shows the daily TCR from October 2022 to the present, indicating that the system has remained sufficiently collateralized since inception. As such, DCHF has never experienced Recovery Mode in production.

![Screen Shot 2023-04-14 at 12 45 30 PM](https://user-images.githubusercontent.com/51072084/232141280-c4574129-6338-490b-98f9-4126386b5c30.png)

Source: [DCHF Dashboard](https://app.defifranc.com/)


### Governance

DeFi Franc draws inspiration from Liquity, but sets itself apart by introducing DAO governance and having the ability to update the system with new features. Governable aspects of the system include:

* Add new collateral types, including an associated Stability Pool and oracle
* Adjust system parameters, including set oracle pricefeed, set minimum collateral ratio before triggering liquidation (101%-1000%), set critical collateral ratio before triggering Recovery Mode (101%-1000%), set min/max borrow fee (0%-10%), set Liquidation Reserve fee (1-200), set minimum borrow amount (0-10,000), and set min redemption fee (.1%-10%)
* Introduce a whitelist for DCHF redemptions (note: this is seperate from repaying debt, and governance cannot prevent depositors from withdrawing their collateral)
* Upgrade to a new AdminContract, TroveManager, and BorrowingOperations contract
* Handling of MON funds between the CommunityIssuance and StabilityPool contracts
* Emergency pause of MON staking deposits and pause of DCHF minting

While the protocol allows for additional collateral types and native leverage on crypto-assets and LP Tokens, the core team currently has no immediate plans to add new collateral types. The team believes, based on informal discussion, if additional collateral were to be considered in the future it may involve staked ETH. Nevertheless, Moneta DAO has discretion over supporting new collateral types for DCHF, and the 4-of-6 multi-sig has the power to enact it. 


#### Moneta DAO

The most significant addresses to Moneta governance are the [4-of-6 Owner multi-sig](https://etherscan.io/address/0x83737eae72ba7597b36494d723fbf58cafee8a69) and the [4-of-6 Treasury multi-sig](https://etherscan.io/address/0x5592cb82f5b11a4e42b1275a973e6b712194e239). The Treasury and Protocol Owner addresses are composed of the same individuals.

The Moneta DAO has two governance bodies:

* Token House is hosted by MON token holders who govern strategic decisions, including who should be in the Delegate House. It governs allocation of treasury funds and protocol upgrades. 
* Delegate House comprises of elected members who oversee day-to-day governance decisions ([source](https://monetadao.com/))

To put it plainly, Moneta is structuring itself similar to many early-stage DeFi projects: tokenholders can signal vote through Snapshot and a multi-sig of trusted individuals has the power to enact (or not) the wishes of tokenholders. The Moneta DAO has recently raised the multi-sig threshold from 3-of-5 to 4-of-6 to address concerns of potential collusion among multi-sig signers ([source](https://commonwealth.im/moneta-dao/discussion/10509-expand-signers-on-the-monetadao-multisig)). 

The six multisig signers are ([source](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69#readProxyContract)):

1. Andrés Soltermann (Moneta Co-Founder)

2. Christian Killer (Moneta Advisor)

3. Roman Fritschi (Moneta Co-Founder)

4. Nils (elnilz @ FiatDAO)

5. Hubert (StakeDAO)

6. Mimaklas (Curve)

Moneta DAO hosts community discussions on the Commonwealth forum where they made their inception announcement on February 26th, 2023 ([source](https://commonwealth.im/moneta-dao/discussions)). There are 55 members in the governance forum ([source](https://commonwealth.im/moneta-dao/members)), with 138 voting members on Snapshot ([source](https://snapshot.org/#/monetadao.eth)). To date, the DAO has held four governance votes, including a test vote, launching Moneta DAO, adopting a liquidity strategy, and expanding multisig signers ([source](https://snapshot.org/#/monetadao.eth)). You can find additional governance details in this forum post and the Moneta Tokenomics page ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao), [source](https://docs.defifranc.com/tokenomics/moneta-tokenomics#allocation)). 

On April 7th, 2023, a proposal was made to create a Community Grants Committee with a 5-of-9 multi-sig ([GnosisSafeProxy address available here](https://etherscan.io/address/0xba4ee2C6f2DC76f69d14F36ac4255bd4899BF5f0)). However, the Moneta DAO Token House (MON token holders) can choose to dissolve the Community Grants Committee, which has granted the Treasury Multi-sig full allowance over its MON holdings ([source](https://commonwealth.im/moneta-dao/discussion/11047-community-grants-program)). 

Once the allowance transaction is executed, the Treasury multi-sig will have full allowance over the Community Grants Committee multi-sig. The Community Grants Committee consists of representatives from Moneta DAO and the team, including Dr. LM, Andrea, Markus, Nilic, Daniel, ViTo, Tom, Leon, and Andrés ([source](https://commonwealth.im/moneta-dao/discussion/11047-community-grants-program)).


#### MON Governance Token

The [MON token](https://etherscan.io/address/0x1ea48b9965bb5086f3b468e50ed93888a661fc17), issued by Moneta DAO, is intended to govern the DCHF protocol and is described as a revenue-sharing token that can only be obtained by staking DCHF in one of the stability pools, or by supplying liquidity to the Curve pool. Users can stake their MON to earn a proportionate share of borrowing and redemption fees in DCHF, ETH, and wBTC ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics), [source](https://docs.defifranc.com/tokenomics/moneta-staking)).

There are inconsistencies between the Moneta DAO documentation and governance forum announcements regarding the governance role of MON token holders. On one hand, the documentation states: “staked MON tokens are not used for governance as there is no DeFi Franc governance” ([source](https://docs.defifranc.com/tokenomics/moneta-staking)), but the announcement post for launching Moneta DAO suggest MON token holders and stakers “govern over strategic DAO decisions” including: allocation of DAO treasury funds, critical risk parameters and protocol upgrades ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao)). This is likely due to the recent introduction of the DAO (March 2023) and the docs should be updated for clarity.

According to the Moneta DAO [Tokenomics page](https://docs.defifranc.com/tokenomics/moneta-tokenomics), MON was launched with a max supply of 100m tokens that have been allocated toward four main purposes:

* 72.5% - Treasury reserve for future distribution (decided by the DAO)
* 15% - Airdrop to tokenholders of Grizzly (GHNY), Liquity (LQTY), and token lockers in the [Grizzly Freezer](https://app.grizzly.fi/en/freezer)
* 10% - Team and advisors
* 2.5% - Liquidity bootstrapping (Supply liquidity and incentives to a MON/ETH pool)

The [Protocol Owner multi-sig](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69) was initially seeded with 100,000,000 tokens on September 23, 2022 ([source](https://etherscan.io/tx/0xa57dd3c6e8f44be7e3f17904b3d5719da1857b205ce3a0af4ddcecd8a4cbb4fc)). The [docs](https://docs.defifranc.com/contracts) list the contracts used in the initial distribution, but many have been deprecated with the launch of Moneta DAO. Moneta documentation should be updated to reflect new decisions around treasury management and allocation. The following chart shows funds that have been returned to the owner multi-sig since the original distribution.

![mon_distribution](https://user-images.githubusercontent.com/4058461/232518729-ade22691-2c4c-46b4-b8c4-92d351e953ee.png)

Here is a [dashboard](https://dune.com/paulapivat/llama-risk-assessment-defi-franc) to show onchain transactions to and from the [Protocol Owner multi-sig](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69); see "MON Received and In Circulation".

The following chart shows the actual distribution of MON as of April 17, 2023. The majority of funds have been transferred to a new Treasury multi-sig and to the Grizzli.fi multi-sig, which provided Moneta DAO with $1m of seed funding denominated in USDC, CRV, and LIT. The owner multi-sig continues to be a regular funding source for Curve gauge rewards, Stability Pool incentives (CommunityIssuance), and airdrop claims.

![MON dist flow drawio](https://user-images.githubusercontent.com/51072084/232602695-d5099d2b-8671-48d7-806a-d6a344851419.png)

Contract addresses:

* [Uniswap MON pool](https://etherscan.io/address/0x21f396dd37a26d7754c513fd916d07f66aa6b81e) ([tx](https://etherscan.io/tx/0x8b6a6522a2f3422eb444c9577fdb07a4f107527e30de1f9e75e0b6c8a397a71e))
* [UniV3 rewards vest](https://etherscan.io/address/0xc0747a27c6fa20effba2937419647e976f111611)
* [Team vest](https://etherscan.io/address/0x8eba1ad289f5e6c50d2f924e17cc8dd607b3c083)
* [lockedMON](https://etherscan.io/address/0x020b7d785d343c92f3be7d802545d031e943366f)
* [Treasury multi-sig](https://etherscan.io/address/0x5592cb82f5b11a4e42b1275a973e6b712194e239)
* [Grizzli.fi multi-sig](https://etherscan.io/address/0xcE88F73FAA2C8de5fdE0951A6b80583af4C14265)
* [Moneta DAO: Deployer](https://etherscan.io/address/0x7d7711efd844e5e204df29dc3e109d1af95a801c)
* [Curve Gauge Rewards](https://etherscan.io/address/0xe55393dd3002ac8e9b82a804774616c35cc8fd64)
* [CommunityIssuance](https://etherscan.io/address/0x0fa46e8cBCEff8468DB2Ec2fD77731D8a11d3D86) (Stability Pool incentives)
* [MON airdrop](https://etherscan.io/address/0xff42ec1c83e0f4939c45ab4f6a027b44e5a3fc8f)

The [Treasury multi-sig](https://etherscan.io/address/0x5592cb82f5b11a4e42b1275a973e6b712194e239) currently holds around 50% of all tokens. Of circulating supply, the majority is in the [MONStaking contract](https://etherscan.io/token/0x1EA48B9965bb5086F3b468E50ED93888a661fc17?a=0x8bc3702c35d33e5df7cb0f06cb72a0c34ae0c56f) (17m) and the [Uniswap pool](https://etherscan.io/token/0x1EA48B9965bb5086F3b468E50ED93888a661fc17?a=0x21f396dd37a26d7754c513fd916d07f66aa6b81e) (3m). Since all allocations were made before the DAO was formed and without community input, there may be questions regarding the airdrops. The Grizzly.Fi community received the most tokens (11.17m), and it is worth noting that members of the core team are also co-founders of Grizzly.Fi. This overlap may be a cause for concern, as the majority of MON circulating supply is from airdrops and the team's overall MON allocation is obscured by the airdrop strategy.




### Market and Utility

Over 90% of circulating DCHF is held in the WBTC/ETH Stability Pools and the Curve DCHF/3CRV pool. The UniV3 DCHF/USDC pool also ranks among the top addresses, although only making up ~2% of the supply. Top address as of April 2023 are as follows:

![Screen Shot 2023-04-13 at 5 41 09 PM](https://user-images.githubusercontent.com/51072084/231912967-29ad3f62-7c4c-4fb2-a2cd-763f4a740d15.png)

Source: [Etherscan](https://etherscan.io/token/0x045da4bFe02B320f4403674B3b7d121737727A36#balances)

Currently, the DCHF token is traded on two major DEXes including Uniswap and Curve ([source](https://dune.com/queries/2297121), [source](https://www.coingecko.com/en/coins/defi-franc#markets)) and does not appear to be traded on any CEXes. The vast majority of liquidity resides in the Curve pool.

For DEX volume, Curve has historically led, with Uniswap gaining ground in recent months. These two charts show daily sell and buy volume, respectively:

![DCHF_DEX_vol](https://user-images.githubusercontent.com/51072084/231853767-f7908825-e4c3-4fc5-8a17-a9bf42ded16a.png)

[Source query](https://dune.com/queries/2318214/3794424)

![DCHF_DEX_vol_buy](https://user-images.githubusercontent.com/51072084/231853847-13dfc290-b241-40c1-8cad-6a66baf4b3b5.png)

[Source query](https://dune.com/queries/2320640/3798115)

TVL and outstanding DCHF declined after initial release in September of 2022 and adoption appears to be relatively flat since November 2022.

![DCHF_TVL](https://user-images.githubusercontent.com/51072084/231854056-f81d3ae5-ca2b-454a-9517-9553325e1a65.png)

[Credit: DeFiLlama](https://defillama.com/protocol/defi-franc?denomination=USD)

![DCHF_TVL2](https://user-images.githubusercontent.com/51072084/231854095-c8d9a024-94e2-4d1a-a948-f98f0cca1c06.png)

[Source query](https://dune.com/queries/2297708/3764348)


## Risk Vectors

### Smart Contract Risk

DCHF is a fork of Vesta Finance, which itself is a fork of the Liquity decentralized borrowing protocol. Although Liquity is a well-audited protocol, engineers from Vesta have cautioned against forking these projects due to messy and unoptimized code that may not be up-to-date with new features ([source](https://github.com/vesta-finance/vesta-protocol-v1/releases/tag/v1.0)). Despite this warning, DCHF remains a fork of Vesta with no visible effort to keep up with changes in the original code base, possibly due to their use of non-upgradeable contracts ([source](https://github.com/monetadao/dchf/tree/main/contracts)).


**Audits**

DeFi Franc Version 1.0 has undergone [two audits from CertiK and General IT](https://docs.defifranc.com/extras/audits). The latter is an audit by a foreign company. An in-browser translation was required to decipher the contents of the audit (see screencap below). Two critical issues were found including: 

1. Possible re-entrancy attacks: This vulnerability was found for the StabilityPool and ETHTransferScript contracts. The auditors advised the use of “ReentrancyGuard” library and the team appears to have fixed the issue with a commit to a private repository ([source](https://hackmd.io/Jsn0wBW7SOWFm-L34V4cMw)).
2. Wrong ether value: This vulnerability is featured in the PriceFeed and PriceFeedV2 contracts and involves the conversion of “int256” to “uint256”, which could lead to an arithmetic overflow. The team has taken the issue into account, but it is unclear if this issue has been fully addressed ([source](https://hackmd.io/Jsn0wBW7SOWFm-L34V4cMw)). 

There were four middle level issues identified including ([source](https://hackmd.io/Jsn0wBW7SOWFm-L34V4cMw)):

1. Violation of the ERC-20 standard: This issue has been fixed. 
2. Lack of gas to complete all loop iterations (DoS): This issue has been taken into account. 
3. It is possible to block tokens on the balance of the contract: This issue has been taken into account. 
4. Wrong token price: This issue has been taken into account. 

The CertiK audit has reported 0 unresolved and 0 critical issues from this project. With no critical issues, here’s a sample of major/medium issues that may be related to additional risk vectors outside of smart contracts: 

**GLOBAL-01 | Centralization Related Risk**: In the [AdminContract](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code), the `_owner` role has extensive authority over several functions that could affect borrow/redemption fees, addition or removal of collateral assets from stability pool, transferring funds out of Stability Pool, changing vesting periods or, worse yet, changing the AdminContract itself including, among others:

        * setAddresses 
        * addNewCollateral
        * emergencyStopMinting
        * setDfrancParameters 
        * setCollateralParameters 
        * setPriceFeed 
        * setAdminContract 
        * removeFundFromStabilityPool
        * transferFundToAnotherStabilityPool
        * setAdminContract 
        * changeTreasuryAddress
        * setRedemptionWhitelistStatus 
        * addUserToWhitelistRedemption 
        * removeUserFromWhitelistRedemption 

Moreover, the AdminContract has the power to upgrade several contracts without community consensus including: SortedTroves, StabiltiyPool, StabiltiyPoolManager, and TroveManager, among others. This opens the door for an attacker to change the contract implementation and drain tokens. 

**MOT-01 | Initial Token Distribution (mitigated)**: Another major issue within the centralization / privilege risk category involves how MON tokens were sent to a `_treasurySig` address when deploying the contract. This implies that MON tokens can be distributed without community consensus. To mitigate this risk, the team has documented their tokenomics and how MON tokens are distributed ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics)). 

**GLOBAL-02 | Lack of Storage Gap (resolved)**: Several contracts were upgradable including `AdminContract`, `StabilityPoolManager`, `TroveManager` etc. This means there needs to be a storage gap to allow the addition of new state without compromising existing deployments. In response, the Certik report indicates the team has modified the design and opted for non-upgradeable contracts. While non-upgradeable contracts prevent future changes that could compromise the protocol, there are still options for the system to make updates. The Protocol Owner multisig could still addNewCollateral through the AdminContract ([source](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code)) or they could make updates to parameters through the DFrancParameters contract ([source](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code)). 


**Audit Recommendations**

The team followed Certik's recommendations by assigning privileged roles to multi-sig wallets and transitioning to a DAO governance/voting module. The Treasury multi-sig and Protocol owner multi-sig (which controls the AdminContract) addresses can be found [here](https://etherscan.io/address/0x5592cB82f5B11A4E42B1275A973E6B712194e239) and [here](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69), respectively.

Certik also advised implementing a time-lock contract with a 48-hour latency for privileged operations. The team has considered this but opted not to give ownership to a time-lock contract at this time to remain agile. A permanent solution could be to renounce ownership or remove the owner role's ability to affect collateral assets from the stability pool, transfer funds, change vesting periods, or alter the AdminContract. This would put DCHF's trust assumptions in line with Liquity, but would sacrifice flexibility, as new collateral types and oracles could no longer be altered.

Another solution could be to introduce fully on-chain governance controlled by a DAO of tokenholders. The transition to a DAO is still in its early stages with limited community activity outside of the team. Questions could be raised about the fairness of the MON token distribution, given the overlap between core Treasury multi-sig signers and Grizzly.Fi airdrop recipients. To address this, the team has set up a [multi-sig proxy address](https://etherscan.io/address/0x83737eae72ba7597b36494d723fbf58cafee8a69) and transferred ownership of the AdminContract to this address, providing transparency to users about the token distribution ([proof of transfer](https://etherscan.io/tx/0xea7d8303eb36885d2446bd3ea73ca64027f5e851c5ba0119fddd0370b3604468)). This provides a reference to help users check whether MON tokens have been distributed as advertised. For more information, check the Moneta DAO's token distribution documentation ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics)).


### Centralization & Custody Risk

The [4-of-6 multi-sig](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69) ownership of the AdminContract (as well as ownership of all relevant system contracs) grants it extensive authority over critical protocol functions, raising custody risk concerns. The team has opted for non-upgradeable contracts, which means the Owner multi-sig must add new collateral through the [AdminContract](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code) or update existing collateral parameters through the [DFrancParameters](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code) contract. This multi-sig has the power to:

* Add new collateral types, including an associated Stability Pool and oracle
* Adjust system parameters, including set oracle pricefeed, set minimum collateral ratio before triggering liquidation (101%-1000%), set critical collateral ratio before triggering Recovery Mode (101%-1000%), set min/max borrow fee (0%-10%), set Liquidation Reserve fee (1-200), set minimum borrow amount (0-10,000), and set min redemption fee (.1%-10%)
* Introduce a whitelist for DCHF redemptions (note: this is seperate from repaying debt, and governance cannot prevent depositors from withdrawing their collateral)
* Upgrade to a new AdminContract, TroveManager, and BorrowingOperations contract
* * Handling of MON funds between the CommunityIssuance and StabilityPool contracts
* Emergency pause of MON staking deposits and pause of DCHF minting

Although centralization risks found in the audit have been mitigated to an extent, extensive privileges remain available to the core team to exercise.

The team has [expanded the multi-sig threshold](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69#readProxyContract) from 3-of-6 to 4-of-6 and added a Curve team member ([Mimaklas](https://twitter.com/cryptomits)). They have additionally transitioned to a DAO, which is still in its early stage of development. It remains to be seen how well Moneta can garner community involvement in decision-making and on-chain governance. At this time, the core team is necessary for the DAO to operate effectively.

The recommended next step to reduce custody risk is to pass ownership to a timelock contract for all functions that are not time-sensitive. The multi-sig without timelock should ideally be reserved for emergency actions (pause MONStaking/pause DCHF minting). The long-term goal should be to transition either to fully on-chain governance or to opt for governance minimization by renouncing ownership of the relevant contracts. 

A list of access controls for the system contracts has been compiled on this [Google Sheet](https://docs.google.com/spreadsheets/d/1B3Y2HIc9BNz5iVQPA-cw_GORxubk8MwLNCjYwIatVes/edit?usp=sharing).


## Governance Risk

Here’s an overview of major MON token holders:

![MON_token_distribution](https://user-images.githubusercontent.com/4058461/232521246-a083515d-7a35-4172-8716-665116091442.png)

[Source query](https://dune.com/queries/2320797/3800099)

![MON_token_distribution2](https://user-images.githubusercontent.com/51072084/231855260-c667ec27-9148-4fbd-9e42-035ec92e4978.png)

[Source query](https://dune.com/queries/2320797/3800099)

Grizzly.Fi incubated both the DeFi Franc and Moneta DAO. The team and advisors have been allocated 10% of tokens (10m MON) ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics)), and the DAO was allocated a treasury seed of 1M USD worth of assets (9,174,312 MON) ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao)).

There are two entities to Moneta DAO governance: Token House and Delegate House ([source](https://monetadao.com/)). As discussed above Moneta DAO token holders will have say over strategic protocol decisions including allocation of DAO treasury funds, critical risk parameters and protocol upgrades ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao)). However, in the early stage of development, there continues to be an absence of a clear process for prominent community members to serve as delegates and permissioned roles within the DAO. 

Additionally, the distribution of MON tokens was made before the formation of the DAO, without community input. Moreover, given that there is no clear process for delegation, we can assume that the current multi-sig signers of the Treasury and Protocol Owner proxy are the de facto delegates. While delegates execute the wishes of tokenholders, this relationship is non-binding and trust-based. 

While the token holders will have a say in strategic protocol decisions, the extent of governance remains uncertain. Clear communication of a governance process and plans for delegation will be helpful.


## **Oracle Risk**

DCHF relies on a Chainlink oracle, with the AdminContract able to call the addOracle function, creating a risk if the owner multi-sig is compromised. 
Chainlink is a well-regarded oracle provider, although DCHF lacks a backup oracle, relying on the fetchPrice function which interacts with two Chainlink oracles and allows a maximum deviation of 50% ([source, line 148](https://github.com/monetadao/dchf/blob/main/contracts/PriceFeed.sol)) between updates. As a result, if one CHF is inaccurately priced, it could lead to negative effects on liquidations and the redemption mechanism, ultimately resulting in potential loss of funds ([source](https://docs.defifranc.com/extras/audits)).

Liquity, by comparison, uses Chainlink as a primary oracle and Tellor as a backup. It will algorithmically reject the primary oracle if it fails to update or produces a value outside of the specified guardrail. The strategy was designed to ensure the pricefeed can autonomously react to black swan events without requiring manual intervention. The DCHF strategy, on the other hand, may require more active monitoring and manual updates in case of oracle failure.


## **Depeg Risk**

The DeFi Franc protocol currently has 6,694,401 DCHF tokens minted, valued at $1.15, totaling approximately $7.6m, and holds collateral worth around $17m (ETH and wBTC combined). Redemptions are supported by sufficient collateral.

![DCHF_minted](https://user-images.githubusercontent.com/51072084/231855722-53a9c0e0-11a1-47ca-b208-edf16eaab5a6.png)

[Query 1](https://dune.com/queries/2322419/3801296), [Query 2](https://dune.com/queries/2297708/3800983)

![DCHF_backing](https://user-images.githubusercontent.com/51072084/231855987-894da348-86ff-4a5c-8111-855a3814467c.png)

[Credit: Etherscan](https://etherscan.io/tokenholdings?a=0x77E034c8A1392d99a2C776A6C1593866fEE36a33)

![DCHF_openposition](https://user-images.githubusercontent.com/51072084/231856182-aeeb2d96-dc3b-4261-a1bc-cec1d629680a.png)

[Source query](https://dune.com/queries/2322141)

However, a minor contract risk exists due to the setDCHFGasCompensation() function potentially resulting in insufficient DCHF tokens to burn from the gas pool, which could prevent redemptions ([source](https://skynet.certik.com/projects/defi-franc)). This has been partially resolved, but still could result in MCR (minimum collateral ratio) > CCR (collateral coverage ratio).

To control the supply of DCHF, the protocol utilizes borrow and redemption fees. A 110% collateral ratio provides a price floor and ceiling, and arbitrageurs are incentivized to maintain price stability. However, a depeg risk exists if demand for DCHF is insufficient and it trades above 1 CHF for a prolonged period. The protocol does not rely on an external AMM for supply expansion or contraction, and burning/withdrawal amounts are approved through on-chain peg mechanisms. 

As a fork of Liquity- a resilient and battle tested stablecoin, the risk of depeg is currently low. The collateral selection (ETH/WBTC) is also reasonably conservative. It is our opinion that the greatest risk of future depeg is the possibility of governance mismangement. If low quality collateral types are ever added to the protocol (e.g. the protocol's native governance token MON, or complex derivative assets), failure or manipulation of underlying collateral can lead to system insolvency and therefore a depeg. 


## LlamaRisk Gauge Criteria

### Centralization Factors

**1. Is it possible for a single entity to rug its users?**

The risk of a single entity rug-pulling users has been reduced by transferring ownership privileges to a multi-sig, which currently has a 4-of-6 signer setup and includes a member from the Curve team. It is still possible for this multi-sig to take action that can impact user funds. 

**2. If the team vanishes, can the project continue?**

The protocol can continue operating normally in the absence of the team, but there would be no way to update the system or respond to an emergency. With the DAO being recently launched, there is still no direct way for tokenholder to directly assert control over the protocol. 

### Economic Factors

**1. Does the project's viability depend on additional incentives?**

The protocol depends on its own incentives to drive liquidity to its Stability Pools and DEXs like Curve. It does not depend on additional incentives except as a bootstrapping tool.

**2. If demand falls to 0 tomorrow, can all users be made whole?**

Yes, there’s roughly $17m in ETH and wBTC collateral supporting $7.6m in DCHF value. Users can redeem their deposits tomorrow. 

### Security Factors

**1. Do audits reveal any concerning signs?**

The audits did not identify any critical issues, but flagged a few major concerns that have been partially or fully addressed. The primary concern was the overreaching powers of the AdminContract, which has been transferred to multi-sig signers. It should also be noted that DCHF is a fork of Vespa Finance and Liquity. Although, the DCHF protocol has switched to non-upgradeable contracts, it does have a path to update the system through the AdminContract ([source](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code)) or make parameter updates through the DFrancParameters contract ([source](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code)). 

# Risk Team Recommendation

We have three recommendations that would be beneficial to the DCHF ecosystem and community:

1. Update documentation to be explicit about MON token holders strategic role in governing DCHF protocol 
2. Be explicit about allowance/powers the Treasury / Protocol  multi-sig continues to have through all revelant system contracts, and provide a clear roadmap toward decentralization.
3. Create a policy for acceptable forms of collateral and process for adding collateral types that Moneta DAO is willing to stand by.

Overall, we see the challenges Moneta DAO faces to decentralize as typical for a DeFi protocol in its early stage of deployment. While there certainly exist risks and uncertainties, we see there has been a good faith effort by their team to reasonably alleviate concerns. Our hope is that they continue to exercise good judgement by remaining conservative when adding any new collateral types. We support the team's proposal for a Curve gauge to the DCHF/3CRV pool.
