# Twitter thread

bLUSD-LUSD LP is providing a tempting yield and it may increase after the $CRV gauge approval.

But what are the risks?

@diligentdeer has covered risks associated with bLUSD-LUSD liquidity. 

A summarising  🧵   

📜Article:

0/13

---
TL;DR

• Impermanent Loss

    • Risks associated with the bLUSD price
    
    • Risks associated with the LUSD peg
    
• Risks associated with yield sources (B.Protocol/Yearn)

• Opportunity cost

• Smart contract risk

1/13

---
Liquidity in bLUSD-LUSD3CRV is crucial for Chicken bonds. The bLUSD market price is derived from this pool, which ultimately impacts the user's decision either to hold the bond, chicken in, or chicken out.

More about bLUSD: https://www.chickenbonds.org/blog-posts/what-makes-the-btokens-valuable

2/13

---

A 3% fee collected from the bonders at the time of chicken in is used to incentivize the LPs.

Initially, when there is low liquidity on the bLUSD-LUSD3CRV pool, bLUSD sell pressure can impact the price of bLUSD in a negative way, incurring an impermanent loss.

3/13

---

Chicken bonds invest the LUSD to generate yield. This yield is diverted into increasing the floor price of bLUSD.

When a user bonds their LUSD, they accrue virtual bLUSD and have a chance to get accrued bLUSD or exit with initially bonded LUSD.

Here is a simple schematic:
![](https://lh6.googleusercontent.com/ZIZUjpj4yUH2lTPfJgC7jmBVJkkegK4eEksZf_2knaZDFrQNeLCUCS1WAoDeaI436GjWWbrHfILcqGECNYI0ABXLP6f_a4LmG_9xcWareS_WHyWxtC7O_CnUC8DgQ7wWOjl9Zd-FgoR_Lt3VdZ1ld-6oIJcC0fYFBORiaAxgv5Zqee43PycL3LzjGoXIEA)

4/13

---
One of the variables for bLUSD accrual is the accrual parameter (α). 
Lowering the α (alpha) would cause the bLUSD accrual to speed up.

The mechanism tracks the average bond age and when it goes beyond 15 days the α (alpha) is lowered.

![](https://lh4.googleusercontent.com/k3o7OuY1SC95kNCOCsNumC7Lx1gjbdQkyLl4HCL4-oizhUWdB9gK93Hj60Hd0ha8w_7DTDtEdPsZn3WMFSDNIDEX1_kTVOLRmtMwu2P2pOkzYM15ymjMfkJcKKtbvuQLboZMUjxHGmz-FPBTt2MmJJV-6TjarZAhM_kGG16EcnHbRhN_AIHHey4C_2DMfg)
![](https://typefully-user-uploads.s3.amazonaws.com/img/original/46439/d01dfb79-0c49-4330-8f71-01dc52ad9db1.png?response-cache-control=no-cache&AWSAccessKeyId=AKIAVTJZGL3H4SWWVYIT&Signature=ZDT7P%2BBeWZ2a3Xv3Zq5UiBV2SSs%3D&Expires=1668487301)

5/13

---
Speeding the bLUSD accrual will change the non-profitable chicken in trade to a profitable one (Check the attached graph).

When they chicken in, the extra profit (which otherwise would not be generated if α was not changed) will come from the bLUSD market price.
![](https://typefully-user-uploads.s3.amazonaws.com/img/original/46439/150a8e3c-146e-4d39-9403-f50beec015fb.PNG?response-cache-control=no-cache&AWSAccessKeyId=AKIAVTJZGL3H4SWWVYIT&Signature=LpnpMSDBVuSyTylyXyYBAgTIyaU%3D&Expires=1668488233)

6/13

---
When the market price trades near the floor price, trading activity between bLUSD & LUSD will slow down resulting in an opportunity cost as well as IL for the LPs.

7/13

---
Some more factors that can impact the bLUSD market price negatively if not true are:

- Bonding volume is greater than the chicken in volume.
- Users chicken out after bonding for a considerable time.
- Users chicken in early.


8/13

---
When people are not confident in holding ETH in a flash crash, there exists a chance that LUSD might get depegged.

Though LUSD has historically traded above its peg, the risk can’t be ignored.


9/13

---
The LUSD ChickenBonds Protocol makes use of B.Protocol to invest funds in the stability pool for generating yield. 

In an adverse situation, the collateral might get liquidated at a loss, which is borne by the Reserve and therefore bLUSD holders.

10/13

---
The B.Protocol's integration with Liquity was audited in July 2021 and does offer an Immunefi bounty of $100k. https://immunefi.com/bounty/bprotocol/

Moreover, the integration is live on the main net since Aug 2021.

11/13

---
Chicken bond uses Yearn's LUSD vault as a yield source.

Stacking protocols for yield generation (Yearn's LUSD vault) can increase the risk surface. 

The vault contract has been live for more than 5 months and Yearn protocol has been time-tested since Feb 2021.

12/13


---
![image](https://user-images.githubusercontent.com/51072084/202838490-63b108c6-d8be-4c53-8ade-57ed639e103c.png)

13/13

---
If you found this thread useful, make sure to share this thread and follow
@diligentdeer, the analyst who made this report possible. 

For a complete overview of the bLUSD risk assessment, read the full article here:
