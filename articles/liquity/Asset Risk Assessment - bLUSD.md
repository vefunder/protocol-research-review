# Asset Risk Assessment bLUSD
### Chicken Bonds are designed to generate amplified yields for the users and protocol-owned liquidity that can be used to strengthen the subjected asset.


# Liquity protocol overview

*[Resources]*

*[Official Liquity Documentation](https://docs.liquity.org/)*

*[Liquity: Decentralized Borrowing Platform](https://docsend.com/view/bwiczmy) (Whitepaper)*

Liquity is a decentralized borrowing protocol that allows interest-free loans against ETH as collateral. The loans are subjected to Bowworing fees and Redemption fees; both of them being a function of the redemption rate. The borrowing fee is paid in LUSD and ranges from 0.5% to 5% whereas the Redemption fee is paid in ETH and ranges from 0.5% to infinity. As the redemption volume of LUSD increases the fees move away from their lower limit of 0.5%. On top of the Borrowing fees, 200 LUSD is charged as a [Liquidation Reserve](https://docs.liquity.org/faq/borrowing#what-is-the-liquidation-reserve) which will be returned when the debt is repaid. 

![Trove creation demonstration for understanding fee at the time of borrowing](https://lh3.googleusercontent.com/ABVAaBAyu-5EXrFCtXZH8v0QUss7gJF54Dwx6GHHG-e_monuGsCq2-OKxNNXNVeGGhyeKwS_69GR7I8b99PONo-tZG4UTNEwMpYMu1UtlxE5Ds27qdCe6400ig4bYx3043aKMZXqvTInslV7qu9Z551P_DQG2NysLOBndXkSf9AEV250ttxJENfrP22vhg)
Liquity allows a minimum debt of 2,000 LUSD. Considering that the Trove is immediately liquidated upon creation, a maximum loss of 0.5% Borrowing fee will be 18.55% and 21.7% in the case of maximum borrowing fees.  

To avoid liquidations, the debt holder needs to maintain a minimum collateral ratio (CR) of 110% or 150% in the case when recovery mode is active (Recovery mode is further explained at the end of this section). The loans are secured by a Stability Pool containing LUSD that handles the liquidations.

Borrowers on Liquity open and maintain a vault called a “Trove” for their debt positions. The Stability Pool is a source of liquidity that is used to repay debt from liquidated Troves. When any Trove is liquidated, an amount of LUSD corresponding to the remaining debt of the Trove is burned from the Stability Pool’s balance to repay the Trove’s debt. In exchange, the entire collateral from the Trove is transferred to the Stability Pool. The Stability Pool is funded by users (called Stability Providers) transferring LUSD into it. Over time, Stability Providers lose a pro-rata share of their LUSD deposits, while gaining a pro-rata share from liquidated Troves.

![Demand Supply flow-diagram for Liquity protocol](https://lh3.googleusercontent.com/FKGFlQaNnZNWyjdwPHTOZlKd4TYSh6HlDK1kHj_ar6SEb5O2YdwjBNiU5-v25qK_4nt_5We2dQUGO1Dp9VaMwvP2uRdmd0dQxzOxIBl-okYQBCusO_pfHV7L1ol4OrlrTckVT9Yi_WWTyb446LgPr8IFPLIw_PtDrtculTuCp4rc31cUEpkKs0ZK4Ycl9Q)

Liquidation rewards are evenly distributed to the LUSD deposited in the Stability Pool. When the ETH price drops quickly, heavy liquidations might get triggered. If people are not confident with exposure to ETH, they might not deposit their LUSD in the stability pool. When the stability pool is empty, the debt obligation is transferred to other LUSD debt holders, reducing their collateral ratio. In the best case, the debt is transferred at a 110% collateral ratio, but in a flash crash, the ratio might drop lower than 110% reducing the collateral ratio for other debt holders.

In an undesirable scenario where the Stability Pool has no funds to facilitate the liquidations, the debt obligation from a Trove that is deemed to be liquidated might get transferred to other LUSD debt holders. Transferring this debt obligation will reduce the health factor (collateral ratio) of the remaining Troves making them vulnerable to liquidation if the ETH price drops.

![Debt trasnfer in case of insufficient funds under Stability Pool](https://lh5.googleusercontent.com/dsMYpvcTpi81p2KC3WNCcD0cWwUfAjtGPN7Uyo3KoI_vu54ELjImim8tCw7Wjf25iVU-MvIeQPxG8_vOWiXDHCkfqKPmODh0kDXAIXz6UwJ4HnQiQGhDSMxDHqPVUK5kO4zcpfHG9lwrnS5PQY9L0n5SLS8iaF7_-z8aTca7v-xEUadX0DKl0Y7UAI487w)

> The redistribution of the collateral and debt is done in proportion to the recipient Trove’s collateral amount. This means that Troves which are heavily collateralized will receive more debt and collateral from liquidated positions than those with low collateralization, ensuring that the system does not create cascading liquidations.
>
> **Liquity Whitepaper**

The above ensures that Recovery Mode is triggered when risky debts are redistributed to the existing debt holders. Recovery Mode is activated when the protocol's Total Collateral Ratio (TCR) falls below 150%. In this undesirable mode of operation, Troves with a collateral ratio between the current TCR and 110% also become subject to liquidation.

These liquidations are carried at 110% CR and excess collateral can be reclaimed by the borrower.

Recovery Mode generates more liquidation with a capped discount. If the funds in the Stability Pool are not sufficient to set off these heavy liquidations, the LUSD peg might suffer.

Chicken Bonds were launched to ensure there are funds available in the Stability Pool to facilitate liquidation and to ensure the LUSD peg in harsh conditions.

## LUSD peg stability

![LUSD Price history](https://lh5.googleusercontent.com/2NL2bc3JNY3ii4igTPPt90Tr_h2sMYU3OoZeycfVkEwoVYtuSLgOeWJIHFyjiVDMSz90hmJKv3G3BTnWlWba-Ju8hOzqRHXSw1RL95st-2Ds-qy1g-LbhsC6z9XP5mn1U-0hYicwhv_t90C_yWcia0qds0-FJL2QITf-AsAGuX_y-f6KAaJ_coSUz4dw9Q)
Image [Source](https://www.coingecko.com/en/coins/liquity-usd)

![Source: https://www.liquity.org/blog/on-price-stability-of-liquity](https://lh6.googleusercontent.com/7RUygHVopKPhdCqEUUJU5i-TJVCNOktQZl_mfw5vZXwkJ7hFQW9H3T3Ov4odAE_DaTHBMDMWlDxyEo-7tFJgXmlcDvXyS5OjKATSC8fD9RqYFsOS020AG6pAFaukrhCtOP9QJSBaTKTMcjJ6obBpBJthgw-QAdxDGjp2MM-SGBY4LX6B_pg25dagBuDLQw "LUSD hard peg and soft peg distinction")

Image [Source](https://www.liquity.org/blog/on-price-stability-of-liquity)

### Hard peg stability

The ability to redeem LUSD for ETH at face value (i.e. 1 LUSD for $1 of ETH) and the minimum collateral ratio of 110% create a price floor and price ceiling (respectively) through arbitrage opportunities. We call these "hard peg mechanisms" since they are based on direct processes.

In order to ensure that the LUSD price does not drop below $1, there is a 110% minimum collateral ratio. This means that for every $100 worth of LUSD, there must be at least $110 worth of collateral in the system. This creates a price floor of $1 for the LUSD.

If the LUSD price rises beyond $1.10, there would be an arbitrage opportunity with the borrowed LUSD to make a profit by redeeming it for Ether. When redeemed, the system uses the LUSD to repay the riskiest Trove(s) with the currently lowest collateral ratio and transfers the respective amount of Ether from the affected positions to the redeemer. In other words, the Ether is drawn from the Troves’ collateral, starting from the position with the lowest collateral ratio.

### Soft peg stability

Liquity is a system where users view the 1:1 dollar peg of LUSD as a Schelling Point. This means that the system usually returns to this point after temporary deviations. If the price of LUSD goes above $1, it makes borrowing more attractive because the user expects to repay at a rate of $1 or lower. If the price goes below $1, it incentivizes repaying existing debts because this state is likely to be short-lived.

Higher Borrowing fees immediately make new loans less attractive and thus throttle the generation of LUSD if there is not enough demand to keep up with the supply. 

If the redemption mechanism succeeds to push a lower price back to parity, new loans will become more attractive (see above “Parity as a Schelling Point”). 

A certain fraction of the entire LUSD supply will be inside the Stability Pool and thus outside regular circulation. However, the pooled fraction of LUSD may depend on the current LUSD: USD exchange rate. The higher the price of LUSD in USD, the lower will be the (expected) collateral surplus gains in case of liquidations, since the conversion is based on the nominal value of LUSD being equal to USD.  As the price of LUSD approaches USD 1.10, the risk of a potential loss for depositors increases accordingly, and stability deposits become less attractive. In this way, more LUSD will flow into the system.

# Chicken Bonds

*[Resouces]*

*[Chicken Bonds: Self-Bootstrapping Liquidity](https://docsend.com/view/dakurpcuv3259bnx)*

*[Chicken Bond Docs](https://liquity.gitbook.io/chicken-bonds/faq/table-of-content)*

*[A Game Theoretic Model of Chicken Bonds Dynamic](https://github.com/Risk-DAO/Reports/blob/main/Chicken%20bonds%20analysis.pdf)*

Chicken Bonds are a novel mechanism for projects to bootstrap protocol-owned liquidity at no cost while boosting yield opportunities for end users. The amplified value accrual to bTKN is achieved by investing the funds under protocol-owned liquidity and the bonds’ (underlying tokens (TKN)).  

Users may deposit TKN in exchange for an accruing balance of bTKN. At any time, bond-holders can either retrieve their principal foregoing the accrued amount (“chicken out”) or trade it in for the accrued bTKN (“chicken in”). Here, a portion of the TKN acquired by the system backs the bTKN supply, and this portion depends on the time of chicken in.

The protocol operates a Treasury consisting of three logical parts (“buckets”): Pending Bucket, Reserve Bucket, and Permanent Bucket.

-   The Pending bucket can be considered a protocol-controlled vault that stores TKN for the people that are yet to make a decision on their bonds. The Pending bucket is used to invest to generate yield that contributes to the floor price of bTKN.

-   The Reserve bucket can be considered a protocol-controlled vault that stores a portion of the TKN gathered from the former bondholders after a ”chicken in” event. Yields generated with the funds in every bucket are directed towards the Reserve bucket which serves as a backing for the bTKN supply.
    
-   The Permanent bucket can be considered a protocol-owned vault that stores the other portion of the TKN gathered from the former bondholders. Similar to protocol-owned liquidity, funds in this vault decide the maximum liquidity of LUSD that can be transferred to the [LUSD3CRV pool](https://curve.fi/lusd) as liquidity through [Yearn vault](https://yearn.finance/#/vault/0x5fA5B62c8AF877CB37031e0a3B2f34A78e3C56A6). The yield earned by the Permanent bucket is credited to the Reserve bucket.

The schematic shows the behavior of the protocol and what choices the user has when 1000 LUSD are bonded and chicken in at time t1.

![Chicken Bonds mechanism breakdown](https://lh6.googleusercontent.com/ZIZUjpj4yUH2lTPfJgC7jmBVJkkegK4eEksZf_2knaZDFrQNeLCUCS1WAoDeaI436GjWWbrHfILcqGECNYI0ABXLP6f_a4LmG_9xcWareS_WHyWxtC7O_CnUC8DgQ7wWOjl9Zd-FgoR_Lt3VdZ1ld-6oIJcC0fYFBORiaAxgv5Zqee43PycL3LzjGoXIEA)

As a LUSD holder, a user can:

-   Create a LUSD bond and accrue bLUSD
    
-   Trade LUSD for bLUSD on the bLUSD/LUSD-3CRV Curve pool
    

As a bond owner (holding the bond NFT), a user can:

-   Chicken In (claim your accrued bLUSD)
    
-   Chicken Out (cancel your bond, reclaim your LUSD)
    

As a bLUSD holder, a user can:

-   Rebond; trade bLUSD for LUSD on Curve pool and bond again
    
-   Become an LP in the bLUSD/LUSD-3CRV Curve pool


The Reserve bucket backs the bLUSD supply, and thus bLUSD has a growing hard price ﬂoor.

> ... the bonding rules guarantee that the price ﬂoor can only increase over time.
>
> **Chicken bond Whitepaper**

## Amplified yield generation

The yield is generated by investing the funds in two sources:

1.  [B.Protocol](https://www.bprotocol.org/)
    
-   The funds in the Pending bucket are always invested here.
    
-   A portion of the Reserve and the Permanent bucket will also be invested here.
    
-   The funds in B.Protocol are deposited in the stability pool facilitating the liquidations. ETH and LQTY generated are then sold at a discount for LUSD and are redeposited in the stability pool. The LUSD quantity will increase provided liquidations are executed with more than a 100% collateral ratio.

2.  [Yearn LUSD3CRV vault](https://yearn.finance/#/vault/0x5fA5B62c8AF877CB37031e0a3B2f34A78e3C56A6)

-   Maximum funds invested in Yearn will never exceed the amount of funds currently available in the Permanent bucket.
    
-   Funds from the Reserve and the Permanent bucket are invested in the Yearn vault. E.g. if there are 900 in Reserve and 100 in Permanent, up to 100 LUSD can be invested in Yearn. If 100 LUSD is invested, there will be 90 from Reserve and 10 from Permanent in Yearn vault.

The amplified yield comes from the yield generated through the funds in the pending and permanent bucket. Currently, this yield amplification is 10.53 as per the below data ([Liquity.app](https://liquity.app/#/bonds)). The amplification would reduce in the case where the Pending bucket holds fewer funds compared to other buckets.

![LUSD bonds statistics November 2nd 2022](https://lh5.googleusercontent.com/MsqVvA0LBqmYVPRKlVjIC3kahF6jrkEuMwtBdFgkEqhXTJ2odOshXnclbE6Pdp9tLwyb6ET7hoSA80oUDNF7nLlQ-tfPovQWY9FH5WTuoOADWz-J9WbiHGWJxiqxVN1u8_-IKYFzGpagc3o5NF14kmQ06KpoHWyVsjk6P1vVzy-SgqEOglICux0mDAW-Gg)

As of Nov 2nd, 2022

The market price of bLUSD can be influenced by this ability to amplify the yield amount in the pending bucket i.e. funds that are bonded but have yet to chicken in or chicken_out.

The impact of the market price of bLUSD will be discussed further in the article.

## bLUSD accrual mechanism

A user who has bonded LUSD will get nothing but a promise of redeeming accrued bLUSD at the time of chicken-in, foregoing the bonded LUSD. This accrual of bLUSD depends on the variables like time, bonded amount, chicken in fees, accrual parameter (alpha), and redemption price of bLUSD.

Here is the equation:

$$
s(t) = \frac {b}{p_r}(\frac {t}{t + \alpha})
$$

$s(t)$ = Virtual accrual of bLUSD

$b$ = Amount of LUSD bonded

$\alpha$ = Accrual parameter

$t$ = Time in seconds

$p_r$ = Redemption price (LUSD in Reserve Bucket / bLUSD supply)

The amount of LUSD bonded (b) is taken into consideration after reducing the 3% chicken in fees from the actual bond value for the calculation of bLUSD accrual. The accrual rate might vary based on the current redemption price (LUSD in the Reserve bucket / bLUSD supply).

Moreover, there is a function where alpha is reduced to accelerate the bLUSD accrual rate. This is triggered when the weighted average bond age goes beyond the target age.

![Controller logic for triggering the accrual parameter](https://lh4.googleusercontent.com/k3o7OuY1SC95kNCOCsNumC7Lx1gjbdQkyLl4HCL4-oizhUWdB9gK93Hj60Hd0ha8w_7DTDtEdPsZn3WMFSDNIDEX1_kTVOLRmtMwu2P2pOkzYM15ymjMfkJcKKtbvuQLboZMUjxHGmz-FPBTt2MmJJV-6TjarZAhM_kGG16EcnHbRhN_AIHHey4C_2DMfg)

Lowering the alpha would cause the accrual of bLUSD to speed up thus bringing the break-even point and the optimal rebonding time close. This will in turn cause the average bond age to drop which is why the controller is used.

## A consequence of lowering the alpha

A major reason for greater bond age is people not willing to take an action (Chicken in or Chicken out), in other words, people not convinced enough that the bLUSD accrued so far is profitable enough to chicken in. When the [current weighted average bond age](https://dune.com/queries/1460655) is greater than the target age which is 15 days, the controller would lower the alpha.

Lowering the alpha will change the non-profitable chicken in trade to a profitable chicken in trade by accelerating the bLUSD accrual. If we were to create a bond for 1000 LUSD on Nov 2nd the break-even time (value of accrued bLUSD = value of LUSD bonded) considering the market price of bLUSD as $1.20 turn out on Nov 25th.
![Demonstartion of expected bond age with breeak even point and rebonding time with 1000 LUSD as on November 2nd 2022](https://lh6.googleusercontent.com/wl-7yMaLz2OLSAYuQ2vmZCFnKliQOPv5YHwXogI61kj4UM_WeXuy1rJ78K7L8c-J3fVKivEiMUvOe1mReWhDtoNG_uK5Q8tmbg-seV1gIxVvJq5CrRZr0z6mO8-vqRWUZUa7EP8eKtqgNoudnHDrUrcbKxKp7b68CPOlLNWehQ_6iNOlWbW2jl3SYziFXw)

Considering a zero-sum game, if there is only 1 user currently bonding with a considerable amount of LUSD, their non-profitable trade will be converted to a profitable one (based on the mechanic discussed above). When they chicken in, the extra profit (which otherwise would not be generated if  was not to be lowered) will come from the bLUSD inflation.

Now consider the same scenario when there is almost negligible liquidation. The market price might drop beyond the floor price triggering the arb bots to empty the Reserve bucket.

If we were to create a bond for 1000 LUSD on Nov 2nd the break-even time (value of accrued bLUSD = value of LUSD bonded) considering the market price of bLUSD as $1.20 turn out on Nov 25th. This means at the current production of yields, and bucket ratios if people bond more than the current bonded LUSD amount the alpha would get triggered even though the spread b/w the floor price and the market price is large as of Nov 2nd, 2022. What happens at a lower market price is a question.

A [technical paper by The Risk DAO](https://github.com/Risk-DAO/Reports/blob/main/Chicken%20bonds%20analysis.pdf) focused on estimating a fair price for bLUSD assuming fully rational agents (players), who only aim to maximize their profits, and a fully liquid world. They concluded with a simple equilibrium state for this game of chicken bonds. They claim that after a certain period people would not bond anymore, due to non-profitability. A scenario discussed earlier might produce a situation where bonding gets less attractive due to non-profitability.

![Conclusion decribing the Chicken Bonds equilibrium state](https://lh5.googleusercontent.com/YhDfpfJGCjwHeZg8jgClLB9IdStaqiQFTR1nKQ1Nog-vptqm4fJW-4rpKE66hjsx3VwXHUu-_TyOPZFJATLtM5bpIx-S2YbUUiM9ZplOE_5k7q3X4vrAxqMVlnuw68V3CA9xv4eUdnivm59Ze8h1daR5YOeiJE1Y4Af7dlQrVo07JYUPZuU3MNVgcovAEQ)

## Exiting the bonds - Chicken_out

When someone chicken_out the LUSD is paid back by withdrawing the investment positions in the B.Protocol. At the time of heavy liquidation, some Chicken Outs might temporarily revert if the user's bond is greater than the LUSD available.

![Chicken out may revert due to heavy liquidations](https://lh3.googleusercontent.com/PPOAewlKcG5gifPmT6NGino5Bjcyoh4mot-L5Uqcb3EADn84KgfgQ4u3c-P4RXgbQ06izB0bNpRS8sl3FOki4K05Y8tEKWkCRV0cQt-Eu-_jFfKysR8kgX0JgOK-_F4ozjURNH20dopGluUsLm6_WS5YCDg4FUqRo_pMvyrgoSqLkf4XItIiRLrMGukqIg)

# Risks to LPs of bLUSD-LUSD3CRV

The scope was limited to LUSD and bLUSD as a product for this risk assessment. Here are the liquidity pools in consideration: [bLUSD-LUSD3CRV](https://curve.fi/factory-crypto/134) (v2 pool), [LUSD3CRV](https://curve.fi/lusd) (v1 pool), & [3POOL](https://curve.fi/3pool) (v1 pool).

Chicken bonds are in a sense transferring the yield from the bonded funds to the previous bondholders having bLUSD a.k.a the amplified yields.

Other than smart contract risk, LP might face a risk associated with bLUSD price fluctuation, LUSD peg, risks at the point of yield generation, and other assets in the liquidity pool.

### Impermanent Loss

Impermanent loss is basically the loss that is equivalent to the upside gained on holding the token instead of providing liquidity. In other words, it is an opportunity cost that may occur while providing liquidity, due to the price shift of one asset of the liquidity pool, versus holding the assets. It does depend on the ratio in which the liquidity was provided, the amount of liquidity, and the depth of that liquidity pool.

Ideally, bLUSD’s ever-increasing floor price should drive the market price up and LUSD being a pegged asset there exists a clear impermanent loss while providing liquidity. While providing liquidity as 50% in bLUSD and 50% in LUSD, if the market price of bLUSD increases, the ROI generated without considering the trading fees would be less than simply holding bLUSD for the same value.

If there are huge positions chickening in, there is high probability that they find the current bLUSD price suitable to exit and Curve bLUSD-LUSD3CRV is the pool where they would swap their bLUSD at. LP are exposed to impermanent loss when bond holders swap their huge bLUSD position on the Curve pool which will bring the bLUSD price down. 

### The risk associated with the bLUSD price

As we discussed in the earlier section, due to the factors like the creation of fewer bonds, lower yield generation and accelerated bLUSD accrual there exists a chance that the market price of bLUSD drops.

### The risk associated with the LUSD price

When people are not confident in holding ETH in a flash crash there exists a chance that LUSD might get depegged. Based on the historic performance of LUSD, this sounds highly unlikely but it can’t be ignored.

### The risk associated with the yield-generating protocols

bLUSD is exposed to the risks at B.Protocol and Yearn used for generating yield. Loss of funds from these protocol/vaults for any reason can severely impact the bLUSD floor price which is the biggest demand driver for holding blUSD or bonding LUSD.

### Opportunity cost

People trading b/w bLUSD and LUSD doesn’t make sense at a point where people do not bond anymore. This would lead to 0 chicken in fee gauge incentives for LPs almost negligible vAPY.

# Personal note:

The Liquity team is [proposing to increase the amplification](https://dao.curve.fi/vote/parameter/47) factor and the gamma of the [bLUSD-LUSD3CRV](https://curve.fi/factory-crypto/134) along with a [discussion/ proposal to add a gauge](https://gov.curve.fi/t/add-the-blusd-lusd-3crv-pool-to-the-gauge-controller/4351). Both of these promote higher liquidity depth which will facilitate price discovery ensuring less volatility. Since bLUSD market price discovery is essential for the bonders to make informed decisions Curve should acknowledge their proposal for a CRV gauge.
