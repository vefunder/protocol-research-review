# Twitter thread - clevCVX & clevUSD

75%+ yield on clevCVX-CVX, 
Borrowing w/o Liquidations,
Leveraging Curve wars and the power of FRAX.

But Risks?

@diligentdeer has covered risks associated with clevCVX & clevUSD.
A summarising 🧵

📜Article: ~~LINK~~
---

CLever enables leverage farming through the use of a yield-bearing asset (TKN) or its derivative by allowing users to borrow its future yield in the form of clevTKN. 

ClevTKN can be redeemed for TKN using the Furnace or exchanged in a liquidity pool (Curve & Balancer).

1
---

The Furnace can be used to exchange clevTKN for TKN by simply depositing clevTKN in it.

For the same yield, if the clevTKN deposit increases in the furnace the time to execute the swap from clevTKN to TKN will increase.

![image](https://user-images.githubusercontent.com/117331039/209197291-6a82544f-b154-499f-aa8d-953285d8f3c1.png)

2
---

Another utility for clevTKN is to provide liquidity on the respective liquidity pool. CLever does have a liquidity mining service where a user can stake their LP tokens to get CLEV rewards.

![image](https://user-images.githubusercontent.com/117331039/209196495-3cafbe42-86c9-4383-90b4-1f1e5fb1267c.png)

3
---

The incentives to provide liquidity to the clevTKN-TKN pool will start high but eventually be diluted due to users depositing their clevTKNs. 

The LP rewards will be diluted till the rewards to deposit clevTKN in the furnace is equivalent to that of the LP rewards.

4
---

When this equilibrium is disrupted, users may experience impermanent loss (pool imbalance) and price impact (lower liquidity) on DEXs, or as an opportunity cost (investing for a longer period to receive a small number of rewards).

5
---

Another factor that can contribute to this equilibrium is the yield obtained from bribes. The possibility of decreasing bribe yields may cause users to unknowingly take on greater maturity risk with their self-repaying debts.

6
---

Using CLever w/o borrowing clevTKN is a loss-making venture. Check the different strategies possible with clevCVX compared to directly vote-lock CVX and earning yields on it: 

![](https://typefully-user-uploads.s3.amazonaws.com/img/original/46439/c5db867d-b02a-4ee6-87c8-76e88064fd10.png?response-cache-control=no-cache&AWSAccessKeyId=AKIAVTJZGL3H4SWWVYIT&Signature=qTkSuKRlJkB6HsGWmQZGltNY9YI%3D&Expires=1671805533)

7
---

Considering the harvest fee charged by CLever, it is not beneficial to use CLever without borrowing clevTKN. With the above fact, clevTKN circulating supply will eventually reach the amount of CR (collateral ratio) x TKN locked in the CLever vault.

![](https://typefully-user-uploads.s3.amazonaws.com/img/original/46439/95ea37fa-0ce5-4bb4-9816-cfe2b1591735.png?response-cache-control=no-cache&AWSAccessKeyId=AKIAVTJZGL3H4SWWVYIT&Signature=disaR6Nu9BEpr8%2BerJtup%2Bp4GH4%3D&Expires=1671805533)

8
---

Users are exposed to maturity risk when holding clevTKN. 

This risk can be realized through time or value because the clevTKN can rarely trade at the same value as TKN unless the amount of clevTKN deposited in the furnace is less than the TKN generated from the yield.

9
---

CLever deposits the user's collateral to the selected auto-compounding Concentrator vaults and periodically claims the accrued yield from Concentrator, converts it to FRAX and distributes an equivalent amount of newly minted clevUSD proportionally to all depositors.

10
---

Integrating multiple Concentrator vaults into CLever can result in additional risk exposure, compounding risk from the participating pool tokens, and/or yield-generating sources (protocols other than Concentrator).

11
---

CLever's strategy vaults have undergone thorough auditing, and the Concentrator protocol utilized in clevUSD vaults has also undergone auditing. These ensure reasonable precautionary efforts against smart contract exploitation.

All the relevant audits can be found here: https://github.com/AladdinDAO/aladdin-v3-contracts/tree/main/audit-reports

12
---

clevCVX has a much higher early repayment fee of 5%, impeding arbitrage of the peg.

We recommend reducing A for this pool to 50. 

CLever team agrees that reducing from 100 to 50 is sensible. Moreover it is recommended to lower A if the price does not stabilize

![](https://typefully-user-uploads.s3.amazonaws.com/img/original/46439/4fea14ad-5939-4d8d-abdc-666e43820449.png?response-cache-control=no-cache&AWSAccessKeyId=AKIAVTJZGL3H4SWWVYIT&Signature=%2BFV4Jgp0sRT66s8K4v294x3WXMg%3D&Expires=1671805533)

13
---

clevUSD market price depends on the willingness of users to repay debt.

A=200 is justifiable until CLever team keeps the early repayment fee to 0% (for FraxBP LP) to facilitate debt repayment and support peg.  

A can be changed based on the market mechanics

14
---

---

# An experiment with chatGPT

75%+ yield on clevCVX-CVX, 
Borrowing w/o Liquidations,
Leveraging Curve wars and the power of FRAX.

But Risks?

@diligentdeer has covered risks associated with clevCVX & clevUSD.
A summarising 🧵

📜Article: ~~LINK~~

---

CLever allows users to leverage the farm using a yield-bearing asset (TKN) or its derivative, by allowing users to borrow the future yield in clevTKN.

ClevTKN can be redeemed for TKN using the Furnace or can be supplied as liquidity.

1
---

The Furnace can be used to exchange clevTKN for TKN, but the time it takes to execute the swap will increase as the amount of clevTKN deposited in the furnace increases. 

Here is a rough mechanics on how CLever works:

![image](https://user-images.githubusercontent.com/117331039/209197291-6a82544f-b154-499f-aa8d-953285d8f3c1.png)

2
---

ClevTKN can also be used to provide liquidity to the clevTKN-TKN pool, where users can stake their LP tokens on CLever to earn CLEV rewards. 

However, these incentives will eventually be diluted as more clevTKN is deposited in the liquidity pool.

![image](https://user-images.githubusercontent.com/117331039/209197388-1394e4bd-bc8f-4f8c-9642-689197d421e7.png)

3
---

It's worth noting that users providing liquidity may experience impermanent loss and/or price impact on DEXs if the equilibrium between the returns gained from the Furnace swap and LP rewards liquidity pool is disrupted.

4
---

This equilibrium can also be affected by the yields from the yield-bearing assets, which may result in users taking on greater maturity risk along with the risks mentioned in the previous tweet with their self-repaying debts.

5
---

clevTKN may not trade at the same value as TKN unless the amount of clevTKN deposited in the Furnace is less than the TKN generated from yield.

Meaning that the price of clevTKN is affected by the amount of yield being generated.

6
---

Using CLever w/o borrowing clevTKN is loss-making. 

Check the different strategies possible with clevCVX compared to directly vote-lock CVX and earning yields on it:

1[](https://typefully-user-uploads.s3.amazonaws.com/img/original/46439/2508c0d8-3447-4e17-94c8-57286de88c1a.png?response-cache-control=no-cache&AWSAccessKeyId=AKIAVTJZGL3H4SWWVYIT&Signature=GatovNKA5H3YIIUU1yurNZJLcws%3D&Expires=1671810363)

7
---

CLever deposits users' collateral in selected auto-compounding Concentrator vaults and periodically claims the accrued yield.

These are converted to FRAX, and distribute an equivalent amount of newly minted clevUSD to all depositors as yields. 

8
---

Integrating multiple Concentrator vaults into CLever can result in additional risk exposure, compounding risk from the participating pool tokens, and/or yield-generating sources (protocols other than Concentrator).

9
---

CLever's strategy vaults and the Concentrator protocol used in clevUSD vaults have been thoroughly audited to ensure reasonable precautions against smart contract exploitation.

All the relevant audits can be found here: https://github.com/AladdinDAO/aladdin-v3-contracts/tree/main/audit-reports

10
---

clevCVX has a much higher early repayment fee of 5%, impeding arbitrage of the peg.

We recommend reducing A for this pool to 50. 

CLever team agrees that reducing from 100 to 50 is sensible. Moreover, it is recommended to lower A if the price does not stabilize.

![](https://typefully-user-uploads.s3.amazonaws.com/img/original/46439/1cef79ca-e4fe-4922-a53d-f43702f48c50.png?response-cache-control=no-cache&AWSAccessKeyId=AKIAVTJZGL3H4SWWVYIT&Signature=Lu%2B7ZKhTA0WSr20ufvJln%2BY0kew%3D&Expires=1671810492)

11
---

clevUSD market price depends on the willingness of users to repay debt.

A=200 is justifiable until CLever team keeps the early repayment fee to 0% (for FraxBP LP) to facilitate debt repayment and support peg.  

A can be changed based on the market mechanics.

12
---
