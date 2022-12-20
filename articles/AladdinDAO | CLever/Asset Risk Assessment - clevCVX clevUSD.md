
# Asset Risk Assessment - clevCVX & clevUSD 

### CLever allows users to create non-liquidating leveraged positions by borrowing clevTKNs, which represent future yields of the underlying asset.

## Index

- CLever Protocol Overview 
	- Furnace (Facilitating extended swap from clevTKN to TKN) 
- clevCVX 
	- The clevCVX-CVX peg 
- clevUSD 
	- Concentrator Vaults 
	- The clevUSD-FRAX peg  
- Centralization Vectors 
- Conclusion

# CLever Protocol Overview

**Useful Links**

* [Gauge Proposal](https://gov.curve.fi/t/proposal-to-add-clevusd-fraxbp-clevcvx-cvx-clev-eth-pools-to-the-gauge-controller/4632) for clevCVX and clevUSD pools on Curve.
* [CLever Docs](https://docs.aladdin.club/clever)
* [CLever Medium](https://medium.com/@0xC_Lever) Blog
* [Audit Reports](https://github.com/AladdinDAO/aladdin-v3-contracts/tree/main/audit-reports) - Compilation on Github 

[CLever](https://clever.aladdin.club/#/clever) enables leverage farming through the use of a yield-bearing asset (TKN) or its derivative by allowing users to borrow its future yield in the form of clevTKN. ClevTKN can be redeemed for TKN using the [Furnace](https://clever.aladdin.club/#/furnace) or exchanged in a liquidity pool ([Curve](https://curve.fi/#/ethereum/pools/factory-v2-209/deposit) & [Balancer](https://app.balancer.fi/#/ethereum/pool/0x69671c808c8f1c1490a4c9e0145884dfb5631378000200000000000000000392)). The system considers clevTKN to be equivalent to the collateral (TKN), so it carries no risk of liquidation and does not require a price oracle. To leverage farm, a user can deposit TKNs in a strategy vault, borrow clevTKN, swap it for TKN, and repeat the process.

Depositors can borrow up to 50% of their CVX in clevCVX or up to 30% of their USD Concentrator LP as clevUSD. If borrowers choose to repay their debt before future yields are earned, a repayment fee will be applied (Currently 5% for clevCVX and .5% for clevUSD). In the case of CVX, depositors must make an unlock request to withdraw their deposit. CVX is locked in 16-week epochs as vlCVX for yield generation purposes, so the withdrawal processing duration depends on the remaining lock time.

## CLEV and veCLEV

CLEV is the native token for CLever protocol with a capped supply of 2 million. veCLEV is locked CLEV, similar to a vote escrow model pioneered by Curve Finance. Clever charges a fee on the yield harvest instead of charging interest on the debt. The majority of this fee goes to veCLEV holders.

The token distribution for CLEV is as follows:

![](https://miro.medium.com/max/875/1*U9C04jrl3PYTJNwT6cvgQw.png)

Source: Medium article by CLever - [A CLever Token Offering](https://medium.com/@0xC_Lever/a-clever-token-offering-2d775943e23c)

## Furnace (Facilitating extended swap from clevTKN to TKN)

The [Furnace](https://clever.aladdin.club/#/furnace)  can be used to exchange clevTKN for TKN by simply depositing clevTKN in it. If there are sufficient tokens available for claiming in the Furnace, redemption is facilitated immediately at 1:1. If the Furnace is oversubscribed, meaning more clevTKN is awaiting redemption than available TKN, the swap time is determined by the APR of all collateral in the strategic vault and by how many other claimants are processing redemptions. 

With each yield harvest, a portion of clevTKN is converted into additional TKNs, a fee is charged on this yield harvest, and the TKNs are gradually (over 2 weeks) streamed into the furnace to facilitate the exchange from clevTKN to TKN and to distribute yields to TKN depositors in the form of clevTKN. When TKNs are streamed into the furnace, an equal amount of clevTKN is minted. The newly minted clevTKN is given to TKN depositors as yield, and the new TKNs (generated from a yield-bearing asset) are used to facilitate the exchange from clevTKN to TKN.

If the amount of deposited clevTKN is greater than the TKN generated from the yield, it can take a substantial amount of time to exchange the tokens. Indeed, it is possible that oversubscription of the Furnace will occur due to the utility of clevTKN. **clevTKN will likely trade at a discount unless there are no clevTKN left in the furnace.** Conversely, as the number of clevTKN deposited in the furnace approaches 0; the value of 1 clevTKN in the market will approach the value of 1 TKN.

If a user wishes to exchange their clevTKN (yields or borrowings), they have two options. They can use a liquidity pool, although this can result in a loss (if clevTKN is valued lower than TKN). Alternatively, the user can use the furnace to redeem their clevTKN, but this will require them to pay an equivalent amount in time. The choice between "loss of time" or "loss of value" quoted above is significant to the expected valuation for clevTokens. The annual percentage rate (APR) numbers on the CLever website are calculated under the assumption that clevTKN is equal to TKN, which is unlikely to be achieved in practice.

**![](https://lh5.googleusercontent.com/9nzn-Vu6KKO3ocaGLGa87OCsNnjakAOBmlN5Lrox0A-YF-Sp6urXZMCmUhRw7uqvIkhMGQTg4g7dOfHhCj1oAeaK0I4tkTvyT-oWh6hmMwynkUKWnnxPJyChvulVOWFWBQvTIqLfB8BFiUxJ_VR1kx6qP2snM2hg2yW_hvel50De9ca46Ewyd2sbg_7ZoQ)**
Source: Clever - [clevCVX Vault](https://clever.aladdin.club/#/clever/cleverCVX)

In the above image, the 33% APR is calculated based on the assumption that the clevCVX is equivalent to CVX which is empirically false.

Liquidity incentives in the form of CLEV (48.25% allocation) can be used to limit the deposit of clevTKN into the furnace and improve liquidity. Implementing a Curve gauge to distribute CRV rewards on the clevTKN-TKN liquidity pool can also help with liquidity and limit the deposit of clevTKN in the furnace.


CLever currently has two strategic vaults:
1. clevCVX
2. clevUSD

# clevCVX

**Useful Links**

* CLever [clevCVX Vault](https://clever.aladdin.club/#/clever/cleverCVX)
* CLever Medium Blog explaining clevCVX strategy - [A CLever Token Offering](https://medium.com/@0xC_Lever/a-clever-token-offering-2d775943e23c)

The clevCVX strategy has the following key attributes:

- The locked version of CVX (vlCVX) is used as the yield-generating asset.
- The yield is derived from bribes (Votium delegation) and vlCVX rewards (a share of the Convex platform fee).
- Users can borrow up to 50% of their collateral value.
- Withdrawals may take up to 17 weeks, depending on the vlCVX unlock date.
- The current harvest fee is 2% and will soon be increased to 20%.
- Early Repayment fee = 5%

Here is a comparison of the three different strategies with CLever:

1)  **CVX Locking without borrowing**: This strategy involves locking CVX without borrowing any additional funds.
	-  Users may use this strategy to stay relatively liquid compared to other strategies that are possible with CLever
2)  **Maximum borrowing without depositing in the furnace (farming elsewhere)**: This strategy involves borrowing the maximum amount possible, but not depositing the borrowed funds into the furnace. Instead, the borrowed funds are used for farming elsewhere.
	- Users may deposit their borrowed clevTKN into a liquidity pool to capture the incentives. This can be a viable option in the case when a user feels that yield from farming can generate better returns than simply putting their clevTKN into the furnace to swap it for TKN. 
	- This kind of farming is only viable when a borrower is confident with their leverage position.
3)  **Maximum borrowing and depositing in the furnace (leverage farming)**: This strategy involves borrowing the maximum amount possible and depositing the borrowed funds into the furnace, also known as leverage farming.
	- Users may deposit their borrowed clevTKN into the furnace when the swaps (from clevTKN to TKN) take less time to execute. Users may choose this strategy when the yields from liquidity farming are diluted and the furnace offers a better deal to swap their clevTKN to TKN. 

Compare the above strategies with manual vlCVX harvesting:
-  **Votium Delegation**: This strategy involves delegating vlCVX to Votium to generate bribe yield denominated in CVX.
	- This strategy doesn't use CLever but is more of a comparison to understand a perspective of opportunity cost.
 
**![](https://lh4.googleusercontent.com/oH9GcxQRyTs-KWWvucB_m8E7UvfR_MQlcZinyhKb5112vBfGfDnzBM1klfa6ZAwfkeoPa7nn7uE5fzkl8NYctkQks_6W11QAW7hcaBe0Mz1NdcgC3-BADNpNV8hxKweJmzEAKoc5BA1pcON5ol8fV_L394v0jzvmr5ujBJk8rHEg0jO8kZ40TsfgxmGjnA)** 

Source: [Google sheet](https://docs.google.com/spreadsheets/d/1eeGXJ_GCIPc9YVCqt61F8TISN4K59DdRPD0xcrPiKsg/edit?usp=sharing) by @diligentdeer

Each of these strategies has its pros and cons, and it is up to the individual to determine which strategy is the most suitable based on their specific goals and circumstances.

As we can see in the sheet above, depositing 1000 CVX can generate a yield of 12 clevCVX whereas 1000 vlCVX delegated on Votium will yield almost 15 CVX. It can be concluded from the above that, **with a 20% harvest fee, it is not advisable to use CLever without borrowing.**

With this [Desmos Simulator](https://www.desmos.com/calculator/vtc88qmbdz), you can analyze your position with almost all the variables involved in clevTKN vaults. Moreover, it will also show you how much time it takes to repay the debt as well as the time it would take to swap your clevTKN deposit in the furnace.

## The clevCVX-CVX peg

Aside from redemption in the Furnace, the only utility of clevCVX is to provide liquidity on Curve or Balancer. As a result, there is a high likelihood that users will deposit clevTKN in liquidity pools to farm the liquidity mining rewards.

Several factors can affect demand for clevCVX:
1)  The amount of time it takes for the Furnace to swap clevCVX for CVX. If it takes a long time to complete this swap, the market may value clevCVX lower than CVX because of the time value of money.
2)  The amount of clevCVX provided as liquidity in a pool. If a large proportion of clevCVX is used as liquidity, the price of clevCVX in the liquidity pool may decrease. However, this may result in favorable conditions for redemption in the Furnace, increasing demand for clevCVX. 

Eventually, there will be a balance between these two factors. The key variables defining this balance are the incentives to store clevCVX in either the furnace or the liquidity pool. If this balance is disrupted, users may experience impermanent loss (pool imbalance) and price impact (lower liquidity) on DEXs, or as an opportunity cost (investing for a longer period to receive a small amount of rewards).

Another factor that can contribute to volatility is the yield obtained from bribes. The possibility of decreasing bribe yields may cause users to unknowingly take on greater maturity risk with their self-repaying debts. Eventually, users may shift their clevCVX to the furnace to exit the system, disrupting the previously discussed balance.

# clevUSD

**Useful Links**

* CLever clevUSD Vaults - [Frax-USDC](https://clever.aladdin.club/#/clever/Frax-USDC), [LUSDFraxBP](https://clever.aladdin.club/#/clever/LUSDFraxBP), [TUSDFraxBP](https://clever.aladdin.club/#/clever/TUSDFraxBP)
* CLever Medium Blog explaining clevUSD - [FRAX and CLever: The Future of Stable Farming is Now](https://medium.com/@0xC_Lever/frax-and-clever-the-future-of-stable-farming-is-now-c03501f941b6), [Higher Stablecoin Yields: Get CLever with Frax](https://medium.com/@0xC_Lever/higher-stablecoin-yields-get-clever-with-frax-7cc0d72a848d)

The clevUSD strategy has the following key attributes:

 - Staked LP tokens of [FRAXUSDC](https://curve.fi/#/ethereum/pools/fraxusdc/deposit), [LUSDFRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-137/deposit), and [TUSDFRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-144/deposit) on Concentrator act as a yield-bearing token.
- The yield is derived from the Concentrator compounded yield from Curve and Convex.
- Users can borrow up to 30% of the future yield.
- Withdrawals can be instant.
- The current harvest fee is 2% (beta period) and will soon be increased to 20% once there is a gauge allocated to the liquidity pool.
- CLever USD will limit total borrowing during the beta period to $100K.
- Early Repayment fee = .5%

CLever deposits the user's collateral to the selected autocompounding Concentrator strategy vault. CLever periodically claims the accrued yield from Concentrator, converts it to FRAX, and distributes an equivalent amount of newly minted clevUSD proportionally to all depositors. clevUSD holders can redeem their tokens for FRAX in the Furnace in the same manner as with CVX.

Though the yield from multiple Concentrator vaults is harvested within the clevUSD strategy, if one of those vaults get exploited, only deposits of that particular LP are affected. The overall yield generation will also get affected negatively and consequently, clevUSD might trade at a high discount due to lower yield.

## Concentrator Vaults

**Useful Links**
* [Concentrator Docs](https://docs.aladdin.club/concentrator)
* [Concentrator Medium](https://medium.com/@0xconcentrator_29486) Blog

[Concentrator](https://concentrator.aladdin.club/#/vault)  is a system that accepts Curve LP tokens and deposits them into one or more listed Convex vaults. These vaults regularly harvest all associated rewards, convert them to cvxCRV, and move them into a concentrator vault. In the concentrator vault, the cvxCRV is staked on Convex and compounded into more cvxCRV.

Several variables that can impact the yield from concentrator vaults (FRAXUSDC, LUSDFRAXBP, and TUSDFRAXBP):

- The trading volume on the liquidity pool specific to these vaults.
- The gauge weight on the liquidity pool specific to these vaults.
- The overall trading volume on Curve Finance.
- The price of different yield tokens.

The yields from Concentrator are subject to fees. The fees for these concentrator vaults can be found [here](https://docs.aladdin.club/concentrator/acrv-ifo-vaults).

Concentrator has undergone several [audits](https://github.com/AladdinDAO/aladdin-v3-contracts/tree/main/audit-reports), so precautions against smart contract exploitation seem reasonable.

## The clevUSD-FRAX peg 

As discussed in the clevCVX-CVX peg section, the market will eventually reach a balance between the time value for the Furnace to swap clevUSD for FRAX, the discount on clevUSD in liquidity pools, and the incentives to store clevUSD in either the Furnace or the liquidity pool. Whenever the variables change, it will impact this balance, ultimately affecting the pricing of clevUSD-FRAX.

Due to several layers of swaps between assets and variables to generate yield, it is hard to speculate on the behavior of the market. With CVX gauge weighting from FRAX, possible CRV rewards from Curve, liquidity mining rewards from CLever, and a low issuance cap of 100K clevUSD, the initial discount on clevUSD is likely to be reasonably modest.

# Centralization Vectors

The [CLever treasury](https://etherscan.io/address/0xFC08757c505eA28709dF66E54870fB6dE09f0C5E) is controlled by a 6/9 multisig. More information about the wallet address and other details can be found at [AladinDAO GitHub](https://github.com/AladdinDAO/deployments/blob/main/deployments.mainnet.md#governance-multisigs). Additionally, background information about the parties involved in the multisig can be found below.

**![](https://lh6.googleusercontent.com/d02A8hyq97R6SgLQL7BURUfjwzyUJc_OSEuNpsp64ogOU268xxaACWPQSbMrShzgRxLg7_g7VGRe1aOEBb8z0-EHgpDnmLePbtGFmS-4zt506AJXOzeUC9QVcAf1HD9WpISa4eJucHhUH5CclRtMUFPuGgmBA993LEs7AOJ5xsWXvoTNprpHrjMK0rXptg)**

The CLever smart contracts are designed to be upgradable and allow for the modification of certain parameters such as platform fees, debt ceiling, collateral ratio, and repayment fees. 

Additionally, CLever can revoke its vlCVX delegation from Votium and make use of alternative bribe venues. Actions such as this would involve community discussion and consultation with tokenholders via Snapshot vote. Note, however, that all authority on governance matters ultimately rests with the CLever multisig.

**![](https://lh5.googleusercontent.com/jlKbp814RPv0A5ftLYuL4h6X6FHnWWnom1nz_gFp5_2QHahGvV_6XImWIAW5JUSAFcP5mrgU4rj_Oy6aVM_PqqtfVMaPDHRezwRiDMGAGclFsmFFb1QZm2tSqhj064Oo-w1CJyfvYLbKO4riMLOqMiC8PvrO3XhzrxDK3U5WkbqjO67E_ggqgIfFZKeUyQ)**

~~With the token distribution, CLEV holding is centralized in the initial phase where DAO votes can be manipulated through veCLEV holdings from Aladdin DAO & Treasury Reserve. Following graph shows Aladdin DAO & Treasury Reserve under **Centralized CLEV**, Partnership offerings, and rest all allocation under **Community CLEV**.~~ 


![](https://lh4.googleusercontent.com/h0lJpKdaSiXISEhEriu-736CfVQJFGsUndfnkq4Tb85Q4X6OwlywLROfNlDfukiM8bKmquhWeB0XXUpgddx0wxWbAIrtKezQnGw-xzBmNXXy6YDNITFESp2CtK4G3UC-P_AIXiai33GhCiHImazlgtyKv_x0IcrfCYLogebRuMJs2WkJspid_1XDlJox1w)

Source: [Google Sheet](https://docs.google.com/spreadsheets/d/1JtXtL26nCaC5js5WFYCZrzBMXIjhtBnY8s9VeKnsXUs/edit?usp=sharing) by @diligentdeer

# Conclusion

- clevTKN circulating supply will eventually reach the amount of CR (collateral ratio) x TKN locked in CLever vault.
- Users are exposed to maturity risk when holding clevTKN. This maturity risk can be realized through time or value because the clevTKN can rarely trade at the same value as TKN unless the amount of clevTKN deposited in the furnace is less than the TKN generated from the yield.
- The inverse relationship between the quantity of TKN in the Furnace and the presumed market price of clevTKN suggests that a high reliance on liquidity incentives will be required to support the peg.
- When the incentives for providing clevTKN as liquidity are similar to that of clevTKN redemption in the furnace, the system will achieve an equilibrium. However, if this equilibrium is disrupted, users may suffer losses in the form of impermanent loss, price impact on DEXs, or opportunity cost.
- LPs are likely to experience impermanent loss as yield generation may be highly volatile (one of the variables that can distort the equilibrium stated in the above point), despite claims made in their blog.
> ...For the liquidity farmer, this is an important distinction, because it means that, on a long enough time scale, both your CVX and your clevCVX are the same, so there is **zero impermanent loss!**
> Source: [CLever Medium Blog](https://medium.com/@0xC_Lever/clever-cvx-yields-without-locking-c0fc0638a00c) 

- Integrating multiple Concentrator vaults into CLever can result in additional risk exposure, compounding risk from the participating pool tokens and/or the yield-generating sources (protocols other than Concentrator).
- The platform fees, debt ceiling, collateral ratio, repayment fees, and vlCVX delegation can be changed based on the team's discretion (6-of-9 multisig). 


# Risk Team Recommendation

The clevCVX Curve pool is a V1 pool with A=100. The clevUSD Curve pool is a V1 pool with A=200. These are both pool configurations for assets that are easily redeemable and are expected to maintain a strong reversion to the 1:1 peg. clevCVX pool is currently (Dec.18, 2022) unbalanced 9:91 with clevCVX trading at .87 CVX. clevCVX has a much higher early repayment fee compared to clevUSD (5% vs. .5%), impeding arbitrage of the peg. 

Therefore we recommend reducing clevCVX pool to A=50 and maintaining clevUSD pool at A=200. We have discussed the reduction of A for the clevCVX-CVX pool with the CLever team, and we have agreed that a reduction from 100 to 50 is sensible. We may recommend reducing A further if price doesn't stabilize going forward.

The following graph from the Curve Research team shows the price (y-axis) as a function of pool balance (x-axis) for a V1 pool with different A values. The black horizontal line shows the current price of clevCVX/CVX:
![IMAGE 2022-12-19 20:29:30](https://user-images.githubusercontent.com/51072084/208583931-4f8c2ef6-01d2-42b3-955c-aa7f9b780378.jpg)
