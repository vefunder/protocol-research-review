# Asset Risk Assessment - USV (USDC Saving Vault by Phuture)

### USV is the Phuture Savings Vault's first product which is powered by Notional Finance, a fixed rate lending and borrowing platform.

## Index

- General overview on Phuture
  - Noteworthy Findings   
- Notional Finance
  - fCash
  - Liquidity Pool & Interactions
    - Liquidity providers
    - nTokens
  - Notional AMM and Oracle
    - Liquidity Pool - Governance parameters
    - Interest Rate Oracle
    - Borrowing
      - Collateralization
      - Liquidation
    - Lending
- Demand - Supply Balance
- Conclusion
- Appendix
  - Trade calculation

## Relation to Curve  
In January 2023, Phuture made a [proposal](https://gov.curve.fi/t/proposal-to-add-usv-fraxbp-to-the-gauge-controller/8687) on the Curve governance forum to add the [USV/FraxBP](https://curve.fi/#/ethereum/pools/factory-crypto-202/deposit) pool to the Gauge Controller. The on-chain vote has passed and can be seen [here](https://dao.curve.fi/vote/ownership/274). This report seeks to identify relevant risks to Curve’s liquidity providers. 

## Phuture - USDC Saving Vault (USV) Overview

**Useful Links**

- [Website](https://www.phuture.finance/)
- [Docs](https://docs.phuture.finance/introduction/master)
- [Blog](https://www.phuture.finance/blog)
- [Github](https://github.com/Phuture-finance)
- [Audits](https://docs.phuture.finance/protocol/audits)
- [USV Contract Addresses](https://docs.phuture.finance/protocol/contract-addresses#phuture-savings-vaults)
- [USV Vault Contract](https://www.contractreader.io/contract/0x4030de8f22f17822e325287ce3c8d97c36982b67)
- [2-of-3 multi-sig](https://app.safe.global/eth:0x6575A93aBdFf85e5A6b97c2DB2b83bCEbc3574eC/home)

Phuture is a crypto index platform that offers its users exposure to baskets of DeFi tokens through its on-chain index funds and savings vaults. Its newest product, the USDC Savings Vault (USV), seeks to simplify access to fixed-rate stablecoin yields. Users simply deposit USDC into the vault and receive the yield-earning USV receipt token. By automating the process of managing maturities and optimizing yield through its lending strategy, Phuture aims to reliably generate yields on USDC.

Phuture utilizes Notional Finance, a fixed yield DeFi lending platform, to power their USDC savings vault. The USV strategy optimizes yield by dynamically investing into a blend of Notional's 3 and 6-month bonds. Notional, in turn, lends all deposited funds to Compound for increased capital efficiency. Given that the USV product is primarily exposed to Notional Finance, a significant portion of this report will focus on the inner workings of this platform.

When USDC is deposited into Phuture, it is lent out to the highest yielding tenor (either 3 or 6 month tenor). These deposits may remain idle until a minimum threshold of USDC is reached (although currently the threshold is 0, with harvesting done every 3 days). During the redemption process, USV follows a waterfall structure that prioritizes paying out any available USDC before selling positions. To safeguard future returns, USV sells positions in order of the lowest yielding tenor first.

The following flowchart shows the strategy employed by USV:

![USV_strategy_20230221_2](https://user-images.githubusercontent.com/51072084/220424330-cbab09a6-ae4e-46b7-8dc6-18cacf251f52.png)

## USV and Phuture Product Adoption

After conducting an analysis of the USV-FraxBP pool, our findings show that as of 2/18/23, the pool has not generated any trade volume since its inception. This has been confirmed by this [query](https://dune.com/queries/1982268), which provides further analytics. This is not unheard of for new pools preparing to bootstrap with a new gauge, but surely a trend to monitor.

Currently, 100% of the liquidity in the pool is supplied by the Phuture team. Specifically, 99% is provided by a multisig owned by Phuture ([0x237a4d...](https://etherscan.io/address/0x237a4d2166Eb65cB3f9fabBe55ef2eb5ed56bdb9)), while the remaining 1% is provided by one of the multisig owners ([0x9fD6Ac...](https://etherscan.io/address/0x9fD6Ac607AE0B13e066a609f6e5f2d41c3d04A5F)).

When accounting for the 271,208 USV held by their multisig and the 100,000 USV provided as liquidity by their multisig, only 7.5% out of the 401,309 total USV supply is held by wallets other than the Phuture multisig. Phuture has stated that they will deploy the remaining USV in their treasury to the liquidity pool once the CRV gauge is allocated.

Phuture has another product called PDI (Phuture DeFi Index), a DeFi index token built on Ethereum. The performance of PDI can provide insight into the community's willingness to adopt new products from Phuture. Upon analyzing the distribution of PDI tokens, we observed that the majority of the PDI supply, approximately 79%, is held in the team multisig treasury. As of February 18th, the total PDI supply stands at 1389.5 tokens, of which 139.37 PDI is held in a wallet, 800 PDI is supplied as liquidity on Uniswap V3, and 111.99 PDI is parked in bPDI by the treasury multisig.

Here is how the supply change over time correlated with the no. of holders overtime:

![image](https://user-images.githubusercontent.com/117331039/219812142-a9057f87-c24f-49f8-b171-c3ca387bf12d.png)

Source: [PDI Supply and Holders Data [v2-5]](https://dune.com/queries/2014104/3333813)

## Notional Finance

**Useful Links**

- [Website](https://www.notional.finance/) 
- [Whitepapers](https://docs.notional.finance/developers/whitepaper/whitepaper)
- [Docs](https://docs.notional.finance/notional-v2/)
- [Developer Docs](https://docs.notional.finance/developer-documentation/)
- [Blog](https://blog.notional.finance/)
- [Analytics](https://info.notional.finance/)
- [Desmos Graph by Diligent Deer](https://www.desmos.com/calculator/dunk4utpir)
- Audits: 
  * Code4Arena [Audit 1](https://code4rena.com/reports/2022-01-notional/), [Audit 2 Staked Note](https://code4rena.com/reports/2021-08-notional/)
  * Consensys Dilligence [Notional V2.1](https://consensys.net/diligence/audits/2022/03/notional-protocol-v2.1/)
  * Certora [Formal Verification](https://github.com/notional-finance/contracts-v2/blob/master/audits/Certora%20-%20Formal%20Verfication%20Report%2C%20Nov%201%202021.pdf)
  * OpenZeppelin [Governance Contracts](https://blog.openzeppelin.com/notional-v2-audit-governance-contracts/)
  * ABDK [Audit 1](https://github.com/notional-finance/contracts-v2/blob/master/audits/ABDK%20-%20Notional%20V2%2C%20Sept%201%202021.pdf), [Audit 2](https://github.com/notional-finance/contracts-v2/blob/master/audits/ABDK%20-%20Notional%20V2%20Fixes%2C%20Nov%201%202021.pdf)

Notional is a fixed-rate, fixed-term crypto asset lending and borrowing platform that enables the tokenization of future payments in various currencies, such as ETH or USDC. It is able to offer fixed-rate thanks to its complex AMM that divides liquidity into fixed-term pools called tenors. This is made possible by fCash. The fCash mechanism allows Notional users to commit to transfers of value at specific maturity dates in the future.  

### fCash

fCash is minted in pairs of assets and liabilities, which are settled on a predefined date. It represents a fixed amount of cash that a user is entitled to receive (lending) or obligated to pay (borrowing) at a specific point in time, known as maturity. When users lend or borrow at fixed rates, they deposit or receive cash in exchange for fCash.  

+fCash is a claim on the asset at maturity, typically held by a lender, which indicates that a user will receive the underlying assets back upon maturity. In contrast, -fCash represents an obligation to repay the capital at maturity, typically held by a borrower, which should eventually be paid back.


### Notional Market Participants

The three market participants in the system are lenders, borrowers, and liquidity providers. Phuture interacts with the protocol as a lender, depositing USDC in exchange for the promise of a fixed rate yield.


#### Lender

Lender deposits are immediately deposited into Compound and the cToken receipt is held by Notional. The lender receives +fCash, which represents a claim on their capital plus the interest at maturity. Lenders have the flexibility to choose from various active tenors, each with different maturity dates and interest rates. At maturity, the fixed interest rate on the lender's fCash will cease, and it will continue earning the lower variable rate from their underlying Compound cToken position.

It is possible to redeem the underlying cToken before maturity, although doing so requires selling fCash into Notional's interest rate AMM. This will potentially expose the lender to risk of loss, as Teddy from Notional explains:

![teddy](https://user-images.githubusercontent.com/51072084/221062327-0b1e3793-bf68-437d-9234-ae54ed0016d4.png)


[This desmos graph](https://www.desmos.com/calculator/dunk4utpir) helps to show the magnitude of potential gains/losses from early redemption (Note that stablecoin rates on Notional have a max range of 0%-13%, and a normal range of 2%-7% as specified by the AMM parameters). USV holders may be affected, as large USV withdrawals will usually require selling fCash positions before maturity.

#### Borrower

In order to borrow funds, a borrower must first mint both positive and negative fCash tokens based on their chosen collateral and maturity. These tokens will represent their obligation to repay the borrowed funds at a later date.

Once the fCash tokens have been minted, the borrower can then exchange the positive fCash for cTokens, which will allow them to access the funds they need. At the end of the trade, the borrower will receive cTokens and negative fCash, which represents their obligation to repay the capital.

Notional strives to ensure that lenders have a high likelihood of earning the expected returns by requiring borrowers to pay back their debt at maturity. The required amount of collateral that borrowers must hold against their debt is determined based on the risk level of their collateral. For example, DAI requires a lower collateralization ratio of 120% compared to ETH's 150%.

If a borrower is unable to repay the funds they have borrowed by the maturity date, they will be subject to a penalty rate of 2.5%, or 250 basis points, and may be forcibly auto-rolled by a third party. It is possible for the borrower to roll their debt to a future maturity at the prevailing interest rate for that maturity at any time.


#### Liquidity Provider

Notional maintains on-chain liquidity pools that serve as a ready counter-party for borrowers and lenders at all time. The exchange rate between USDC and fUSDC at the specific maturity represents the fixed interest rate that users receive on Notional. In other words, the interest rate for lending and borrowing on Notional V2 is defined by the fCash Markets.

Liquidity providers are essential to the functioning of liquidity pools by supplying both cTokens and fCash. The LP mints a pair of fCash tokens (these cancel out at maturity), and deposits cToken and +fCash into the pool. In exchange for their contribution, these providers are entitled to earn fees every time a borrower or lender trades between cTokens and fCash within the pool. 

Liquidity providers are important for stabilizing interest rates and they bear the risk of impermanent loss depending on the demand for lending and borrowing. Every borrow/lend action will incur some slippage in the interest rate, with the magnitude determined by the size of the loan compared to the overall liquidity in the pool. It is therefore essential that liquidity providers are adequately incentivized to ensure that the fixed-rate borrowing and lending market is stable.

Both liquidity providers and borrowers can mint fCash. Borrowers sell +fCash into the pool in exchange for currency, and holds the obligation (-fCash) to repay at the predefined future date. By contrast, liquidity providers serve as counterparty to both parties by making the underlying asset available to borrowers and +fCash available to lenders.  

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MfKPITPv8YEBxbZh665%2F-MfKPizWILHrStwWO4Uc%2Fuser_types.png?alt=media&token=b2225422-a74b-4672-91f6-7f9efb1113b0)

Source: Notional Finance [Docs](https://docs.notional.finance/notional-v2/notional-v2-basics/liquidity-pools)


#### nToken

Supplying liquidity to a multitude of markets with different maturity dates would be cumbersome for Notional LPs, so instead, there is a passive strategy through nTokens. These tokens automate the process of minting fCash and depositing suitable assets into the liquidity pool with different maturities for the liquidity provider. By holding nTokens, users can earn returns from providing liquidity to Notional across all active maturities without needing to interact with the individual liquidity pools.

To mint nTokens, a user deposits cTokens into the nToken account. The account then distributes the user's liquidity into the underlying pools for that currency and holds the resultant liquidity tokens, which represent the nToken account's claim on the user's deposit. The distribution is based on a set of governance parameters known as "deposit shares," which are percentage figures that determine how much of a user's total liquidity is deposited into each individual market.

![assets2F-MX2K6zXuGl2Zi-qgoUj2F-Me7Mven0A-wvy5XmpvY2F-Me7OujUY9QNbjAnUvTI2FnToken_02](https://user-images.githubusercontent.com/51072084/220424899-fc4a3145-4828-4492-a620-9ece3c07f04a.png)

Source: Notional Finance [Docs](https://docs.notional.finance/notional-v2/ntokens/minting-ntokens)


### Notional AMM

To optimize the trading experience, the AMM curve used by Notional is designed with a balance between flatness and flexibility. A relatively flat curve ensures that normal trading conditions result in reasonable levels of slippage, while also allowing for market repricings with a more flexible curve.

Notional's AMM curve resembles that of Curve's stable swap invariant, which is designed to handle large changes in the equilibrium interest rate while still maintaining a relatively flat curve most of the time. This approach aims to provide a smoother trading experience for users under various market conditions.

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MeyxH0O2F9LIC6MRCp6%2F-Mez12PmZgdb9NrF-CzO%2Fimage.png?alt=media&token=ebcd677d-5828-4ed9-b099-c3b2ce570584)

Source: Notional Finance [Whitepaper](https://docs.notional.finance/developers/whitepaper/whitepaper)


#### Pool Governance Parameters

Liquidity pool governance parameters have a significant influence on the shape of the AMM curve and the liquidity allocation toward each pool. Notional's governance is controlled by a 3-of-5 multisig consisting of two team members and three community members.

- **Deposit shares** represent the proportion of incoming liquidity allocated among active pools. Deposit shares should add up to 1 (100%). For USDC, deposit shares are set at 45% for a 3-month maturity, 40% for a 6-month maturity, and 15% for a 1-year maturity.
  
- **Anchor rate** determines the position of the AMM curves in the X-Y plane. It influences the tradable interest rate range. In the case of USDC, the anchor rate is set at 3.5%.
  

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MeyxH0O2F9LIC6MRCp6%2F-Mez1bvxQl21m0dDHvZj%2Fimage.png?alt=media&token=e3dff1ec-5e6e-4465-8b30-79667fd82bc7)

Source: Notional Finance [Whitepaper](https://docs.notional.finance/developers/whitepaper/whitepaper)

- **Scalar rate** is the parameter that affects the steepness of the AMM curve. A high scalar value corresponds to a flatter curve with lower interest rate sensitivity and lower slippage. Conversely, a low scalar value corresponds to a steeper curve with higher interest rate sensitivity and higher slippage. The scalar rate for USDC is set at 48.

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MeyxH0O2F9LIC6MRCp6%2F-Mez1Qz91Eu3q-RZkPt5%2Fimage.png?alt=media&token=21c201e6-1162-41f0-8ce3-c45225d51dd4)

Source: Notional Finance [Whitepaper](https://docs.notional.finance/developers/whitepaper/whitepaper)


#### Interest Rate Oracle

The most recent trade executed on the pool implies the current interest rate. This rate is vulnerable to manipulation, especially over short time frames, such as through the use of flash loans. Such manipulation can result in a distorted or inaccurate portrayal of the true interest rate of the pool.

To address this issue, a TWAP oracle rate is used to minimize the risk of price manipulation. The oracle rate provides a lagging price feed, which converges on the true interest rate of the pool over a set time window determined by governance. By providing a more stable and manipulation resistant representation of the interest rate, the oracle rate helps to maintain the integrity of the liquidity pool and protect its participants.


**Liquidations:**

Notional uses unprocessed Chainlink price feed to facilitate liquidations. During the liquidation, a liquidator buys a portion of the collateral owned by a borrower account that has fallen below a minimum required level at a discount to the on-chain oracle price. The liquidator then uses this collateral to repay some of the debt owed by the liquidated account. Notably, even if the borrower's collateral is only slightly under the required level, the liquidator can purchase at least 40% of it.

In the unlikely scenario where market volatility delays the liquidation process and causes a potential shortfall, Notional has measures in place to ensure lenders' funds are protected. The protocol can use its own reserve assets or sell its native token, NOTE, to raise the necessary capital to compensate lenders and make sure they are always fully reimbursed.

The NOTE treasury can be tracked from [here](https://info.notional.finance/treasury).


## Risks

### Notional Finance Risks

#### Insolvency

As with any smart contract-based financial platform, Notional Finance carries inherent risks. While the platform has been audited, has an active [Bug Bounty with immunefi](https://immunefi.com/bounty/notional/) and is certified by industry leaders including Certora, ABDK, Code Arena & OpenZeppelin, there exists a possibility of insolvency in the event of borrower defaults. In such a scenario, lenders (ie. USV depositors) may be affected. However, Notional Finance has taken steps to mitigate these risks through a robust liquidation protocol and infrastructure, which aim to minimize potential losses.

In the unlikely event that the liquidation procedure allows a borrower to become insolvent, Notional Finance has an [on-chain reserve fund](https://info.notional.finance/s-note) that can be utilized to help cover any losses that may arise on the platform. This on-chain reserve fund is in the form of sNOTE, which stands for staked NOTE/ETH 80/20 LP tokens. If the platform experiences losses, NOTE token holders can participate in an on-chain vote to take 50% of the assets held in the sNOTE pool and use them to recapitalize the system.

In this scenario, sNOTE holders would bear a loss of up to 50% of their assets. While it's important to understand the risks involved in using any financial platform, Notional Finance has taken steps to ensure that its users are protected to the best of its ability.

The pool holding can be verified here: [Notional Info - sNOTE](https://info.notional.finance/s-note)

#### Operational Risk

It is crucial to balance liquidity provider incentives for smooth pool interactions and healthy liquidity to support borrowing and lending. Various factors affect incentives and demand, including risk adjustments on collateral assets, borrowing and lending rates, minimum collateral ratio, liquidation mechanism, NOTE rewards, and liquidity pool/AMM parameters. Optimizing these parameters safely within the context of the market is essential to sustain yields for the Phuture USDC saving vault.

It is important to consider the impact of changing parameters, such as raising the anchor rate, which may attract more lenders but could also distort the pool and lead to high slippage. This can impact the usability of the protocol, leading to a decrease in lending/borrowing and liquidity provision. Continual changes in the market rate for lending and borrowing necessitate a lower scalar factor to cover a wider spread of interest rate fluctuations within the AMM. High fluctuations in interest rates can result in the protocol's failure to capture demand for borrowing/lending due to high slippage, particularly if the scalar factor is high. Moreover, if the market settles for an interest rate away from the anchor, there would be a higher impermanent loss for LPs at notional if the scalar factor is high.

#### Market Risk

Funds deposited into USV will either be 1) sitting idle as USDC, 2) deployed as 3-month tenor fCash, or 3) deployed as 6-month tenor fCash. fCash redeemed before maturity will be exposed to market risk from changes in interest rate, as modelled [here](https://www.desmos.com/calculator/zwaw8lr0nm). USV uses a waterfall redemption strategy to first prefer USDC, and then the lowest-yielding tenor for withdrawal. All funds will typically be deployed into Notional, as there is no minimum threshold of deposits required by the USV strategy, and the harvesting interval is every 3 days. This means users could be exposed to loss on their principal at time of withdrawal, depending on changes in interest rate. Notably, USV allows users to specify a minimum output amount and does not place any restriction on a user's ability to withdraw, regardless of market conditions.

### Compound Risks

There has been little mention of Compound in this report since readers are likely to be quite familiar already, but USV holders should be aware that Notional always converts lender deposits into Compound cTokens. This exposes users to all risks of Compound, including smart contract and insolvency risk. Compound is commonly viewed as a DeFi blue chip, but has had its share of complications, including [$89m in user losses](https://decrypt.co/49657/oracle-exploit-sees-100-million-liquidated-on-compound) from a faulty oracle and a smart contract bug that wrongly distributed [$80m in COMP token](https://rekt.news/compound-rekt/). 

### Custody Risk

* [Notional Governance Docs](https://docs.notional.finance/developer-documentation/on-chain/notional-governance-reference)
* [Notional 3-of-5 Treasury](https://etherscan.io/address/0x22341fB5D92D3d801144aA5A925F401A91418A05)
* [Notional 2-of-3 Pause Guardian](https://etherscan.io/address/0xD9D5a9dc6a952b7aD6B05a983b399537B7c0Ee88#readProxyContract)
* [Phuture 2-of-3 Multi-sig](https://etherscan.io/address/0x6575A93aBdFf85e5A6b97c2DB2b83bCEbc3574eC)

Both Phuture and Notional have a centralization vector with the ownership authority around the multisig. Notional is fully upgradeable and all system parameters are controlled by the treasury multisig. This is composed of two Notional team members (Jeff and Teddy) and three community members. Additionally, the pause guardian multisig can pause all system functions and is controlled by the Notional company.

The Phuture multisig has significant control over the USV system and is controlled by members of the Phuture team. It allows the `VAULT_MANAGER_ROLE` to call `setMaxLoss`, `setFeeRecipient`, and `authorizeUpgrade`. **The ability to upgrade the USV contract gives the Phuture multisig control over user deposits to USV.**

C2tP points this out in the following discord convo:

![](https://user-images.githubusercontent.com/117331039/219155367-2db25323-d395-47f0-8604-5b685dec4cc0.png)

Source: [Convex Finance Discord (CVX-Voting)](https://discord.com/channels/820795644494610432/885859948402208798/1069564071411732501)


## Conclusion

- USV is exposed to several DeFi products. It relies most significantly on Notional Finance, although users should be aware that Notional utilizes Compound Finance under the hood. Additionally, the foundation of the USV product is USDC, a custodial stablecoin issued by Circle. 

- USV has a centralization vector with its ownership authority. Both Phuture and Notional are governed by team-controlled multisigs, and in both cases there is a high degree of control afforded to them.  
  
- Adequate incentives flowing towards Notional liquidity providers and borrowers are essential to support stable, reliable yield for lenders. USV holders should follow changes in Notional LP incentives and adjustments to governance parameters to help assess the opportunity cost of their position.
  
- Notional has been audited, has an active Bug Bounty ($1,000,000) with immunefi and is certified by industry leaders including Certora, ABDK, Code Arena & OpenZeppelin. Notional Finance has an on-chain reserve fund that can be utilized to help cover any losses and reduce insolvency risk.

- Phuture has several [audits](https://docs.phuture.finance/protocol/audits), including a [Peckshield audit](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Phuture-FRPVault-v1.0.pdf) of the FRPVault contract used by USV. The most significant finding is PVE-004: Trust issue of admin key, which addresses the counter-party risk posed by the `VAULT_MANAGER_ROLE`.

- Given the successful vote in favor of allocating a CRV gauge for USV-FraxBP, it is in the community's interest to monitor the performance of the pool for evidence of organic adoption. In the event that the pool fails to generate adequate volume, it may be appropriate to reevaluate the gauge.


## LlamaRisk Gauge Criteria

### Centralization Factors

**1) Is it possible for a single entity to rug its users?**

Yes. USV is governed by a 2-of-3 multisig controlled by the Phuture team. It can upgrade the contract and therefore has complete control over user deposits. It is also indirectly exposed to the Notional multisig and to Circle as the centralized issuer of USDC.

**2) If the team vanishes, can the project continue?**

Yes. USV does not require active involvement from the team for day-to-day operations. It does not rely on the team to update price oracles or to trigger rebalances/investment transactions. Investment into and settlement of Notional fCash positions are handled within the contract. USV could continue processing deposits and withdrawals indefinitely. 

### Economic Factors

**1) Does the project's viability depend on additional incentives?**

No. However, it is indirectly reliant on adequate incentives for liquidity providers on Notional. In the absence of liquidity on Notional, interest rates offered by USV may fall or become erratic to the point that the strategy is unviable.

**2) If demand falls to 0 tomorrow, can all users be made whole?**

Mostly. USV is always exposed to fCash. If all USV were to be redeemed tomorrow, fCash would be sold before maturity and become exposed to market risk. In normal market conditions realized losses should be modest, although the further away redemption occurs from maturity date will substantially increase the risk of loss. DiligentDeer created [this Desmos graph](https://www.desmos.com/calculator/dunk4utpir) to visualize interest rate risk to lenders.

### Security Factors

**1) Do audits reveal any concerning signs?**

The [Peckshield audit report](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Phuture-FRPVault-v1.0.pdf) from August 2022 found 3 medium issues and 1 low severity issue. Issues were resolved with the exception of the powerful `VAULT_MANAGER_ROLE` privileges. Peckshield recommended the team transfer control to a DAO-like governance contract and impose protective measures such as timelocks. The team instead confirmed they plan to maintain the multisig governance. 

## LlamaRisk Recommendation

A gauge has already been approved by the DAO to help bootstrap the USV product. However, there have been notably few user interaction with USV until now, as evidenced by the high percentage of team owned tokens and negligible Curve pool utilization. A gauge can improve liquidity, but it can't ensure sustainable adoption if USV is fundamentally a product without demand. We recommend the DAO allow a period of bootstrapping and reevaluate USV adoption and Curve pool performance metrics in six months to determine if the gauge should remain.

## Appendix

**Trading on the Notional AMM (Example):**

Consider a Notional liquidity pool between Dai and 1M Dai (fDai maturing in one month’s time) with the following values:

```
Scalar: 100
Anchor: 1.01
Liquidity Fee (absolute terms): .00025
Liquidity Fee (annualized interest rate terms): .30%
Total Currency: 100,000 Dai
Total fCash: 100,000 1M Dai
Proportion: .5
Prevailing Exchange Rate = (1 / 100) * ln(1) + 1.01 = 1.01
Prevailing Interest Rate (annualized) = 12%
```

A lender comes to Notional to buy 1,000 1M Dai. Notional calculates the user’s trade exchange rate and updates the pool balances accordingly.

Trade Details:

```
tradeProportion = (100,000 - 1,000) / (100,000 + 100,000) = .495
Trade Exchange Rate = (1/100) * ln(.495/.505) + 1.01 - .00025 = 1.00955
Trade Interest Rate (annualized) = 11.46%
Dai sold in exchange for 1,000 1M Dai = 990.5403 Dai
All-In Trading Fee (Dai) = 990.5403 - (1000 / 1.01) = .4403 Dai
All-In Trading Fee (annualized interest rate terms) = .54%
```

Updated Pool Details:

```
Total Currency: 100,990 Dai
Total fCash: 99,000 1M Dai
Proportion: .49502
Prevailing Exchange Rate = 1.0098
Prevailing Interest Rate = 11.76%
```
