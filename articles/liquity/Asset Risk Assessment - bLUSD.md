# Asset Risk Assessment bLUSD


### Chicken Bonds directs amplified yields to their bTKN (bLUSD). This report focuses on how safe is it for the users to utilise bLUSD i.e. bonding as well as providing liquidity in bLUSD-LUSD3CRV pool.

Chicken Bonds is designed to generate amplified yields to the bLUSD token and build up protocol-owned liquidity that can strengthen the subjected asset's position in the market. It is first applied to Liquity’s LUSD where bonding is the primary use case, allowing users to acquire bLUSD at a discount upon depositing tokens. 

Index
- Liquity protocol overview
  - LUSD peg stability
    - Hard peg stability
    - Soft peg stability
- Chicken Bonds
  - Yield generation for bLUSD
  - bLUSD accrual mechanism
    - A consequence of lowering the accrual parameter
- Risks to LPs of bLUSD-LUSD3CRV
  - Impermanent Loss
    - The risk associated with the bLUSD price
    - The risk associated with the LUSD price
   - The risk associated with the yield generation
      - B.Protocol - stability pool
      - Yearn LUSD vault
   - Opportunity cost
- Conclusion

The core purpose of this report is to make users aware of the product/services and its risks. This should not, in any way, be treated as financial/investment advice. 

The research was spearheaded by DiligentDeer.

# Liquity protocol overview
_**Useful Links**_

_[Official Liquity Documentation](https://docs.liquity.org/) (Docs)_

_[Liquity: Decentralized Borrowing Platform](https://docsend.com/view/bwiczmy) (Whitepaper)_

_[Trail of Bits Security Assessment](https://github.com/trailofbits/publications/blob/master/reviews/Liquity.pdf) (Audit)_

_[Audit by Coinspect](https://www.coinspect.com/liquity-audit/) (Audit)_

_[Trail of Bits Liquity Protocol and Stability Pool Final Report](https://github.com/trailofbits/publications/blob/master/reviews/LiquityProtocolandStabilityPoolFinalReport.pdf) (Audit)_

Liquity is a decentralized borrowing protocol that allows interest-free loans against ETH as collateral.

Loans are subjected to Borrowing and Redemption fees, both functions of the redemption rate. The borrowing fee is paid in LUSD and ranges from 0.5% to 5%, and the redemption fee is in ETH (ranging from 0.5% to infinity). As the redemption volume of LUSD increases, suggesting LUSD is trading below its peg, borrowing fees begin to increase to discourage borrowing. The redemption fee regulates redemption velocity by increasing when redemptions take place, and decreasing over time since a fee event. On top of the Borrowing fees, 200 LUSD is charged as a [Liquidation Reserve](https://docs.liquity.org/faq/borrowing#what-is-the-liquidation-reserve) to be returned when the debt is repaid. Liquity requires a minimum debt of 2,000 LUSD.

![Trove creation demonstration for an understanding fee at the time of borrowing](https://lh3.googleusercontent.com/ABVAaBAyu-5EXrFCtXZH8v0QUss7gJF54Dwx6GHHG-e_monuGsCq2-OKxNNXNVeGGhyeKwS_69GR7I8b99PONo-tZG4UTNEwMpYMu1UtlxE5Ds27qdCe6400ig4bYx3043aKMZXqvTInslV7qu9Z551P_DQG2NysLOBndXkSf9AEV250ttxJENfrP22vhg)
Source: [Defisaver](https://app.defisaver.com/liquity/smart-wallet/manage)

To avoid liquidations, the debt holder needs to maintain their "Trove" (similar concept to a CPD or vault) at a minimum collateral ratio (CR) of 110% during normal conditions, or 150% in the case when recovery mode is active (Recovery mode is further explained at the end of this section). The loans are secured by a Stability Pool containing LUSD that handles the liquidations.

The Stability Pool is a liquidity source used to repay debt from liquidated Troves. When any Trove is liquidated, an amount of LUSD corresponding to the remaining debt of the Trove is burned from the Stability Pool's balance to repay the Trove's debt. In exchange, the entire collateral from the Trove is transferred to the Stability Pool.

![Demand Supply flow-diagram for Liquity protocol](https://lh3.googleusercontent.com/FKGFlQaNnZNWyjdwPHTOZlKd4TYSh6HlDK1kHj_ar6SEb5O2YdwjBNiU5-v25qK_4nt_5We2dQUGO1Dp9VaMwvP2uRdmd0dQxzOxIBl-okYQBCusO_pfHV7L1ol4OrlrTckVT9Yi_WWTyb446LgPr8IFPLIw_PtDrtculTuCp4rc31cUEpkKs0ZK4Ycl9Q)

In an undesirable scenario where the Stability Pool has no funds to facilitate the liquidations, the debt obligation from a Trove that is deemed to be liquidated might get transferred to other LUSD debt holders. Transferring this debt obligation will reduce the remaining Troves' health factor (collateral ratio), making them vulnerable to liquidation if the ETH price drops further.

![Debt trasnfer in case of insufficient funds under Stability Pool](https://lh5.googleusercontent.com/dsMYpvcTpi81p2KC3WNCcD0cWwUfAjtGPN7Uyo3KoI_vu54ELjImim8tCw7Wjf25iVU-MvIeQPxG8_vOWiXDHCkfqKPmODh0kDXAIXz6UwJ4HnQiQGhDSMxDHqPVUK5kO4zcpfHG9lwrnS5PQY9L0n5SLS8iaF7_-z8aTca7v-xEUadX0DKl0Y7UAI487w)
Source: [Liquity whitepaper](https://docsend.com/view/bwiczmy)

> The redistribution of the collateral and debt is done proportionally to the recipient Trove's collateral amount. Troves with higher collateralization receive more debt and collateral from liquidated positions. This distribution design ensures that the system does not create cascading liquidations.
>
> **Liquity Whitepaper**

Recovery Mode is activated when the protocol's Total Collateral Ratio (TCR) falls below 150%. In this event, the protocol liquidates troves with collateralization ratios between the current TCR and 110%.

These liquidations are carried at 110% CR, and the borrower can reclaim excess collateral. Recovery Mode generates more liquidations with a capped discount. A lack of sufficient funds in the Stability Pool prevents liquidations, potentially leading to the market stress testing LUSD's peg.

The motivation for designing Chicken Bonds was to ensure that the Stability Pool has sufficient liquidity to facilitate liquidation and to ensure the LUSD peg in harsh conditions.

## LUSD peg stability

![Peg stability mechanism](https://lh6.googleusercontent.com/7RUygHVopKPhdCqEUUJU5i-TJVCNOktQZl_mfw5vZXwkJ7hFQW9H3T3Ov4odAE_DaTHBMDMWlDxyEo-7tFJgXmlcDvXyS5OjKATSC8fD9RqYFsOS020AG6pAFaukrhCtOP9QJSBaTKTMcjJ6obBpBJthgw-QAdxDGjp2MM-SGBY4LX6B_pg25dagBuDLQw "LUSD hard peg and soft peg distinction")
Source: [Liquity](https://www.liquity.org/blog/on-price-stability-of-liquity)

### Hard peg stability

- Any LUSD holder has the ability to redeem LUSD for ETH at face value (i.e. 1 LUSD for $1
  of ETH). The system will use the LUSD to pay off debt and draw collateral from the riskiest Troves. Direct arbitrage creates a price floor for LUSD at $1. 
- A minimum collateral ratio of 110% sets an upper price of LUSD at $1.10. Above that price, arbs can deposit $110 worth of ETH and sell 100 LUSD (worth >$110) at a profit. 

### Soft peg stability

- If the price of LUSD goes above $1, borrowing is more attractive as the user expects to repay at a lower rate or arbitrage it on some DEXs.
- Higher redemption volume implies higher Borrowing fees which make new loans less attractive, thus limiting the supply.
- As LUSD moves close to $1.10, the gains from liquidations on the stability pool will taper off; users would remove LUSD from the stability pool, increasing its circulating supply.

In practice, LUSD has typically remained rangebound, as predicted, between the $1 - $1.10 "hard peg". 
![LUSD Price history](https://lh5.googleusercontent.com/2NL2bc3JNY3ii4igTPPt90Tr_h2sMYU3OoZeycfVkEwoVYtuSLgOeWJIHFyjiVDMSz90hmJKv3G3BTnWlWba-Ju8hOzqRHXSw1RL95st-2Ds-qy1g-LbhsC6z9XP5mn1U-0hYicwhv_t90C_yWcia0qds0-FJL2QITf-AsAGuX_y-f6KAaJ_coSUz4dw9Q)
Source: [Coingecko](https://www.coingecko.com/en/coins/liquity-usd)

---

# Chicken Bonds


_**Useful Links**_

_[Chicken Bonds: Self-Bootstrapping Liquidity](https://docsend.com/view/dakurpcuv3259bnx) (Whitepaper)_

_[Chicken Bond Docs](https://liquity.gitbook.io/chicken-bonds/faq/table-of-content) (Docs)_

_[A Game Theoretic Model of Chicken Bonds Dynamic](https://github.com/Risk-DAO/Reports/blob/main/Chicken%20bonds%20analysis.pdf) (Technical paper)_

_[Coinspect - Smart Contract Audit](https://github.com/liquity/ChickenBond/blob/main/LUSDChickenBonds/audits/Coinspect%20-%20Smart%20Contract%20Audit%20-%20Liquity%20ChickenBond.pdf) (Audit)_

_[Coinspect - Smart Contract Audit v2](https://github.com/liquity/ChickenBond/blob/main/LUSDChickenBonds/audits/Coinspect%20-%20Smart%20Contract%20Audit%20-%20Liquity%20ChickenBonds%202nd%20v220803.pdf) (Audit)_

_[Coinspect - Smart Contract Audit v3](https://github.com/liquity/ChickenBond/blob/main/LUSDChickenBonds/audits/Coinspect%20-%20Smart%20Contract%20Audit%20-%20Liquity%20ChickenBonds%203rd%20v220929.pdf) (Audit)_

_[Dedaub - Smart Contract Audit](https://github.com/liquity/ChickenBond/blob/main/LUSDChickenBonds/audits/Dedaub_Chicken%20Bonds%20Audit.pdf) (Audit)_

_[B.AMM Protocol Liquity Integration Assessment](https://github.com/Fixed-Point-Solutions/published-work/blob/master/SmartContractAudits/FPS_B.AMM_Liquity_Assessment_FINAL.pdf)(Audit)_

Chicken Bonds are a novel mechanism for projects to bootstrap protocol-owned liquidity at no cost while boosting yield opportunities for end users.

Users may deposit LUSD in exchange for an accruing balance of bLUSD. At any time, bond-holders can either retrieve their principal and forego the accrued amount ("**chicken out**"), or trade it in for the accrued bLUSD ("**chicken in**"). Here, a portion of the LUSD acquired by the system backs the bLUSD supply, and this portion depends on the time of chicken in.

The protocol operates a Treasury consisting of three logical parts, termed "buckets": Pending Bucket, Reserve Bucket, and Permanent Bucket.

- The Pending Bucket (protocol-controlled vault) stores LUSD for the people who have yet to decide on their bonds. The funds are invested in the stability pool.
- The Reserve bucket (protocol-controlled vault) stores a portion of the LUSD gathered from the former bondholders after a "chicken in" event. Yields generated with the funds in every bucket are directed here, which supports bLUSD's supply.
- The Permanent bucket (protocol-owned vault) stores the other portion of the LUSD gathered from the former bondholders.

The schematic shows the protocol's behaviour and the user's choices when 1000 LUSD are bonded and chicken in at time t1.

![Chicken Bonds mechanism breakdown](https://lh6.googleusercontent.com/ZIZUjpj4yUH2lTPfJgC7jmBVJkkegK4eEksZf_2knaZDFrQNeLCUCS1WAoDeaI436GjWWbrHfILcqGECNYI0ABXLP6f_a4LmG_9xcWareS_WHyWxtC7O_CnUC8DgQ7wWOjl9Zd-FgoR_Lt3VdZ1ld-6oIJcC0fYFBORiaAxgv5Zqee43PycL3LzjGoXIEA)

The Reserve bucket backs the bLUSD supply, and thus bLUSD has a growing hard price ﬂoor.

> ... the bonding rules guarantee that the price ﬂoor can only increase over time.
>
> **Chicken bond Whitepaper**

## Yield generation

The yield is generated by investing the funds in two sources:

1. [B.Protocol](https://www.bprotocol.org/)

- The funds in the Pending bucket are always invested through B.protocol.
- A portion of the Reserve and the Permanent bucket will also be invested here.
- The funds in B.Protocol are deposited in the stability pool and facilitate liquidations. ETH and LQTY generated are sold at a discount for LUSD and are redeposited in the stability pool. The LUSD quantity will increase provided liquidations are executed with more than a 100% collateral ratio.

2.  [Yearn LUSD3CRV vault](https://yearn.finance/#/vault/0x5fA5B62c8AF877CB37031e0a3B2f34A78e3C56A6)

- LUSD in the Yearn vault is deposited into LUSD3CRV-f, and the LP tokens will be supplied to [Convex Finance](https://www.convexfinance.com/stake) to earn CRV and CVX (and any other available tokens). Earned tokens are harvested and sold for more LUSD3CRV-f, which is deposited back into the strategy.

- The transfer of LUSD to the LUSD3CRV Curve pool can be used as a peg reinforcing mechanism by moving funds in the Reserve bucket and Permanent bucket to Yearn vaults to balance the demand and supply of LUSD in the LUSD3CRRV pool. Funds deposited in Curve would never exceed the quantity of LUSD present in the Permanent bucket. Learn more about the shifter function [here](https://github.com/liquity/ChickenBond#shifter-functions). E.g. if there are 900 in Reserve and 100 in Permanent, up to 100 LUSD can be invested in Yearn. If 100 LUSD is invested, there will be 90 from Reserve and ten from Permanent in Yearn vault.

## bLUSD accrual mechanism

A user who bonds LUSD will get nothing but a promise of redeeming accrued bLUSD at the time of chicken-in, foregoing the bonded LUSD. This accrual of bLUSD depends on the variables like time, bonded amount, chicken in fees, accrual parameter (alpha), and redemption price of bLUSD.

This is how a change in accrual parameter looks on a graph:
**![](https://lh4.googleusercontent.com/BWceh8sIda2E5deOwTxHul32KGtKhhySKozv33WRKaSvnokLdQb5zFLI06S2md50UaoWrvHrUF3crY4nefMyTdmnjBnAab_Xo8PUm5KbaaUljndUnHKeteO1FfwpydGA5lcGEqwDIPgCFc2urkdpuqi3DWfq2YKSGIDOBfch_N6AtDpNCUqKmQ26iYEQhw)**

The amount of LUSD bonded is considered after reducing the 3% chicken in fees from the actual bond value for the calculation of bLUSD accrual. The accrual rate might vary based on the current redemption price (LUSD in the Reserve bucket / bLUSD supply).

Moreover, there is a function where alpha is reduced to accelerate the bLUSD accrual rate. When the [current weighted average bond age](https://dune.com/queries/1460655) is greater than the target age of 15 days, the controller will lower the alpha. A major reason for greater bond age is people unwilling to take action (Chicken in or Chicken out); in other words, people aren't convinced that the bLUSD accrued so far is profitable enough to chicken in.

![Controller logic for triggering the accrual parameter](https://lh4.googleusercontent.com/k3o7OuY1SC95kNCOCsNumC7Lx1gjbdQkyLl4HCL4-oizhUWdB9gK93Hj60Hd0ha8w_7DTDtEdPsZn3WMFSDNIDEX1_kTVOLRmtMwu2P2pOkzYM15ymjMfkJcKKtbvuQLboZMUjxHGmz-FPBTt2MmJJV-6TjarZAhM_kGG16EcnHbRhN_AIHHey4C_2DMfg)
Source: [Controller logic for triggering the accrual parameter](https://github.com/liquity/ChickenBond#controller)

Lowering the alpha would cause the accrual of bLUSD to speed up, thus bringing the break-even point and the optimal rebonding time close.

## A consequence of lowering the alpha

Lowering the alpha will change the non-profitable chicken in trade to a profitable one by accelerating the bLUSD accrual.

Considering a zero-sum game, if only one user is currently bonding with a substantial amount of LUSD, their non-profitable trade will be converted to a profitable one. When they chicken in, the extra profit (which otherwise would not be generated if $\alpha$ was not lowered) will come from the bLUSD inflation, i.e. bLUSD market price.

A [technical paper by The Risk DAO](https://github.com/Risk-DAO/Reports/blob/main/Chicken%20bonds%20analysis.pdf) focused on estimating a fair price for bLUSD assuming fully self-centric, rational agents (players) and a fully liquid world. They concluded with a simple equilibrium state for the game of chicken bonds.

They claim that after a certain period, people would not bond anymore due to non-profitability.

Lowering $\alpha$ might produce a situation where bonding gets less attractive due to non-profitability.

![Conclusion decribing the Chicken Bonds equilibrium state](https://lh5.googleusercontent.com/YhDfpfJGCjwHeZg8jgClLB9IdStaqiQFTR1nKQ1Nog-vptqm4fJW-4rpKE66hjsx3VwXHUu-_TyOPZFJATLtM5bpIx-S2YbUUiM9ZplOE_5k7q3X4vrAxqMVlnuw68V3CA9xv4eUdnivm59Ze8h1daR5YOeiJE1Y4Af7dlQrVo07JYUPZuU3MNVgcovAEQ)
Source: [Technical paper by The Risk DAO](https://github.com/Risk-DAO/Reports/blob/main/Chicken%20bonds%20analysis.pdf)

---

# Risks to LPs of bLUSD-LUSD3CRV

Besides smart contract risk, LP might face a risk associated with bLUSD price fluctuation, LUSD peg, risks at the point of yield generation, and other assets in the liquidity pool.

### Impermanent Loss

Impermanent loss refers to the opportunity cost incurred when providing liquidity to a pool due to the price shift of one asset versus holding the assets. It does depend on the ratio in which the liquidity was provided, the amount of liquidity, and the depth of that liquidity pool.

The following cases can lead to IL:

- Based on the claim that bLUSD's floor would always rise, The change
  in the price of bLUSD, w.r.t LUSD would generate IL.
- Variation in bLUSD and LUSD's price gap.

#### The risk associated with the bLUSD price

Low liquidity on bLUSD-LUSD3CRV will cause a higher price impact. A higher price impact will lead to a higher bLUSD price drop when selling bLUSD.

With ever-increasing floor price, the gap between bLUSD's market price and the floor price would increase if:

- Bonding volume is greater than the chicken in volume.
- More users bond, i.e. more funds in pending bucked, thus high amplification.
- Users chicken out after bonding for a considerable time.
- Users chicken in early.

If these factors are not true, it is likely that the market price of bLUSD will drop and trade near the floor price in the long run. When the market price trades near the floor price, trading activity between bLUSD & LUSD will slow down, resulting in an opportunity cost for the LPs.

It is important to note that lowering the accrual parameter will lower the market price of the bLUSD near the floor price due to the high supply in the system.

#### The risk associated with the LUSD price

When people are not confident in holding ETH in a flash crash, there exists a chance that LUSD might get depegged.
Though the historical performance of LUSD has been prolific, it can't be ignored.

### The risk associated with the yield-generating protocols

bLUSD is exposed to the risks at B.Protocol and Yearn used for generating yield. Loss of funds from these protocols/vaults for any reason can severely impact the bLUSD floor price.

#### B.Protocol


_**Useful Links**_


_[B.AMM Protocol Liquity Integration Assessment](https://github.com/Fixed-Point-Solutions/published-work/blob/master/SmartContractAudits/FPS_B.AMM_Liquity_Assessment_FINAL.pdf) (Audit)_

_[Dedaub - B.protocol Chicken Bond integration](https://github.com/liquity/ChickenBond/blob/main/LUSDChickenBonds/audits/B.Protocol%20-%20Chicken%20Bonds%20Audit.pdf) (Audit)_

The B.Protocol's integration with Liquity was audited in July 2021 and currently has an ongoing [Immunefi bounty of $100k](https://immunefi.com/bounty/bprotocol/). Moreover, the integration has been live on the main net since Aug 2021.

The LUSD ChickenBonds Protocol makes use of B.Protocol to store funds in the stability pool whilst generating yield.

Since liquidations are driven based on the price of collateral, oracle is a key point to consider. The audit report clears the assumptions (potential risk vectors if they don't hold true) that might affect the liquidation process as a whole.

Here is the audit report to learn more: [B.AMM Protocol Liquity Integration Assessment](https://github.com/Fixed-Point-Solutions/published-work/blob/master/SmartContractAudits/FPS_B.AMM_Liquity_Assessment_FINAL.pdf).

![](https://lh6.googleusercontent.com/3SIAUAnnITcZE3V9qYz1F3s6jYA4ApwURZco1KJRsEGMEQ6NvdEWvJCj44IzGgTnyGZsVtKKuxL1iZAXMG_jydNTejx2dIcxZJmA9_H9DTWiXrWgzHXokylaHlzYqsX0JD7fD5bLS_-NyXcwoWjBXvYu6g0LrSJ7OPrAlVO1tbUS8bHjT9dKnmufi9Aaig)
Source: [B.AMM Protocol Liquity Integration Assessment.](https://github.com/Fixed-Point-Solutions/published-work/blob/master/SmartContractAudits/FPS_B.AMM_Liquity_Assessment_FINAL.pdf)

#### Yearn LUSD vault

Stacking different protocols for yield generation can increase the risk surface as well. The vault contract has been live for more than five months. Though Yearn has suffered a [hack in the past](https://thedefiant.io/yearn-loses-11m-in-2021s-first-defi-hack), its since been patched and is now reasonably battle tested, but there exist contract risk that can lead to losses.

### Opportunity cost

As we discussed in the earlier sections the system might reach a state where bonding will not be profitable. Thus negligible chicken in fees, and when bonding-rebonding doesn't happen, trades b/w bLUSD-LUSD are hardly expected resulting in negligible vAPY.


# Conclusion

Chicken bonds and Liquity integration heavily rely on the Curve protocol when it comes to yield generation and price discovery of bLUSD. 

Impermanent loss resulting from the price volatility of bLUSD, and the opportunity cost resulting from negligible trading activities are risks that shouldn't be overlooked.

Risks associated with stability pool investment (B.Protocol) and LUSD price can be considered as black swan events but cannot be ignored.

