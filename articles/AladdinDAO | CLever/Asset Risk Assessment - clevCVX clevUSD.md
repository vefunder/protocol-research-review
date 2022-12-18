
# Asset Risk Assessment - clevCVX & clevUSD 

### CLever is a type of leverage farming solution that allows users to create leverage positions by borrowing a non-liquidating debt in clevTKN, which is a future equivalent of a yield-bearing asset.

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
[Gauge Proposal](https://gov.curve.fi/t/proposal-to-add-clevusd-fraxbp-clevcvx-cvx-clev-eth-pools-to-the-gauge-controller/4632) for clevCVX and clevUSD pools on Curve.
[CLever Docs](https://docs.aladdin.club/clever)
[CLever Medium](https://medium.com/@0xC_Lever) Blog
[Audit Reports](https://github.com/AladdinDAO/aladdin-v3-contracts/tree/main/audit-reports) - Compilation on Github 

[CLever](https://clever.aladdin.club/#/clever) enables leverage farming through the use of a yield-bearing asset (TKN) or its derivative. By borrowing the future yield in the form of clevTKN, users can take advantage of this feature. ClevTKN can be exchanged for TKN using the [Furnace](https://clever.aladdin.club/#/furnace) or a liquidity pool ([Curve](https://curve.fi/#/ethereum/pools/factory-v2-209/deposit) & [Balaner](https://app.balancer.fi/#/ethereum/pool/0x69671c808c8f1c1490a4c9e0145884dfb5631378000200000000000000000392)). . The system considers clevTKN to be equivalent to the collateral (TKN), so it carries no risk of liquidation and is not affected by price oracles. To leverage their farming, a user can simply deposit TKNs in a strategy vault, borrow the future yield in the form of clevTKN, swap it for TKN, and repeat the process.

Borrowers are required to keep at least twice the amount of their claimed future yields locked in the system until they are earned. If borrowers choose to repay their claimed future yields before they are earned, a **5% repayment fee** will be applied. To access their funds, borrowers can simply make an unlock request. The length of time it takes for the request to be processed will depend on the asset being used.

Clever charges a fee on the yield harvest instead of charging interest on the debt. The majority of this fee goes to veCLEV holders (veCLEV is a locked CLEV similar to a vote escrow model pioneered by Curve Finance).

CLEV is a native token for CLever protocol with a capped supply of 2 million. The token distribution for CLEV is as follows:

![](https://miro.medium.com/max/875/1*U9C04jrl3PYTJNwT6cvgQw.png)

Source: Medium article by CLever - [A CLever Token Offering](https://medium.com/@0xC_Lever/a-clever-token-offering-2d775943e23c)

## Furnace (Facilitating extended swap from clevTKN to TKN)

The [Furnace](https://clever.aladdin.club/#/furnace)  can be used to exchange clevTKN for TKN by simply depositing clevTKN in it. With each yield harvest, a portion of clevTKN is converted into additional TKNs, a fee is charged on this yield harvest, and the TKNs are gradually (over 2 weeks) streamed into the furnace to facilitate the exchange from clevTKN to TKN and to distribute yields to TKN depositors in the form of clevTKN. When TKNs are streamed into the furnace, an equal amount of clevTKN is minted. The newly minted clevTKN is given to TKN depositors as yield, and the new TKNs (generated from a yield-bearing asset) are used to facilitate the exchange from clevTKN to TKN.

If the amount of deposited clevTKN is greater than the TKN generated from the yield, it will take additional time to exchange the tokens. However, it is likely that this scenario will occur in practice, due to the utility of clevTKN.**As a result, clevTKN will always trade at a discount unless there are no clevTKN left in the furnace.** 

If a user wishes to exchange their clevTKN (yields or borrowings), they have two options. They can use a liquidity pool, but this will result in a loss due to the discount (since clevTKN is valued lower than TKN). Alternatively, the user can use the furnace to swap their clevTKN, but this will require them to pay an equivalent amount in time. A "loss of time" or "loss of value" is quoted above because the annual percentage rate (APR) numbers on the CLever website are calculated under the assumption that clevTKN is equal to TKN, which is difficult to achieve in practice.

**Incentives in the form of CLEV (48.25% allocation) can be used to limit the deposit of clevTKN into the furnace and improve liquidity. Implementing a gauge controller to distribute CRV rewards on the clevTKN-TKN liquidity pool on Curve can also help with liquidity and limit the deposit of clevTKN in the furnace.**

**As the number of clevTKN deposited in the furnace approaches 0; the value of 1 clevTKN in the market will approach the value of 1 TKN.**

CLever currently has two strategic vaults:
1. clevCVX
2. clevUSD

# clevCVX

The clevCVX strategy has the following key statistics:

- The locked version of CVX (vlCVX) is used as the yield-generating asset.
- The yield is derived from bribes (Votium delegation) and vlCVX rewards (a share of the Convex platform fee).
- Users can borrow up to 50% of the future yield.
- Withdrawals may take up to 17 weeks if not timed correctly.
- The current harvest fee is 2% and will soon be increased to 20%.

Here is a comparison of the four different strategies with CLever:

- CVX Locking without borrowing: This strategy involves locking CVX without borrowing any additional funds.
- Maximum borrowing without depositing in the furnace (farming elsewhere): This strategy involves borrowing the maximum amount possible, but not depositing the borrowed funds into the furnace. Instead, the borrowed funds are used for farming elsewhere.
- Maximum borrowing and depositing in the furnace (leverage farming): This strategy involves borrowing the maximum amount possible and depositing the borrowed funds into the furnace, also known as leverage farming.
- Votium Delegation: This strategy involves delegating vlCVX to Votium to generate bribe yield.
 
**![](https://lh4.googleusercontent.com/oH9GcxQRyTs-KWWvucB_m8E7UvfR_MQlcZinyhKb5112vBfGfDnzBM1klfa6ZAwfkeoPa7nn7uE5fzkl8NYctkQks_6W11QAW7hcaBe0Mz1NdcgC3-BADNpNV8hxKweJmzEAKoc5BA1pcON5ol8fV_L394v0jzvmr5ujBJk8rHEg0jO8kZ40TsfgxmGjnA)** 

Source: [Google sheet](https://docs.google.com/spreadsheets/d/1eeGXJ_GCIPc9YVCqt61F8TISN4K59DdRPD0xcrPiKsg/edit?usp=sharing) by @diligentdeer

Each of these strategies has its own pros and cons, and it is up to the individual to determine which strategy is the most suitable based on their specific goals and circumstances.

It can be concluded that, **with a 20% harvest fee, it is not advisable to use CLever without borrowing.**

## The clevCVX-CVX peg

The only utility of clevCVX is to provide liquidity on Curve or Balancer, aside from being able to be swapped for CVX at a discount. As a result, there is a high likelihood that users will deposit clevTKN in liquidity pools to farm the liquidity mining rewards.

There are a few factors that can affect the value(demand) of clevCVX in the market. One of these factors is the amount of time it takes for the Furnace to swap clevCVX for CVX. If it takes a long time to complete this swap, the market may value clevCVX lower than CVX because of the time value of money. Another factor that can affect the value of clevCVX is the amount of clevCVX that is provided as liquidity in a pool. If a large proportion of clevCVX is used as liquidity, the price of clevCVX in the liquidity pool may decrease. Eventually, there will be a balance between these two factors. The key variables defining this balancce are the incentives to store clevCVX in either of the venue (furnace or the liqudity pool). However, if this balance is disrupted, users may suffer losses in the form of impermanent loss and price impact on DEXs, or as an opportunity cost (investing for a longer period in order to receive a small amount of rewards).

Another factor that can contribute to volatility is the yield obtained from bribes. While it is amazing for bribe yields to continually grow, low bribe yields may cause users to unknowingly take on greater maturity risk with their self-repaying debts. Eventually, users will shift their clevCVX to the furnace in order to exit the system with CVX instead of clevCVX, disrupting the previously discussed balance.

# clevUSD

The clevUSD strategy has the following key statistics:

 - Staked LP tokens of [FRAXUSDC](https://curve.fi/#/ethereum/pools/fraxusdc/deposit), [LUSDFRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-137/deposit), and [TUSDFRAXBP](https://curve.fi/#/ethereum/pools/factory-v2-144/deposit) on Concentrator act as a yield-bearing token.
-   The yield is derived from the compounded yield on Concentrator from Curve and Convex.
- Users can borrow up to 30% of the future yield.
- Withdrawals can be instant.
- ~~The current harvest fee is 2% and will soon be increased to 20%.~~

CLever deposits the user's collateral to the selected Concentrator strategy vault, which automates the compounding of yields. CLever periodically claims the accrued yield from Concentrator, converts it to FRAX, and uses it to repay the loan. If the user has not taken out a loan, they can claim their yields as clevUSD without incurring debt.

With every addition of a Concentrator vault under clevUSD strategy users are stacks more risk exposure coming from the participating pool assets and the yield generating sources.

## Concentrator Vaults

**Useful Links**
[Concentrator Docs](https://docs.aladdin.club/concentrator)
[Concentrator Medium](https://medium.com/@0xconcentrator_29486) Blog

[Concentrator](https://concentrator.aladdin.club/#/vault)  is a system that allows users to deposit Curve LP tokens into one or more listed Convex vaults. These vaults regularly harvest all associated rewards, convert them to cvxCRV, and move them into a concentrator vault. In the concentrator vault, the cvxCRV is staked on Convex and compounded into more cvxCRV.

There are several variables that can impact the yield from concentrator vaults (FRAXUSDC, LUSDFRAXBP, and TUSDFRAXBP):

- The trading volume on the liquidity pool specific to these vaults.
- The gauge weight on the liquidity pool specific to these vaults.
- The overall trading volume on Curve Finance.
- The price of different yield tokens.

The yields from Concentrator are subjected to fees and the fees for these concentrator vaults can be found [here](https://docs.aladdin.club/concentrator/acrv-ifo-vaults).

Concentrator has undergone few [audits](https://github.com/AladdinDAO/aladdin-v3-contracts/tree/main/audit-reports), which means that the likelihood of exploitation through smart contract loopholes is low.

## The clevUSD-FRAX peg 

As we discussed in the clevCVX-CVX peg section, the market will eventually reach a balance between the time value for the Furnace to swap clevUSD for FRAX, the discount on clevUSD in liquidity pools, and the incentives to store clevUSD in either furnace or the liquidity pool. Whenever the variables that were previously discussed change, it will impact this balance, ultimately affecting the pricing of clevUSD-FRAX and the users.

Due to several layers of swaps in the assets and varibales to generate yield, it is hard to speculate on the behavior of the market. One thing that can be conveyed for sure is that with CVX voting power from FRAX, possible CRV rewards from Curve, liquidity mining rewards from CLever, and with a lower borrow limit, the discount on clevUSD will be low as compared to FRAX.

# Centralization Vectors

The [CLever treasury](https://etherscan.io/address/0xFC08757c505eA28709dF66E54870fB6dE09f0C5E) is controlled by a 6/9 multisig. More information about the wallet address and other details can be found at [AladinDAO github](https://github.com/AladdinDAO/deployments/blob/main/deployments.mainnet.md#governance-multisigs). Additionally, background information about the parties involved in the multisig can be found below.

**![](https://lh6.googleusercontent.com/d02A8hyq97R6SgLQL7BURUfjwzyUJc_OSEuNpsp64ogOU268xxaACWPQSbMrShzgRxLg7_g7VGRe1aOEBb8z0-EHgpDnmLePbtGFmS-4zt506AJXOzeUC9QVcAf1HD9WpISa4eJucHhUH5CclRtMUFPuGgmBA993LEs7AOJ5xsWXvoTNprpHrjMK0rXptg)**

The CLever smart contracts are designed to be upgradable and allow for the modification of certain parameters such as platform fees, debt ceiling, collateral ratio, and repayment fees. 

Additionally, CLever has the ability to take the vlCVX delegation back from Votium, although this action will require a vote through the decentralized autonomous organization (DAO).

**![](https://lh5.googleusercontent.com/jlKbp814RPv0A5ftLYuL4h6X6FHnWWnom1nz_gFp5_2QHahGvV_6XImWIAW5JUSAFcP5mrgU4rj_Oy6aVM_PqqtfVMaPDHRezwRiDMGAGclFsmFFb1QZm2tSqhj064Oo-w1CJyfvYLbKO4riMLOqMiC8PvrO3XhzrxDK3U5WkbqjO67E_ggqgIfFZKeUyQ)**

~~With the token distribution, CLEV holding is centralized in the initial phase where DAO votes can be manipulated through veCLEV holdings from Aladdin DAO & Treasury Reserve. Following graph shows Aladdin DAO & Treasury Reserve under **Centralized CLEV**, Partnership offerings, and rest all allocation under **Community CLEV**.~~ 


![](https://lh4.googleusercontent.com/h0lJpKdaSiXISEhEriu-736CfVQJFGsUndfnkq4Tb85Q4X6OwlywLROfNlDfukiM8bKmquhWeB0XXUpgddx0wxWbAIrtKezQnGw-xzBmNXXy6YDNITFESp2CtK4G3UC-P_AIXiai33GhCiHImazlgtyKv_x0IcrfCYLogebRuMJs2WkJspid_1XDlJox1w)

Source: [Google Sheet](https://docs.google.com/spreadsheets/d/1JtXtL26nCaC5js5WFYCZrzBMXIjhtBnY8s9VeKnsXUs/edit?usp=sharing) by @diligentdeer

# Conclusion

- clevTKN circulating supply will eventually reach the amount of CR (collateral ratio) x TKN locked in Clever vault.
- There is a risk of maturity (time value) associated with clevTKN, which represents TKN in the future and this risk will vary based on the yield variables of the specific strategy or vault.
- The clevTKN can rarely trade at the same value as TKN unless the amount of clevTKN deposited in the furnace is less than the TKN generated from the yield.
- If the Furnace takes a long time to swap clevCVX, the market will value clevCVX lower due to the time value of money. If a large proportion of clevCVX is used as liquidity, the price of clevCVX in the liquidity pool will decrease. A balance between these factors will eventually be achieved, but if it is disrupted, users may suffer losses in the form of impermanent loss, price impact on DEXs, or opportunity cost.
- It is unlikely that impermanent loss will be zero as CLever has no control over the generated yields (one of the varible that can distort the balance stated in the above point), despite what is stated on their blog.
> ...For the liquidity farmer, this is an important distinction, because it means that, on a long enough time scale, both your CVX and your clevCVX are the same, so there is **zero impermanent loss!**
> Source: [CLever Medium Blog](https://medium.com/@0xC_Lever/clever-cvx-yields-without-locking-c0fc0638a00c) 

- Integrating multiple Concentrator's vaults into CLever can result in a stacked risk exposure streaming from the participating pool tokens and or the yield generating sources (protocols other than Concentrator).
- The platform fees, debt ceiling, collateral ratio, and repayment fees can be changed based on the team's discretion. 
- In addition to the above, the vlCVX delegation (presently Votium) can also be revoked by the team through a DAO vote.
- ~~veCLEV holds voting powers and there exists a phase where this voting power can be misused due to it being concentrated in the hands of AladdingDAO and the CLever team.~~ 
