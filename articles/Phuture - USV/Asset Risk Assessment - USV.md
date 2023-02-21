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
In January 2023, Phuture made a [proposal](https://gov.curve.fi/t/proposal-to-add-usv-fraxbp-to-the-gauge-controller/8687) on the Curve governance forum to add the [USV/FraxBP](https://curve.fi/#/ethereum/pools/factory-crypto-202/deposit) pool to the Gauge Controller. This report seeks to identify relevant risks to Curve’s liquidity providers. 

## Phuture - USDC Saving Vault (USV) Overview

**Useful Links**

- [Phuture](https://www.phuture.finance/) Website
- [Phuture Docs](https://docs.phuture.finance/introduction/master)
- [Phuture Blog](https://www.phuture.finance/blog)
- [Phuture Github](https://github.com/Phuture-finance)
- [Phuture Audits](https://docs.phuture.finance/protocol/audits)
- [Phuture USV Contract Addresses](https://docs.phuture.finance/protocol/contract-addresses#phuture-savings-vaults)
- [Phuture 2-of-3 multisig](https://app.safe.global/eth:0x6575A93aBdFf85e5A6b97c2DB2b83bCEbc3574eC/home)

Phuture is a crypto index platform that offers its users exposure to baskets of DeFi tokens through its on-chain index funds and savings vaults. Its newest product, the USDC Savings Vault (USV), seeks to simplify stable coin services. Users simply deposit USDC into the vault and receive the USV receipt token. By incorporating liquidity provisioning, lending and borrowing, and RWA-based investments under the hood, Phuture aims to reliably generate yields on USDC.

Phuture utilizes Notional Finance, a fixed yield DeFi lending platform, to introduce their savings vault product for USDC. USV offers an optimized interest rate by dynamically investing in a blend of 3 and 6-month bonds. Since the USV product is predominantly exposed to Notional Finance, a large portion of this report will cover that platform.

When USDC is deposited into Phuture, it is lent on Notional to the most liquid tenors (between 3-month and 6-month tenors). USDC deposits may sit idle until a minimum threshold of USDC is ready to be invested in an active and eligible tenor. During the redemption process, USV utilizes a waterfall structure that first pays out any available USDC before selling positions. To protect future returns, USV will sell positions sorted by the lowest yielding tenor first.

The following flowchart shows the strategy employed by USV:

![USV_strategy_20230220](https://user-images.githubusercontent.com/51072084/220230334-e1e0a50c-dbe7-453a-a5b4-94b2afa39033.png)

## USV and Phuture Product Adoption

After conducting a thorough analysis of the USV-FraxBP pool, our findings show that as of 2/18/23, the pool has not generated any trade volumes since its inception. This has been confirmed by this [query](https://dune.com/queries/1982268), which provides further analytics.

Currently, 100% of the liquidity in the pool is supplied by the Phuture team. Specifically, 99% is provided by a multi-sig owned by Phuture ([0x237a4d2166Eb65cB3f9fabBe55ef2eb5ed56bdb9](https://etherscan.io/address/0x237a4d2166Eb65cB3f9fabBe55ef2eb5ed56bdb9)), while the remaining 1% is provided by one of the multi-sig owners ([0x9fD6Ac607AE0B13e066a609f6e5f2d41c3d04A5F](https://etherscan.io/address/0x9fD6Ac607AE0B13e066a609f6e5f2d41c3d04A5F)).

When accounting for the 271,208 USV held by their multi-sig and the 100,000 USV provided as liquidity by their multi-sig, only 7.5% out of the 401,309 total USV supply is held by wallets other than the Phuture multi-sig. Phuture has stated that they will deploy the remaining USV in their treasury to the liquidity pool once the CRV gauge is allocated.

Phuture has another product known as PDI, a DeFi index token built on Ethereum. The performance of PDI might provide insight into the community's willingness to adopt new products from Phuture. Upon analyzing the distribution of PDI tokens, we observed that the majority of the PDI supply, approximately 79%, is held in the team multi-sig treasury. As of February 18th, the total PDI supply stands at 1389.5 tokens, of which 139.37 PDI is held in a wallet, 800 PDI is supplied as liquidity on Uniswap V3, and 111.99 PDI is parked in bPDI by the treasury multi-sig.

Here is how the supply change over time correlated with the no. of holders overtime:

![image](https://user-images.githubusercontent.com/117331039/219812142-a9057f87-c24f-49f8-b171-c3ca387bf12d.png)

Source: [PDI Supply and Holders Data [v2-5]](https://dune.com/queries/2014104/3333813)

## Notional Finance

**Useful Links**

- [Notional Finance](https://www.notional.finance/) Website
- [Notional Finance Whitepapers](https://docs.notional.finance/developers/whitepaper/whitepaper)
- [Notional Finance Docs](https://docs.notional.finance/notional-v2/)
- [Notional Finance Developer Docs](https://docs.notional.finance/developer-documentation/)
- [Notional Finance Blogs](https://blog.notional.finance/)
- [Notional Analytics](https://info.notional.finance/)
- [Audit List](https://www.notional.finance/#:~:text=notional.finance.-,RECENT%20AUDITS,-Code%20Arena%2C%20Staked) from their website

Notional is a fixed-rate, fixed-term crypto asset lending and borrowing platform. This is made possible by fCash. fCash offers a simple and reliable mechanism for Notional users to commit to transfers of value at specific points in the future. Trading fCash allows users to efficiently move value back and forth through time.

### fCash

Notional Finance enables the tokenization of payments in various currencies, such as ETH or USDC, at specific future points using fCash. The interest rate for lending and borrowing on Notional V2 is defined by the fCash Markets. When users lend or borrow at fixed rates, they deposit or receive cash in exchange for fCash. Essentially, fCash represents a fixed amount of cash that a user is entitled to receive (lending) or obligated to pay (borrowing) at a specific point in time, known as maturity.

+fCash is a claim on the asset at maturity, typically held by a lender, which indicates that a user will receive the underlying assets back upon maturity. In contrast, -fCash represents an obligation to repay the capital at maturity, typically held by a borrower, which should eventually be paid back.

The exchange rate between USDC and fUSDC at the specific maturity represents the fixed interest rate that users receive on Notional.

### Liquidity Pool & Interactions

Notional maintains an on-chain liquidity pools that serve as a ready counter-party for borrowers and lenders at all time.

#### Liquidity Provider

Liquidity providers play a crucial role in contributing cTokens and fCash to liquidity pools. In return for their contribution, these providers are entitled to earn fees every time a borrower or lender trades between cTokens and fCash within the pool.

It's worth noting that both liquidity providers and borrowers can mint fCash. Each fCash token is always minted in pairs, consisting of a +fCash and a -fCash token. These two tokens represent a claim and an obligation, respectively, with the netting to a zero position change.

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MfKPITPv8YEBxbZh665%2F-MfKPizWILHrStwWO4Uc%2Fuser_types.png?alt=media&token=b2225422-a74b-4672-91f6-7f9efb1113b0)

Source: Notional Finance [Docs](https://docs.notional.finance/notional-v2/notional-v2-basics/liquidity-pools)

#### nToken

Notional provides users with a way to passively earn returns by providing liquidity to their platform, and the primary method for doing so is through nTokens. These tokens automate the process of minting fCash and depositing suitable assets into the liquidity pool with different maturities for the liquidity provider. By holding nTokens, users can earn returns from providing liquidity to Notional across all active maturities without needing to interact with the individual liquidity pools.

To mint nTokens, a user deposits cTokens into the nToken account. The account then distributes the user's liquidity into the underlying liquidity pools for that currency and holds the resultant liquidity tokens, which represent the nToken account's claim on the user's deposited liquidity. The distribution is based on a set of governance parameters known as "deposit shares," which are percentage figures that determine how much of a user's total liquidity is deposited into each individual market.

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-Me7Mven0A-wvy5XmpvY%2F-Me7OzNEjSv-9CSZLfXJ%2FnTokens_01.png?alt=media&token=e5dc53a0-f2c4-4b02-a918-548f2c452afd)

Source: Notional Finance [Docs](https://docs.notional.finance/notional-v2/ntokens/minting-ntokens)

Providing incentives to liquidity providers is critical to ensure that the essential use case of providing fixed-rate borrowing and lending services is not negatively affected.

### Notional AMM and Interest rate oracle


To optimize the trading experience, the AMM curve used by Notional must be designed with a balance between flatness and flexibility. A relatively flat curve ensures that normal trading conditions result in reasonable levels of slippage, while allowing for accommodating market repricings requires a more flexible curve.

Notional's AMM curve resembles that of Curve's stable swap invariant, which is designed to handle large changes in the equilibrium interest rate while still maintaining a relatively flat curve most of the time. This approach aims to provide a smoother trading experience for users under various market conditions.

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MeyxH0O2F9LIC6MRCp6%2F-Mez12PmZgdb9NrF-CzO%2Fimage.png?alt=media&token=ebcd677d-5828-4ed9-b099-c3b2ce570584)

Source: Notional Finance [Whitepaper](https://docs.notional.finance/developers/whitepaper/whitepaper)

Notional ensures that the exchange rate following a trade matches the prevailing exchange rate. The exchange rate used by Notional during the trade is an estimation of what the exchange rate will be once the trade is complete.

#### Liquidity pool governance parameters


Liquidity pool governance parameters have a significant influence on the shape of the AMM curve and the liquidity allocation of each liquidity pool.

- **Deposit shares** represent the percentages of incoming liquidity allocated among active liquidity pools. It is important to note that deposit shares should add up to 1 (100%). For example, for USDC, deposit shares are set at 45% for a 3-month maturity, 40% for a 6-month maturity, and 15% for a 1-year maturity.
  
- **Anchor rate**, on the other hand, determines the position of the AMM curves in the X-Y plane. It influences the tradable interest rate range. In the case of USDC, the anchor rate is set at 3.5%.
  

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MeyxH0O2F9LIC6MRCp6%2F-Mez1bvxQl21m0dDHvZj%2Fimage.png?alt=media&token=e3dff1ec-5e6e-4465-8b30-79667fd82bc7)

Source: Notional Finance [Whitepaper](https://docs.notional.finance/developers/whitepaper/whitepaper)

- The scalar rate is the parameter that affects the steepness of the AMM curves. A high scalar value corresponds to flatter AMM curves with lower interest rate sensitivity and lower slippage. Conversely, low scalar values correspond to steeper AMM curves with higher interest rate sensitivity and higher slippage. The scalar rate for USDC is set at 48.

![](https://3597103031-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MX2K6zXuGl2Zi-qgoUj%2F-MeyxH0O2F9LIC6MRCp6%2F-Mez1Qz91Eu3q-RZkPt5%2Fimage.png?alt=media&token=21c201e6-1162-41f0-8ce3-c45225d51dd4)

Source: Notional Finance [Whitepaper](https://docs.notional.finance/developers/whitepaper/whitepaper)

#### Interest Rate Oracle


In the context of liquidity pools, the last traded rate is the implied interest rate that results from the most recent trade executed on the pool. It is important to note that this rate can be subject to manipulation, especially over short time frames, such as through the use of flash loans. Such manipulation can result in a distorted or inaccurate portrayal of the true interest rate of the pool.

To address this issue, an oracle rate is used to minimize the risk of price manipulation. The oracle rate provides a dampened price feed, lagged behind the last traded rate, which converges to the true interest rate of the pool over a set time window determined by governance. By providing a more stable and accurate representation of the interest rate, the oracle rate helps to maintain the integrity of the liquidity pool and protect its participants.

#### Borrower

In order to borrow funds on the Notional protocol, a borrower must first mint both positive and negative fCash tokens based on their chosen collateral and maturity. These tokens will represent their obligation to repay the borrowed funds at a later date.

Once the fCash tokens have been minted, the borrower can then exchange the positive fCash for cTokens, which will allow them to access the funds they need. However, it is important to note that at the end of the trade, the borrower will be left with cTokens and negative fCash, which represents their obligation to repay the capital.

If a borrower is unable to repay the funds they have borrowed by the maturity date, they will be subject to a penalty rate of 2.5%, or 250 basis points, and may be forcibly auto-rolled by a third party. It is possible for the borrower to roll their debt to a future maturity at the prevailing interest rate for that maturity at any time.

**Collateralization:**

Notional strives to ensure that lenders have a high likelihood of earning the expected returns by requiring borrowers to pay back their debt at maturity. The required amount of collateral that borrowers must hold against their debt is determined based on the risk level of their collateral. For example, DAI requires a lower collateralization ratio of 120% compared to ETH's 150%.

**Liquidations:**

In Notional protocol, liquidation is a process where a liquidator buys a portion of the collateral owned by a borrower account that has fallen below a minimum required level, at a discount to the on-chain oracle price. The liquidator then uses this collateral to repay some of the debt owed by the liquidated account. Notably, even if the borrower's collateral is only slightly under the required level, the liquidator can purchase at least 40% of it.

Notional uses unprocessed Chainlink price feed to facilitate liquidations.

In the unlikely scenario where market volatility delays the liquidation process and causes potential losses to lenders, Notional has measures in place to ensure lenders' funds are protected. The protocol can use its own reserve assets or sell its native token, NOTE, to raise the necessary capital to compensate lenders and make sure they are always fully reimbursed.

The NOTE treasury can be tracked from [here](https://info.notional.finance/treasury).

#### Lender


In the Notional Finance platform, lenders have the option to exchange their assets in the form of cTokens for +fCash, which is a claim on their capital plus interest at maturity.

As with any smart contract-based financial platform, Notional Finance carries inherent risks. While the platform has been audited, has an active Bug Bounty with immunefi and certified by industry leaders including Certora, ABDK, Code Arena & OpenZeppelin, it is important to acknowledge the possibility of insolvency in the event of borrower defaults. In such a scenario, lenders may be affected as well. However, Notional Finance has taken steps to mitigate these risks through a robust liquidation protocol and infrastructure, which aim to minimize potential losses.

In the unlikely event that the liquidation procedure allows a borrower to become insolvent, Notional Finance has an on-chain reserve fund that can be utilized to help cover any losses that may arise on the platform. This on-chain reserve fund is in the form of sNOTE, which stands for staked NOTE/ETH 80/20 LP tokens. If the platform experiences losses, NOTE token holders can participate in an on-chain vote to take 50% of the assets held in the sNOTE pool and use them to recapitalize the system.

In this scenario, sNOTE holders would bear a loss of up to 50% of their assets. While it's important to understand the risks involved in using any financial platform, Notional Finance has taken steps to ensure that its users are protected to the best of its ability.

The pool holding can be verified here: [Notional Info - sNOTE](https://info.notional.finance/s-note)

## Demand-Supply Balance

The borrowing and lending market is driven by the demand to borrow and if the utility of the borrowed tokens is high, the risk reward for users to take on leveraged position gets favorable. When there is an expectation of high rewards from borrowing, the demand for borrowing increases, which results in the borrowing rate varying depending on the number of lenders willing to lend. Usually, in peer-to-peer or pool-based lending and borrowing services, the lender bears the counterparty risk.

Notional Finance, on the other hand, sets lending and borrowing rates based on the balance of assets in the liquidity pool. Liquidity providers take on the risk to deliver fixed lending and borrowing rates, sharing counterparty risk with lenders. In the event of borrower default or system insolvency, both liquidity providers and lenders bear the loss. Liquidity providers are also exposed to impermanent loss, which can lead to a net borrowing or lending position at the time of withdrawal based on the pool proportion. This net position impacts the overall yield for the liquidity providers, which is why they are incentivized with NOTE rewards.

Balancing incentives for liquidity providers is crucial for smooth pool interactions and healthy liquidity to support borrowing and lending. Various factors affect incentives and demand, including risk adjustments on collateral assets, borrowing and lending rates, minimum collateral ratio, liquidation mechanism, NOTE rewards, and liquidity pool/AMM parameters. Balancing these parameters with risk and the overall market is essential for steady yield for lenders and for the Phuture USDC saving vault.

It is important to consider the impact of changing parameters, such as raising the anchor rate, which may attract more lenders but could also distort the pool and lead to high slippage. This can impact the usability of the protocol, leading to a decrease in lending/borrowing and liquidity provision. Continual changes in the market rate for lending and borrowing necessitate a lower scalar factor to cover a wider spread of interest rate fluctuations. High fluctuations can result in the protocol's failure to capture demand for borrowing/lending due to high slippage, particularly if the scalar factor is high.


## Risks

It's important to note that USDC depositors and/or USV LPs may be affected by regulatory uncertainty, depending on their legal jurisdiction.

Phuture does have a centralization vector with its ownership authority around the multi-sig. C2tP points this in the following discord convo:

![](https://user-images.githubusercontent.com/117331039/219155367-2db25323-d395-47f0-8604-5b685dec4cc0.png)

Source: [Convex Finance Discord (CVX-Voting)](https://discord.com/channels/820795644494610432/885859948402208798/1069564071411732501)

Phuture [proposed](https://gov.curve.fi/t/proposal-to-add-usv-fraxbp-to-the-gauge-controller/8687) a CRV rewards gauge for their USV-FraxBP pool and the vote was raised and passed through the convex snapshot vote, here is the Curve [DAO vote](https://dao.curve.fi/vote/ownership/274).

## Conclusion

- Phuture is launching a savings vault product for USDC that utilizes Notional Finance to invest in a blend of 3 and 6-month bonds and offers an optimized interest rate. The USV product is predominantly exposed to Notional Finance and has a centralization vector with its ownership authority.
  
- Healthy incentives flowing towards liquidity providers and the borrowers at Notional would ensures Lenders get a stable yield. Following the incentives and change in governance parameters associated with LPs and Borrowers can help a user mitigate the risk associated with opportunity cost.
  
- In liquidity pools, the last traded rate is the implied interest rate resulting from the most recent trade, but it can be subject to manipulation over short time frames. To address this issue, an oracle rate is used to provide a more stable and accurate representation of the interest rate over a set time window determined by governance.
  
- Notional uses unprocessed Chainlink price feed to facilitate liquidations and Even if the borrower's collateral is only slightly under the required level, the liquidator can purchase at least 40% of it.
  
- The platform has been audited, has an active Bug Bounty ($1,000,000) with immunefi and certified by industry leaders including Certora, ABDK, Code Arena & OpenZeppelin. Notional Finance has an on-chain reserve fund that can be utilized to help cover any losses as the possibility of insolvency in an unexpected events cannot be ignored.

- Given the current successful vote in favor of allocating a CRV gauge for USV-FraxBP, it is in community's interest to monitor the performance of the pool to ensure that it does not attract opportunistic LPs. In the event that the pool fails to generate adequate volume, it may be prudent to consider discontinuing the CRV reward stream. Such a decision would be made in the best interest of maintaining a sustainable and equitable ecosystem for all stakeholders involved.
  

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
