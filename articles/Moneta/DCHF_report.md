# Asset Risk Assessment: DeFi Franc (DCHF)


#### A look into MonetaDAO and its Swiss franc stablecoin DCHF


### Useful Links

* [Protocol Website](https://www.defifranc.com/)
* [App](https://app.defifranc.com/)
* [Docs](https://docs.defifranc.com/)
* [GitHub](https://github.com/monetadao)
* [Deployed Smart Contracts](https://docs.defifranc.com/contracts)
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

When the Total Collateral Ratio (TCR) of the DeFi Franc protocol falls below 150%, Recovery Mode is activated, allowing for the liquidation of all positions with a collateral ratio (CR) below 150%. This is intended to encourage borrowers to increase the TCR above 150% and for DCHF holders to replenish the Stability Pool. Details on the liquidation mechanics during Recovery Mode are available in the [documentation](https://docs.defifranc.com/in-depth/recovery-mode). 

The chart below shows the daily TCR from October 2022 to the present, indicating that while most days exceed the minimum collateral ratio (110%, red line), on some days the ratio falls below 150% (green line), suggesting that Recovery Mode may have been triggered.

![DCHF_CR](https://user-images.githubusercontent.com/51072084/231853447-c6973a9e-a28b-4c0b-bdb4-4d3303171800.png)

[Source: query](https://dune.com/queries/2316272/3794165)


### Governance

DeFi Franc draws inspiration from Liquity, but sets itself apart by introducing DAO governance and having the ability to update the system with new features. Governable aspects of the system include:

* Add new collateral types, including an associated Stability Pool and oracle
* Adjust system parameters, including set oracle pricefeed, set minimum collateral ratio before triggering liquidation (101%-1000%), set critical collateral ratio before triggering Recovery Mode (101%-1000%), set min/max borrow fee (0%-10%), set Liquidation Reserve fee (1-200), set minimum borrow amount (0-10,000), and set min redemption fee (.1%-10%)
* Introduce a whitelist for DCHF redemptions (note: this is seperate from repaying debt, and governance cannot prevent depositors from withdrawing their collateral)
* Upgrade to a new AdminContract, TroveManager, and BorrowingOperations contract

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

![Moneta_community](https://user-images.githubusercontent.com/51072084/231854594-f01c0ca3-ddbe-44f4-8327-43784c8a5e41.png)

[Source](https://commonwealth.im/moneta-dao/members) 

Moneta DAO hosts community discussions on the Commonwealth forum where they made their inception announcement on February 26th, 2023 ([source](https://commonwealth.im/moneta-dao/discussions)). There are 55 members in the governance forum ([source](https://commonwealth.im/moneta-dao/members)), with 138 voting members on Snapshot ([source](https://snapshot.org/#/monetadao.eth)). To date, the DAO has held four governance votes, including a test vote, launching Moneta DAO, adopting a liquidity strategy, and expanding multisig signers ([source](https://snapshot.org/#/monetadao.eth)). You can find additional governance details in this forum post and the Moneta Tokenomics page ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao), [source](https://docs.defifranc.com/tokenomics/moneta-tokenomics#allocation)). 

On April 7th, 2023, a proposal was made to create a Community Grants Committee with a 5-of-9 multi-sig ([GnosisSafeProxy address available here](https://etherscan.io/address/0xba4ee2C6f2DC76f69d14F36ac4255bd4899BF5f0)). However, the Moneta DAO Token House (MON token holders) can choose to dissolve the Community Grants Committee, which has granted the Treasury Multi-sig full allowance over its MON holdings ([source](https://commonwealth.im/moneta-dao/discussion/11047-community-grants-program)). 

Once the allowance transaction is executed, the Treasury multi-sig will have full allowance over the Community Grants Committee multi-sig. The Community Grants Committee consists of representatives from Moneta DAO and the team, including Dr. LM, Andrea, Markus, Nilic, Daniel, ViTo, Tom, Leon, and Andrés ([source](https://commonwealth.im/moneta-dao/discussion/11047-community-grants-program)).


#### MON Governance Token

The [MON token](https://etherscan.io/address/0x1ea48b9965bb5086f3b468e50ed93888a661fc17), issued by Moneta DAO, is intended to govern the DCHF protocol and is described as a revenue-sharing token that can only be obtained by staking DCHF in one of the stability pools, or by supplying liquidity to the Curve pool. Users can stake their MON to earn a proportionate share of borrowing and redemption fees in DCHF, ETH, and wBTC ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics), [source](https://docs.defifranc.com/tokenomics/moneta-staking)). 

According to the Moneta DAO [Tokenomics page](https://docs.defifranc.com/tokenomics/moneta-tokenomics), MON was launched with a max supply of 100m tokens that have been allocated toward four main purposes:

* 72.5% - Treasury reserve for future distribution (decided by the DAO)
* 15% - Airdrop to tokenholders of Grizzly (GHNY), Liquity (LQTY), and token lockers in the [Grizzly Freezer](https://app.grizzly.fi/en/freezer)
* 10% - Team and advisors
* 2.5% - Liquidity bootstrapping (Supply liquidity and incentives to a MON/ETH pool)

Token allocation can be confirmed in the following table, with the [Protocol Owner multi-sig](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69) being seeded with 100,000,000 tokens on September 23, 2022 ([source](https://etherscan.io/tx/0xa57dd3c6e8f44be7e3f17904b3d5719da1857b205ce3a0af4ddcecd8a4cbb4fc)) and distributing them since then. We attempt to corroborate token distribution as stated in the documentation:


<table>
  <tr>
   <td>Group
   </td>
   <td>Detail
   </td>
   <td>MON Supply (Documentation)
   </td>
   <td>MON Supply
<p>
(Confirmation)
   </td>
  </tr>
  <tr>
   <td>Launch (2.5%
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Liquidity Pool
   </td>
   <td>1,000,000
   </td>
   <td><strong>Uni V3 Liquidity Pool</strong> (971,695) (<a href="https://etherscan.io/tx/0x8b6a6522a2f3422eb444c9577fdb07a4f107527e30de1f9e75e0b6c8a397a71e">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Bootstrapping Rewards
   </td>
   <td>1,500,000
   </td>
   <td><strong>Vester</strong> contract with totalVestingAmount (1,500,000) (<a href="https://etherscan.io/address/0xc0747a27c6fa20effba2937419647e976f111611#readContract">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>Treasury (82.5%)
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Reward & Reserves  \
 \
(Marketing & Growth)
   </td>
   <td>72,000,000
   </td>
   <td><strong>Protocol Owner</strong> (start at 100,000,000 MON) (<a href="https://etherscan.io/tokentxns?a=0x83737eae72ba7597b36494d723fbf58cafee8a69&p=81">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td><strong>Marketing</strong> (vesting)
<p>
Defifranc_mon: LockedMONmarketing (12,750,000) (<a href="https://etherscan.io/address/0x2e92c456278d77558723ca263c713b5d30520a39#tokentxns">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td><strong>Marketing</strong> (wallets) (4,250,000) (<a href="https://etherscan.io/address/0x844a255666f63f1F663Ac93dD876F8Aa584830Ff#tokentxns">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td><strong>Reserves</strong> (wallets)
<p>
(7,750,000) (<a href="https://etherscan.io/tokentxns?a=0x83737eae72ba7597b36494d723fbf58cafee8a69&p=81">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td><strong>Reserves</strong> (vesting) (7,750,000) (<a href="https://etherscan.io/tokentxns?a=0x83737eae72ba7597b36494d723fbf58cafee8a69&p=81">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td><strong>Treasury Multisig </strong>(50,508,223) (<a href="https://etherscan.io/address/0x5592cb82f5b11a4e42b1275a973e6b712194e239">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Team & Advisors
   </td>
   <td>10,000,000
   </td>
   <td><strong>Team</strong> (vesting) \
DefiFranc_mon: LockedMONteam 
<p>
(<a href="https://etherscan.io/address/0x8eba1ad289f5e6c50d2f924e17cc8dd607b3c083#tokentxns">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>Airdrops (15%)
   </td>
   <td>
   </td>
   <td>
   </td>
   <td><strong>Airdrop</strong> (15,000,000) (<a href="https://etherscan.io/address/0x1bd03ad578e8c9f457961856842e920881ab2d8c#tokentxns">etherscan</a>)
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Airdrop 1 (GHNY Snapshot)
   </td>
   <td>5,882,676
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Airdrop 2 (Freezer)
   </td>
   <td>5,291,841
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Airdrop 3 (LQTY)
   </td>
   <td>3,825,483
   </td>
   <td>
   </td>
  </tr>
</table>


The [Treasury multi-sig](https://etherscan.io/address/0x5592cb82f5b11a4e42b1275a973e6b712194e239) currently holds around 50% of all tokens. Of circulating supply, the majority is in the [MONStaking contract](https://etherscan.io/token/0x1EA48B9965bb5086F3b468E50ED93888a661fc17?a=0x8bc3702c35d33e5df7cb0f06cb72a0c34ae0c56f) (17m) and the [Uniswap pool](https://etherscan.io/token/0x1EA48B9965bb5086F3b468E50ED93888a661fc17?a=0x21f396dd37a26d7754c513fd916d07f66aa6b81e) (3m). Since all allocations were made before the DAO was formed and without community input, there may be questions regarding the airdrops. The Grizzly.Fi community received the most tokens (11.17m), and it is worth noting that members of the core team are also co-founders of Grizzly.Fi. This overlap in roles may be a cause for concern, as the majority of MON circulating supply is from airdrops and the team's realized MON allocation is obscured by a significant airdrop to their previous project.

There are inconsistencies between the Moneta DAO documentation and governance forum announcements regarding the governance role of MON token holders. On one hand, the documentation states: “staked MON tokens are not used for governance as there is no DeFi Franc governance” ([source](https://docs.defifranc.com/tokenomics/moneta-staking)), but the announcement post for launching Moneta DAO suggest MON token holders and stakers “govern over strategic DAO decisions” including: allocation of DAO treasury funds, critical risk parameters and protocol upgrades ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao)). This is likely due to the recent introduction of the DAO (March 2023) and the docs should be updated for clarity.


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


# **Risk Vectors**

## **Smart Contract Risk**

DeFi Franc Version 1.0 has undergone [two audits from CertiK and General IT](https://docs.defifranc.com/extras/audits). The latter is an audit by a foreign company. An in-browser translation was required to decipher the contents of the audit (see screencap below). Two critical issues were found including: 

1. Possible re-entrancy attacks: This vulnerability was found for the StabilityPool and ETHTransferScript contracts. The auditors advised the use of “ReentrancyGuard” library and the team appears to have fixed the issue with a commit to a private repository ([source](https://hackmd.io/Jsn0wBW7SOWFm-L34V4cMw)).
2. Wrong ether value: This vulnerability is featured in the PriceFeed and PriceFeedV2 contracts and involves the conversion of “int256” to “uint256”, which could lead to an arithmetic overflow. The team has taken the issue into account, but it is unclear if this issue has been fully addressed ([source](https://hackmd.io/Jsn0wBW7SOWFm-L34V4cMw)). 

There were four middle level issues identified including ([source](https://hackmd.io/Jsn0wBW7SOWFm-L34V4cMw)):

1. Violation of the ERC-20 standard: This issue has been fixed. 
2. Lack of gas to complete all loop iterations (DoS): This issue has been taken into account. 
3. It is possible to block tokens on the balance of the contract: This issue has been taken into account. 
4. Wrong token price: This issue has been taken into account. 

![DCHF_audit](https://user-images.githubusercontent.com/51072084/231854879-d3a1ed32-90be-4246-8a48-d006fb860d0b.png)

[Source](https://hackmd.io/Jsn0wBW7SOWFm-L34V4cMw) 

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

Moreover, the AdminContract has the power to upgrade several contracts <span style="text-decoration:underline;">without</span> community consensus including: SortedTroves, StabiltiyPool, StabiltiyPoolManager, and TroveManager, among others. This opens the door for an attacker to change the contract implementation and drain tokens. 

**MOT-01 | Initial Token Distribution (mitigated)**: Another major issue within the centralization / privilege risk category involves how MON tokens were sent to a `_treasurySig` address when deploying the contract. This implies that MON tokens can be distributed without community consensus. To mitigate this risk, the team has documented their tokenomics and how MON tokens are distributed ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics)). 

**GLOBAL-02 | Lack of Storage Gap (resolved)**: Several contracts were upgradable including `AdminContract`, `StabilityPoolManager`, `TroveManager` etc. This means there needs to be a storage gap to allow the addition of new state without compromising existing deployments. In response, the Certik report indicates the team has modified the design and opted for non-upgradeable contracts. While non-upgradeable contracts prevent future changes that could compromise the protocol, there are still options for the system to make updates. The Protocol Owner multisig could still addNewCollateral through the AdminContract ([source](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code)) or they could make updates to parameters through the DFrancParameters contract ([source](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code)). 


**Audit Recommendations**

The team followed Certik's recommendations by assigning privileged roles to multisig wallets and transitioning to a DAO governance/voting module. The Treasury multisig and Protocol owner multisig (which controls the AdminContract) addresses can be found [here](https://etherscan.io/address/0x5592cB82f5B11A4E42B1275A973E6B712194e239) and [here](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69), respectively.

Certik also advised implementing a time-lock contract with a 48-hour latency for privileged operations. The team has considered this but opted not to give ownership to a time-lock contract to remain agile. A permanent solution would be to renounce ownership or remove the _owner role's ability to affect collateral assets from the stability pool, transfer funds, change vesting periods, or alter the AdminContract. 

The transition to a DAO is still in its early stages with limited community activity outside of the team. Questions could be raised about the fairness of the MON token distribution, given the overlap between core Treasury multisig signers and Grizzly.Fi airdrop recipients. To address this, the team has set up a [multisig proxy address](https://etherscan.io/address/0x83737eae72ba7597b36494d723fbf58cafee8a69) and transferred ownership of the AdminContract to this address, providing transparency to users about the token distribution ([proof of transfer](https://etherscan.io/tx/0xea7d8303eb36885d2446bd3ea73ca64027f5e851c5ba0119fddd0370b3604468)). This provides a reference to help users check whether MON tokens have been distributed as advertised, the Moneta DAO has released documentation for their token distribution (see above section). For more information, check the Moneta DAO's token distribution documentation ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics)).

Here’s an overview of major MON token holders:

![MON_token_distribution](https://user-images.githubusercontent.com/51072084/231855112-c2bea070-c719-4d52-be1a-015fb157ab2e.png)

[Source query](https://dune.com/queries/2320797/3800099)

![MON_token_distribution2](https://user-images.githubusercontent.com/51072084/231855260-c667ec27-9148-4fbd-9e42-035ec92e4978.png)

[Source query](https://dune.com/queries/2320797/3800099)

DCHF is a fork of Vesta Finance, which itself is a fork of the Liquity decentralized borrowing protocol. Although Liquity is a well-audited protocol, engineers from Vesta have cautioned against forking these projects due to messy and unoptimized code that may not be up-to-date with new features ([source](https://github.com/vesta-finance/vesta-protocol-v1/releases/tag/v1.0)). Despite this warning, DCHF remains a fork of Vesta with no visible effort to keep up with changes in the original code base, possibly due to their use of non-upgradeable contracts ([source](https://github.com/monetadao/dchf/tree/main/contracts)).


## **Centralization & Custody Risk**

Transferring ownership of the AdminContract to a multisig wallet partially mitigates centralization risk, but does not fully address it. The transfer of the AdminContract to a multisig has granted it extensive authority over critical protocol functions, raising custody risk. The team has extended the multisig threshold and transitioned to a DAO, which remains to be seen how well it can garner community involvement in decision-making and on-chain governance. 

Additional expansion of the current 4-of-6 multisig threshold ([source](https://etherscan.io/address/0x83737EAe72ba7597b36494D723fbF58cAfee8A69#readProxyContract)) and a plan to further reduce privileges associated with the `_owner` role, which the multisig has full control of, should be developed and communicated. 

However, the team has opted for non-upgradeable contracts, which means the owner (multisig) must add new collateral through the [AdminContract](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code) or update existing collateral parameters through the [DFrancParameters](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code) contract. Although centralization risks found in the audit have been mitigated to an extent, extensive privileges remain available to the core team to exercise.

Presently, the Treasury multisig controls 

* Adding new collateral types
* Full allowance of Community Grants Committee
* Collateral supply and Risk Parameters
* Protocol Updates
* Moving funds in and out of Stability Pool
* Redemption list
* Treasury address
* And more.

The Treasury multisig currently controls many aspects of the protocol, and the Moneta DAO lacks a clear delegation process. As a result, the core team is necessary for the DAO to operate effectively.


## **Governance Risk**

Grizzly.Fi incubated both the DeFi Franc and Moneta DAO, with the latter receiving a treasury seed of 1M USD worth of assets for 9,174,312 MON ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao)), and the team and advisors being allocated 10% or 10m MON ([source](https://docs.defifranc.com/tokenomics/moneta-tokenomics)). 

There are two entities to Moneta DAO governance: Token House and Delegate House ([source](https://monetadao.com/)). As discussed above Moneta DAO token holders will have say over strategic protocol decisions including allocation of DAO treasury funds, critical risk parameters and protocol upgrades ([source](https://commonwealth.im/moneta-dao/discussion/10191-launching-monetadao)). However, the absence of a clear process for prominent community members to serve as delegates and permissioned roles within the DAO compounds this issue. 

Additionally, the distribution of MON tokens was made before the formation of the DAO, without community input. Moreover, given that there is no clear process for delegation, we can assume that the current multisig signers of the Treasury and Protocol Owner proxy are the de facto delegates. While delegates execute the wishes of tokenholders, this relationship is non-binding and trust-based. 

While the token holders will have a say in strategic protocol decisions, the extent of governance remains uncertain. Clear communication of a governance process and plans for delegation will be helpful.


## **Oracle Risk**

DCHF relies on a Chainlink oracle, with the AdminContract able to call the addOracle function, creating a risk if the AdminContract is compromised. Unfortunately, DCHF lacks a backup oracle, relying on the fetchPrice function which interacts with two Chainlink oracles and allows a maximum deviation of 50% ([source, line 148](https://github.com/monetadao/dchf/blob/main/contracts/PriceFeed.sol)). As a result, if one CHF is inaccurately priced, it could lead to negative effects on liquidations and the redemption mechanism, ultimately resulting in potential loss of funds ([source](https://docs.defifranc.com/extras/audits)).


## **Depeg Risk**

The DeFi Franc protocol currently has 6,694,401 DCHF tokens minted, valued at $1.15, totaling approximately $7.6m, and holds collateral worth around $17m (ETH and wBTC combined). Redemptions are supported by sufficient collateral.

![DCHF_minted](https://user-images.githubusercontent.com/51072084/231855722-53a9c0e0-11a1-47ca-b208-edf16eaab5a6.png)

[Query 1](https://dune.com/queries/2322419/3801296), [Query 2](https://dune.com/queries/2297708/3800983)

![DCHF_backing](https://user-images.githubusercontent.com/51072084/231855987-894da348-86ff-4a5c-8111-855a3814467c.png)

[Credit: Etherscan](https://etherscan.io/tokenholdings?a=0x77E034c8A1392d99a2C776A6C1593866fEE36a33)

![DCHF_openposition](https://user-images.githubusercontent.com/51072084/231856182-aeeb2d96-dc3b-4261-a1bc-cec1d629680a.png)

[Source query](https://dune.com/queries/2322141)

However, a minor contract risk exists due to the setDCHFGasCompensation() function potentially resulting in insufficient DCHF tokens to burn from the gas pool, which could prevent redemptions ([source](https://skynet.certik.com/projects/defi-franc)). This has been partially resolved, but still could result in MCR (minimum collateral ratio) > CCR (collateral coverage ratio).

To control the supply of DCHF, the protocol utilizes borrow and redemption fees. A 110% collateral ratio provides a price floor and ceiling, and arbitrageurs are incentivized to maintain price stability. However, a depeg risk exists if demand for DCHF is insufficient and it trades above 1 CHF for a prolonged period.

The protocol does not rely on an external AMM for supply expansion or contraction, and burning/withdrawal amounts are approved through on-chain peg mechanisms. Although the risk of depeg is currently low, it still exists.


# **LlamaRisk Gauge Criteria**

(An analysis of all the above findings are used to answer the following questions, which we ultimately use to determine whether we recommend a gauge or not)

**Centralization Factors**

**1. Is it possible for a single entity to rug its users?**

The risk of a single entity rug-pulling users has been reduced by transferring the AdminContract's extensive privileges to a multisig, which currently has a 4-of-6 signer setup. However, despite this arrangement, there is still a considerable amount of centralization. 

**2. If the team vanishes, can the project continue?**

Currently, activities seem to be well-coordinated with the core team able to coordinate critical protocol functions. With the DAO being recently launched, there is no clear process for selecting delegates, and the forum is only active with a few participants. The project will be unable to continue without the core team. 

**Economic Factors**

**1. Does the project's viability depend on additional incentives?**

While the project may depend on incentives for liquidity (voting for Curve Factory v2 pool to add a gauge), the primary peg stability mechanism is achieved through borrow/redemption fees and 110% minimum collateral ratio to define a soft peg. 

**2. If demand falls to 0 tomorrow, can all users be made whole?**

Yes, there’s roughly $17m in ETH and wBTC collateral supporting $7.6m in DCHF value. Users can redeem their deposits tomorrow. 

**Security Factors**

**1. Do audits reveal any concerning signs?**

The audits did not identify any critical issues, but flagged a few major concerns that have been partially or fully addressed. The primary concern was the overreaching powers of the AdminContract, which has been transferred to multisig signers. It should also be noted that DCHF is a fork of Vespa Finance and Liquity. Although, the DCHF protocol has switched to non-upgradeable contracts, it does have a path to update the system through the AdminContract ([source](https://etherscan.io/address/0x2748C55219DCa1D9D3c3a57505e99BB04e42F254#code)) or make parameter updates through the DFrancParameters contract ([source](https://etherscan.io/address/0x6F9990B242873d7396511f2630412A3fcEcacc42#code)). Nevertheless, it may be challenging for the protocol to implement upgrades in a modular fashion like Vespa Finance and Liquidity ([source](https://github.com/vesta-finance/vesta-protocol-v1/releases/tag/v1.0)).

# Risk Team Recommendation (Don't worry about this section in the first draft, we will discuss together and with the protocol team to determine our final recommendation)

Here are three recommendations that would be beneficial to the DCHF ecosystem and community: \

1. Update documentation to be explicit about MON token holders strategic role in governing DCHF protocol 
2. Be explicit about allowance/powers the Treasury / Protocol proxy multisig continues to have through contracts like AdminContract and DFrancParameter
3. Put in a process for DAO delegates to apply and be nominated from the community.
