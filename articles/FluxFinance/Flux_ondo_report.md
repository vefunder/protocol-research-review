# Asset Risk Assessment: Ondo and Flux Finance (OUSG)

### A look into the utility and challenges of tokenizing U.S. Treasuries


Resources (Useful links):

* [Proposal to add fUSDC-fDAI to the Curve Gauge Controller](https://gov.curve.fi/t/proposal-to-add-fusdc-fdai-to-the-curve-gauge-controller/9098)
* [Flux finance website](https://fluxfinance.com/)
* [Flux Finance documentation](https://docs.fluxfinance.com/)
* **Legal docs** - [Terms of Services](https://docs.fluxfinance.com/legal/terms-of-service), [Privacy Policy](https://docs.fluxfinance.com/legal/privacy-policy), [CoinList Risk factors](https://docs.fluxfinance.com/legal/coinlist-risk-factors), [Cookies policy](https://docs.fluxfinance.com/legal/cookies-policy)
* [Github Flux finance](https://github.com/flux-finance)
* [DefiLlama - Flux Finance](https://defillama.com/protocol/flux-finance)
* [Flux Finance audit](https://code4rena.com/reports/2023-01-ondo/) (additional to Compound “reputation”)
* [Flux Finance Bug Bounty on ImmuneFi](https://immunefi.com/bounty/fluxfinance/)
* Flux Finance - [governance forum](https://forum.fluxfinance.com/)
* [Flux finance blog](https://blog.fluxfinance.com/)
* [Ondo Finance website](https://ondo.finance/)
* [Ondo Finance - OUSG](https://ondo.finance/ousg)
* [Ondo Finance docs](https://docs.ondo.finance/)
* [Ondo DAO - Tally](https://www.tally.xyz/gov/ondo-dao)
* [Compound cTokens docs](https://docs.compound.finance/v2/ctokens/#ctokens)
* [MakerDAO Proposal (MIP119)](https://forum.makerdao.com/t/mip119-onboard-dai-funds-to-the-flux-finance-dai-lending-pool/19885)
* [Curve DAO Proposal (Gauge proposal)](https://gov.curve.fi/t/proposal-to-add-fusdc-fdai-to-the-curve-gauge-controller/9098)

Articles, posts, tweets and other useful content:

* [The Defiant article](https://thedefiant.io/flux-lending-tokenized-treasuries)
* [Short Term Government Bond ETF List](https://etfdb.com/themes/short-term-government-bond-etfs/)
* [Crypto Economy post](https://crypto-economy.com/defi-platform-flux-finance-rolls-out-lending-token-collateralized-by-u-s-treasury-debt/)
* [iShares Short Treasury Bond ETF](https://www.ishares.com/us/literature/fact-sheet/shv-ishares-short-treasury-bond-etf-fund-fact-sheet-en-us.pdf) - Fact Sheet
* [Twitter thread - why Bond ETF and not directly exposure](https://twitter.com/nathanlallman/status/1640773058825953280)

Adresses

* [Flux OUSG (fOUSG)](https://etherscan.io/token/0x1dD7950c266fB1be96180a8FDb0591F70200E018#code)
* [OUSG](https://etherscan.io/token/0x1B19C19393e2d034D8Ff31ff34c81252FcBbee92)
* [Timelock](https://etherscan.io/address/0x2c5898da4df1d45eab2b7b192a361c3b9eb18d9c#readContract)
* [ONDO token](https://www.contractreader.io/contract/mainnet/0xfAbA6f8e4a5E8Ab82F62fe7C39859FA577269BE3)
* [Ondo Finance Cash Manager](https://etherscan.io/address/0x3501883a646f1F8417BcB62162372550954D618f#code)
* [Coinbase Prime address](https://etherscan.io/address/0xF67416a2C49f6A46FEe1c47681C5a3832cf8856c)
* [Management multisig](https://etherscan.io/address/0xAEd4caF2E535D964165B4392342F71bac77e8367#code)
* [OUSG redemption multi-sig](https://etherscan.io/address/0x72Be8C14B7564f7a61ba2f6B7E50D18DC1D4B63D)
* [KYC Registry](https://etherscan.io/address/0x7cE91291846502D50D635163135B2d40a602dc70#code)


## Relation to Curve

Flux Finance submitted a [proposal](https://gov.curve.fi/t/proposal-to-add-fusdc-fdai-to-the-curve-gauge-controller/9098) to add the [fUSDC-fDAI pool](https://curve.fi/#/ethereum/pools/factory-crypto-237/deposit) to the Curve Gauge Controller, allowing users to assign gauge weight and mint CRV. This proposal aims to enhance liquidity and usage of the pool, which provides additional utility to Flux Finance users (fDAI or fUSDC holders). The proposal successfully passed an [on-chain vote](https://dao.curve.fi/vote/ownership/313) on April 22nd, 2023.

## TLDR

[Ondo Finance](https://ondo.finance/) is a blockchain company that tokenizes various financial products, including tokenized securities (OUSG). Its flagship product, [OUSG](https://ondo.finance/ousg), is a tokenized security backed by the [SHV ETF](https://www.ishares.com/us/products/239466/ishares-short-treasury-bond-etf), offering users exposure to short-duration US Treasuries. 

[Flux Finance](https://fluxfinance.com/) is a lending protocol developed by the technology arm of Ondo. It is designed to complement Ondo's products with additional utility. Flux Finance facilitates stablecoin loans against Ondo's tokenized US Treasuries collateral. The protocol is governed by the Ondo DAO, with discussions taking place on the governance forum and votes held on Tally.

LlamaRisk conducted an extensive risk analysis on Ondo Finance and Flux Finance, revealing several risk vectors:

1. Smart Contract Risk: Ondo's smart contracts underwent an audit by C4A, identifying one high-severity vulnerability, five medium-severity vulnerabilities, and numerous low-severity and non-critical issues. The Ondo team worked with C4A to resolve these vulnerabilities, particularly the high-risk related to "Loss of user funds when completing CASH redemptions".
2. Governance Risk: Ondo employs a two-stage governance process to ensure community involvement and reduce potential risks. Despite this, voting power distribution appears highly centralized, with two governors, "Glass markets" and "vexmachina.eth," holding a significant share of voting power. A treasury multi-sig arrangement controls 88% of the ONDO token total supply, raising concerns about centralized decision-making.
3. Centralization Risk: Ondo Finance relies on centralized exchanges like Coinbase and third-party service providers like NAV Consulting. Many of its products are held in third-party ETFs such as the Blackrock's iShares Short Treasury Bond ETF. This reliance could expose the platform to additional risks associated with centralized institutions.
4. Collateral/Solvency Risk: Flux Finance presents a low risk of insolvency, since it only includes highly liquid, low volatility assets as collateral (short-term U.S. treasuries). However, liquidators must be whitelisted by Ondo to handle OUSG, which may contribute to slow response times and slightly increasing the possibility of bad debt accrual.
5. Oracle Risk: Ondo Finance uses NAV Consulting for daily price feeds and its custom price oracle, OndoPriceOraclev2, for managing markets. However, third-party oracle providers introduce potential risks to the protocol as there is no on-chain feed for the price of its assets.

The OUSG token is notably permissioned due to regulatory requirements associated with its underlying positions. However, Flux's fTokens, representing a variety of stablecoin lending deposits (like Compound cTokens), are permissionless.  This creates an opportunity for the Curve DAO to consider if and when permissioned/semi-permissioned protocols should be incentivized by the Curve gauge.


## Introduction to Ondo Finance

[Ondo Finance](https://ondo.finance/) is a blockchain services company that creates and manages institutional-grade financial products such as US Treasuries and money market funds, and builds DeFi protocols around those products. As for its constituent applications, Ondo seeks to develop decentralized, composable protocols and offer tailored services catering to organizations, DAOs, and high-net-worth individuals. The platform aims to bridge the gap between traditional finance (TradFi) and decentralized finance (DeFi) by onboarding real-world assets (RWAs) to DeFi.

Ondo Finance was founded in 2021 by [Nathan Allman](https://www.linkedin.com/in/nathanlallman) and has so far raised $24 million from  investors that include Pantera Capital, Founders Fund, Coinbase Ventures, and Tiger Global. Team members have backgrounds in a variety of institutions and protocols like Goldman Sachs, Fortress, Bridgewater, and MakerDAO.


### Legal Structure

Ondo Finance operates with a standard fund structure that includes a limited and general partner, as well as third-party service providers such as qualified custodians, a fund administrator, and a financial auditor. 

Below is an overview of Ondo's own legal structure:

* Ondo Finance Inc.: The parent company
* Ondo I GP: The General Partner (GP) who manages the fund and instructs service providers.
* Ondo Capital Management LLC: The Investment Manager (Ondo IM) who collaborates with the GP to manage the fund.
* Ondo I LP: The Delaware limited partnership receiving investor capital contributions and holding assets with a third party service provider. It is the issuer of OUSG.

Ondo Finance implements extensive security measures to ensure the safe and efficient management of funds, collaborating with reputable service providers like Coinbase and Clear Street. Qualified Custodians are Regulator-approved institutions holding client assets in separate accounts under the client's name. Ondo makes use of the following third-party fund service providers:

* [Clear Street](https://clearstreet.io/): The securities brokerage and qualified custodian managing off-chain assets and trade orders for the fund.
* [NAV Consulting Inc.](https://www.navconsulting.net/): Performs third-party administration services including a daily calculation of the Fund’s net asset value
* [Coinbase Prime](https://prime.coinbase.com/): Holds stablecoins, convert stablecoins to USD, and wire funds to Clear Street, as directed by the Investment Manager  

The following diagram shows shows the relationship between these entities with an explanation from the MakerDAO [forum proposal](https://forum.makerdao.com/t/mip119-onboard-dai-funds-to-the-flux-finance-dai-lending-pool/19885/9):

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/20bb04e4-58c7-40f6-bf98-caec2051f2b8)

(source: [MakerDAO Governance Forum](https://forum.makerdao.com/t/mip119-onboard-dai-funds-to-the-flux-finance-dai-lending-pool/19885/9))

> 1) Ondo Finance wholly owns the Investment Manager and GP
> 2) The Investment Manager is responsible for the purchase and sale of ETFs
> 3) The GP acts as the General Partner of the Fund
> 4) OUSG Investors send stablecoins to the Fund’s Coinbase account, purchasing OUSG tokens, which the Fund sends to the OUSG Investors
> 5) The Fund has engaged Coinbase to hold stablecoins, convert stablecoins to USD, and wire funds to Clear Street, as directed by the Investment Manager
> 6) The Fund has engaged Clear Street for prime brokerage services and utilizes Clear Street to hold and trade assets
> 7) The Investment Manager instructs Clear Street (CS), which executes trades, clears, and custodies the Fund’s assets in the Fund’s CS Account 

The Fund has established access controls to ensure security, especially for third-party transfers. Accounts at Coinbase are only permitted to send US dollar wire transfers to the Fund's account at Clear Street. The Clear Street account wires are sent and received through its bank, BMO Harris, while Coinbase wires are sent and received through its bank, Customer's Bank. To approve another account for wire transfers, the Fund must first receive a wire transfer from that bank account to the Fund's Coinbase account and then work with a Coinbase representative to configure the bank as a trusted withdrawal destination. Additionally, Ondo maintains criteria for approving new bank accounts as transfer destinations.


### Ondo I LP: The OUSG Fund

The Ondo I LP Fund was created in February 2023 with its initial product: the [Ondo Short-Term US Government Bond](https://ondo.finance/ousg) (OUSG). The single underlying asset that the Fund invests into is the [iShares Short Treasury Bond ETF](https://www.ishares.com/us/products/239466/ishares-short-treasury-bond-etf) (SHV), which is an index of U.S. Treasury bonds with a maturity of under a year. As of May 15, the ETF has $23.4 billion net assets and an average daily trade volume over $300 million. 

The Fund automatically reinvests dividends earned by the underlying position. Fees for the fund include ETF management fees of the underlying assets (0.15%) and a management fee charged by Ondo (0.15%) with an overall fee capped at 0.3%

NAV Consulting provides [daily attestations](https://www.dropbox.com/sh/m6s7l9tiex7hex1/AACiqtviGv274DiEHiJGMO8va?dl=0) of account balances and statements of assets and liabilities. Ondo updates the contract price of OUSG daily based on this calculation. OUSG investors additionally receive monthly update reports on the NAV from NAV Consulting, and the Fund undergoes an annual audit.

The Llama Risk team has examined Ondo Finance's documentation, including the trial balance, account statement, and balance sheet, comparing them to the on-chain data. It was noted that there are no material differences between NAV Consulting reporting and on-chain data.)

The following images shows a recent attestation by the Fund Administrator, which shows the portfolio assets account ("Long Portfolio Value") and various cash accounts against the Fund liabilities. The attestation can be compared against the OUSG [outstanding supply](https://etherscan.io/token/0x1b19c19393e2d034d8ff31ff34c81252fcbbee92) and the most recent update to the on-chain share price ([lastSetMintExchangeRate()](https://etherscan.io/address/0x3501883a646f1F8417BcB62162372550954D618f#readContract#F32) in the CashManager contract). The current value on-chain value is $118.4M, which corroborates the attestation of the NAV shown in the image below.

<img width="1142" alt="Screen Shot 2023-05-15 at 3 13 09 PM" src="https://github.com/vefunder/protocol-research-review/assets/51072084/9afb8c26-ee20-482a-8cdf-352898031d25">

(source:[NAV Consulting](https://www.dropbox.com/sh/81aevv8ufzghd9p/AAApEqp7gwNUm7pFQUHJsdAta/2023-05-11?dl=0&preview=BalanceSheet_Ondo+I_20230511.PDF&subfolder_nav_tracking=1))

In terms of off-chain protection, OUSG holders have [SIPC coverage](https://www.sipc.org/for-investors/what-sipc-protects) that is capped at $500k, as Ondo Finance has an account with Clear Street, a brokerage firm that is a [member of SIPC](https://clearstreet.io/regulatory). However, the amount covered by SIPC is irrelevant if we compare it with the value of the Ondo I LP fund assets. Also worth mentioning is that the “Ondo I LP” account is a “cash account” (not a “margin account”), so Clear Street can't use account securities for rehypothecation.

Legal documentation involving the fund are made available at this [Dropbox](https://www.dropbox.com/scl/fo/g6yw3y1wu1e0p4kx05u32/h?dl=0&rlkey=u3b64grnv253u8km27bs1hck3), including a detailed disclosure of risk factors pertaining to the Fund in the Private Placement Memorandum (PPM). 


### Investment Workflow

Shares of OUSG are issued as tokens on the Ethereum blockchain and can be minted/redeemed on U.S. business days. These actions are processed by the Ondo Ops team with accounting services provided by the Fund administrator (NAV Consulting). During times of heavy redemption demand, the Fund may not have sufficient liquidity on hand, and Ondo anticipates that it is possible redemption requests can take 2-3 days to process. 

OUSG can be minted from USDC or DAI with a minimum investment of $100,000. It is available to both U.S. and non-U.S. persons, although access to mint, redeem and transfer OUSG is permissioned. Ondo uses smart contracts to enforce these transfer restrictions. Investors must undergo KYC/AML/CFT screening and be both “accredited investors” and “qualified purchasers”. Whitelisted users may only transfer their tokens on-chain to other whitelisted addresses. Whitelisted addresses are stored in the [KYCregistry](https://etherscan.io/address/0x7cE91291846502D50D635163135B2d40a602dc70) contract and is managed by the Ondo [management multi-sig](https://etherscan.io/address/0xAEd4caF2E535D964165B4392342F71bac77e8367). 

The following workflows outline the subscription and redemption process for investors contributing stablecoins:

**Subscription (issuance) Process:**

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/57aa676f-5bd9-4d28-8031-0173fc66c32a)

1. Complete KYC/AML process with Ondo by providing the required documents and passing automated screenings.
2. Review and sign fund documents.
3. Provide an Ethereum wallet address for whitelisting to make subscriptions, receive fund tokens, and perform redemptions.
4. Send USDC to the fund's smart contract for subscription.
5. The smart contract logs your subscription request and atomically (immediately) transfers the USDC to the fund’s account at Coinbase Custody.
6. After computing the next daily NAV and accepting your subscription request, you'll receive OUSG tokens representing your shares in the fund.
7. Ondo IM uses the USD at Clear Street to purchase the ETF.
8. ETF dividends are reinvested by purchasing more ETF shares, increasing the fund's value.

**Redemption Process:**

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/f5e56545-8d54-43d4-a323-6a4c51bce448)

1. Submit a redemption request by sending OUSG tokens to the Cash Manager’s smart contract.
2. The smart contract logs your redemption request.
3. Once the next daily NAV is computed and your redemption request is accepted, Ondo IM sells sufficient ETF shares to cover your redemption.
4. Clear Street will wire the resulting USD to Coinbase, who will convert it to USDC.
5. Ondo IM will complete the redemption request and distribute the USDC to the user's wallet.


### OUSG Access Control

Ondo authorizes the flow of funds between the blockchain and its Fund account service providers (Coinbase Prime for stablecoin <> USD conversion, and Clear Street brokerage account for custody and trading of ETFs). Custody agreements have been made between Ondo, Coinbase, and Clear Street on permissions and approvals for money transfers. Measures have been taken to structure access securely between brokerage and bank accounts to minimize employee access to Fund accounts.

There are two multi-sigs Ondo uses to manage the on-chain portion of their system. The team say that each member is an employee of Ondo and required to use a hardware wallet for signing. 

[Ondo 3-of-6 Cash Management Multi-sig](https://etherscan.io/address/0xAEd4caF2E535D964165B4392342F71bac77e8367)

* Configure minimum redemption and subscription amounts on the CashManager contract.
* Configure rate limiter parameters on the CashManager contract. (i.e. what quantity of subscriptions and redemptions can be serviced in a single day)
* Configure fee recipients on the CashManager contract (fees are currently turned off).
* Set exchange rates for the minting of OUSG.
* Mint OUSG to service subscriptions.
* Pause functionality on the CashManager contract in the event of an emergency
* Burn OUSG in the event of an emergency.
* Upgrade the OUSG implementation contract in the event of an emergency.
* Execute a multicall function in the CashManager contract for the scenario in which a user accidentally transfers tokens to the CashManager contract.

[Ondo 3-of-7 Redemption Multi-sig](https://etherscan.io/address/0x72Be8C14B7564f7a61ba2f6B7E50D18DC1D4B63D)

* Can send stablecoins it has possession of through the CashManager contract to service redemptions.


## Introduction to Flux Finance

Flux Finance is a decentralized lending protocol developed by the Ondo Finance team and governed by the Ondo DAO ([ONDO](https://www.coingecko.com/en/coins/ondo-finance) token holders). It is a fork of Compound V2 with minor modificications to handle permissioned tokens like OUSG. The protocol has various tokens available for lending such as USDC, DAI, USDT, and FRAX. OUSG is the only collateral asset and can not be borrowed. 

Flux's primary goal is to create utility for the OUSG asset, and generally to facilitate the onboarding of real-world assets onto the blockchain in a compliant manner. This approach to decentralized finance (DeFi) seeks to ensure that every token operates within a suitable framework, promoting an environment that balances accessibility and compliance. 

The figure below depicts how Ondo and Flux ecosystems interact with each other:

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/9ce86712-ed9c-415a-b539-c1493bb423cb)

(source: [MakerDAO Governance Forum](https://forum.makerdao.com/t/mip119-onboard-dai-funds-to-the-flux-finance-dai-lending-pool/19885/9)) 


### fTokens

fTokens are analogous to Compound's commonly recognized [cToken standard](https://docs.compound.finance/v2/ctokens/). Flux Finance allows lenders to earn interest on their stablecoins by supplying them to the platform and minting fTokens. These ERC-20 tokens represent balances on the protocol and earn interest through the fToken/Token exchange rate. The interest earned by the protocol is not directly distributed to lenders. Instead, the fToken exchange rate increases over time, allowing users to redeem more assets as interest accrues. Supply and borrow rates on Flux Finance are algorithmically determined based on supply and demand.

fTokens have additional functionality to support permissioned token restictions, so fOUSG can only be transferred between addresses that are whitelisted to own OUSG. Any interaction with fOUSG, including mint, redeem, or transfer involves a check to the [kycRegistry](https://etherscan.io/address/0x7cE91291846502D50D635163135B2d40a602dc70) contract where whitelisted addresses are stored. Additionally, transfers that would result in negative account liquidity for borrowers will fail, ensuring the stability and security of the protocol.

Several parameters are set in the [Unitroller contract](https://etherscan.io/address/0x95Af143a021DF745bc78e845b54591C53a8B3A51#code) that affect the OUSG lending market:

* Collateral Factor: A value set between 0 and 98% that represents the value that can be borrowed against value supplied.
* Close Factor: A value between 5% and 90% that represents the amount of a liquidatable account’s borrow that can be repaid in a single liquidate transaction.
* Liquidation Premium: An additional percent share of a liquidated value sent to the liquidator as compensation.

At the moment, the OUSG Collateral Factor is set to [92%](https://fluxfinance.com/market/ousg), Close Factor to [50%](https://etherscan.io/address/0x95Af143a021DF745bc78e845b54591C53a8B3A51#readProxyContract) and Liquidation Premium to [5%](https://etherscan.io/address/0x95Af143a021DF745bc78e845b54591C53a8B3A51#readProxyContract).

The following tokens are available to lend on Flux (with fToken contract):

* Flux USDC ([fUSDC](https://etherscan.io/token/0x465a5a630482f3abD6d3b84B39B29b07214d19e5))
* Flux DAI ([fDAI](https://etherscan.io/token/0xe2bA8693cE7474900A045757fe0efCa900F6530b))
* Flux USDT ([fUSDT](https://etherscan.io/token/0x81994b9607e06ab3d5cF3AffF9a67374f05F27d7))
* Flux FRAX ([fFRAX](https://etherscan.io/token/0x1C9A2d6b33B4826757273D47ebEe0e2DddcD978B))

The following token is available as collateral on Flux (with fToken contract):

* Flux OUSG ([fOUSG](https://etherscan.io/token/0x1dD7950c266fB1be96180a8FDb0591F70200E018))

According to the Defi Llama which uses a TVL calculation method that includes the borrowed amount, in early May 2023 the Flux protocol has a TVL of $57.95m. of which 60% is OUSG. USDC is by far the most supplied asset, followed by DAI. 

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/bf64d33c-2472-4e7c-be55-15387b8b87bf)

### Curve fUSDC/fDAI Pool

The [Flux Curve pool](https://curve.fi/#/ethereum/pools/factory-crypto-237/deposit) has so far had minimal usage, although it was only deployed a month before this writing. The pool was seeded with $2M by a [team multi-sig](https://etherscan.io/address/0x7fbe0de6ffa86f4b9528aa27029595429b0c74a9#tokentxns). It has not yet received any meaningful trade volume.

The pool was deployed as a V2 pool meant for assets that do not keep a 1:1 peg. This was done to account for variation in interest accrual between fUSDC and fDAI. The team was interested in a pool with parameters as close to XY=k as possible while still rebalancing liquidity. They opted to use minimum values for A and gamma parameters, which is an unusual choice that the team felt is most suitable for the purpose of this pool. 

The protocol targets an optimal borrowing rate, beyond which the borrowing rate rapidly increases. The Curve pool can help arbitrage the fTokens around the optimal rate and additional incentives on Curve may contribute to increased demand for lending on Flux.

<img width="1586" alt="Screen Shot 2023-05-16 at 10 18 44 AM" src="https://github.com/vefunder/protocol-research-review/assets/51072084/dbda1d27-d05b-4f19-9262-6f3daad9be63">

Source: [Flux Markets](https://fluxfinance.com/markets)


### Overview of the Flux Finance Ecosystem

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/0671dcae-d6a5-42b9-b9b0-c31d0e9914b8)

(source: [Flux finance markets](https://fluxfinance.com/markets) )

The illustration above reveals around 90% utilization rate for permissionless fTokens as an equilibrium where OUSG yield (underlying asset - SHV ETF yield) matches the borrowing cost of supported stablecoins. Given that the permissioned token OUSG is exclusively used as collateral (permissionless fTokens can only be borrowed) it can be deduced that permissionless fTokens at a 90% utilization rate signifies the maximum capacity at which OUSG lenders can borrow without experiencing a negative APY on their loan.

* Borrow APY when utilization rate is 90%: 4.41%

* Borrow APY 91% when utilization rate is 91%: 4.78%

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/d3f0924d-4739-4c36-9fdd-6e3ccb478829)

(source: [Ondo Finance Twitter](https://twitter.com/OndoFinance/status/1649051019756847105))

By accounting for management costs and reducing the underlying collateral APY accordingly, OUSG depositors currently experience a yield of 4.3%. By comparison, the average cost of borrowing is 4.575%, incurring a minimal overall cost to borrowers.

Given the current lending protocol (and respectively fTokens) utilization rate, it is beneficial to add/assign some extrinsic productivity to fTokens to fulfill the demand for using OUSG as collateral. 

Ondo Finance team already works on fTokens composability; aside from the current Curve proposal, they have made a [proposal to MakerDAO](https://forum.makerdao.com/t/mip119-onboard-dai-funds-to-the-flux-finance-dai-lending-pool/19885). MIP119 proposes to create a DAI 500MM vault for lending to the Flux Finance DAI Lending Pool. Another proposal with Frax recently passed a [Snapshot vote](https://snapshot.org/#/frax.eth/proposal/0x11e285e59830fe2a6dec50138e335d7ba69036da2f915db465b38a64623b3717) to activate an AMO that lends up to 2 million FRAX on Flux. Funds from this proposal are still awaiting deployment.

At the moment, there are [33 OUSG token holders](https://etherscan.io/token/0x1b19c19393e2d034d8ff31ff34c81252fcbbee92#balances). The largest holder is Flux Finance (fOUSG) with a ~31.05% share of the total supply. Given that currently the OUSG token is capital productive only as collateral on the Flux protocol, the relationship between the fOUSG supply and total OUSG supply can serve as an indicator of how much capacity is utilized in relation to the potential (maximum) OUSG capacity.

Regarding the permissionless side of the protocol, fUSDC has [420](https://etherscan.io/token/0x465a5a630482f3abd6d3b84b39b29b07214d19e5#balances) token holders, fDAI [160](https://etherscan.io/token/0xe2bA8693cE7474900A045757fe0efCa900F6530b), fUSDT [76](https://etherscan.io/token/0x81994b9607e06ab3d5cf3afff9a67374f05f27d7), and fFRAX only [7](https://etherscan.io/token/0x1c9a2d6b33b4826757273d47ebee0e2dddcd978b) holders. Although supplying (lending) [interest rates are competitive](https://defillama.com/yields?token=USDC&category=Lending&category=CDP) against larger money market protocols, on-chain adoption seems relatively low. 


### Flux Finance Governance

Flux Finance is governed by the [Ondo DAO](https://www.tally.xyz/gov/ondo-dao). ONDO token holders have control over protocol economic parameters, smart contract upgrades through on-chain proposals, and control the OUSG oracle and lending protocol interest rate model contracts. Although the ONDO token is still non-transferable, users can use the token to vote on DAO proposals or delegate voting power to another account.

Ondo DAO governance follows a standard two-step process:

1. [Forum discussion](https://forum.fluxfinance.com/)
2. On-chain voting (managed by [Tally](https://www.tally.xyz/gov/ondo-dao))

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/5f8c30a2-e885-4156-acc7-184fd7d77329)

(source: [Flux finance docs](https://docs.fluxfinance.com/governance/ondo-dao))

The ONDO token has a max total supply set to 10,000,000,000 ONDO tokens with the following token allocation and vesting schedule:

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/96e37a19-9194-4dcd-a421-97be1aa19d91)

(source: [Vestlab](https://vestlab.io/ondo-finance))

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/a8009b8f-f359-4b92-a180-f99ad63a796c)

(source: [Etherscan](https://etherscan.io/address/0xfaba6f8e4a5e8ab82f62fe7c39859fa577269be3#readContract))

At the time of writing, the ONDO token boasts [9,770 holders](https://etherscan.io/token/0xfaba6f8e4a5e8ab82f62fe7c39859fa577269be3#balances), all of whom have completed KYC during the public and private sale events. These token distribution initiatives were facilitated through the [Coinlist platform](https://blog.coinlist.co/announcing-the-ondo-token-sale-on-coinlist/), where 11.31% of the total ONDO supply was allocated. The remaining supply, accounting for 88.69% of undistributed ONDO, is held in the[ treasury multi-signature](https://etherscan.io/address/0x677fd4ed8ae623f2f625deb2d64f2070e46ca1a1#readProxyContract) wallet.

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/4915f637-4daa-4816-9ae1-f3e8bbdd14ba)

(source: [Etherscan](https://etherscan.io/token/0xfaba6f8e4a5e8ab82f62fe7c39859fa577269be3#balances))

Per the Ondo DAO governance profile on [Boardroom](https://boardroom.io/ondoDao/overview), the platform has seen six proposals since its launch, with 762 participating voters and a total of 1,589 votes cast. Examining the delegate landscape, the two largest accounts ([Account1](https://boardroom.io/voter/0x118919e891D0205A7492650AD32E727617FA9452https://boardroom.io/voter/0x118919e891D0205A7492650AD32E727617FA9452) and [Account2](https://boardroom.io/voter/0xD2e6E930E25456fFcD4Df0124563cC334F3284f4)) collectively hold approximately 70% of the DAO's total voting power. While these accounts have voting restrictions, they can create and submit new proposals.

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/2f52ca0f-bba2-4f38-a464-1394397624a7)

(source: [Boardroom - Ondo DAO](https://boardroom.io/ondoDao/delegates))

The two accounts with the highest voting power possess 202,806,000 ONDO, contributing to a voting weight of approximately 70%. However, these accounts are subject to voting restrictions, leaving 30% of the weighted voting power available, which equates to roughly 86,916,850 ONDO tokens. The three delegates with a combined 65.28% share of the total weighted voting power include:

1. [glassmarkets.eth](https://boardroom.io/voter/0x78A8AE116443B61Dadb88D186A0d9d6630F61259) - ~24.063m VP(894 delegators)
2. [0xcd7979e12E2A502a280270827077Fd7f206f9a44](https://boardroom.io/voter/0xcd7979e12E2A502a280270827077Fd7f206f9a44) (inactive during previous proposals) - 20.52m VP (193 delegators)
3. [vexmachina.eth](https://boardroom.io/voter/0xF8dFC5643A1d76900E823433a2a2F486559Db62A) - 12.164m VP (33 delegators)

The voting restrictions for the two above-mentioned accounts are set by the admins in the settings on the Tally page. 

It is evident that the Ondo Finance team has control over all decisions related to the Flux protocol. Although it is stated on the Tally page that the two accounts with the highest voting power are non-voting accounts, the same is not implemented (restricted) in Governor smart contract. In this case, "non-voting" accounts can be included in the voting process at any time.


### Flux Finance Multi-sigs

In addition to the two multi-sigs employed by Ondo for management of the OUSG asset, Flux utilizes two multi-sigs for treasury and operational management. Flux says that all members are employees of Flux Finance (a BVI company). These wallets consist of:

**Flux Protocol Treasury [3/6 multi-sig](https://etherscan.io/address/0x677fd4ed8ae623f2f625deb2d64f2070e46ca1a1#readProxyContract)** holds over 88.7% of ONDO token supply.

**Neptune Foundation (fluxfinance.eth) [3/6 multi-sig](https://etherscan.io/address/0x118919e891d0205a7492650ad32e727617fa9452#code)** controls Flux protocol Interest Rate Model and Oracle contracts. Following the implementation of [FIP-04](https://forum.fluxfinance.com/t/fip-04-removing-trust-for-daily-rwa-price-and-yield-curve-updates/306/3), privileges from the Neptune Foundation have since been transferred to the DAO. It regularly supplies updated price data for OUSG with the constraint that the price cannot change more than 100bps per day. With the recent implementation of FIP-04, the 100bps constraint is done by [this address](https://etherscan.io/address/0xc53e6824480d976180A65415c19A6931D17265BA).


## Risk Vectors

### Smart Contract Risk

Ondo Finance's smart contracts have undergone an [audit](https://code4rena.com/reports/2023-01-ondo/) performed by code4rena, which assessed the security and potential vulnerabilities of the code. The audit evaluated 19 smart contracts, five abstracts, and six interfaces, totaling 4,365 lines of Solidity code.

The Ondo team has worked alongside C4A to resolve any critical vulnerabilities in the smart contracts. The C4A auditors identified six unique vulnerabilities, with one classified as high severity and five as medium severity. Additionally, the audit included 54 reports detailing low-severity or non-critical issues and 24 reports recommending gas optimizations.

The key high-risk finding, titled "Loss of user funds when completing cash redemptions" concerns the completeRedemptions function in the CashManager contract. The issue arises from the total refunded amount not being updated in the _totalBurned_ storage variable for the given epoch. If the manager completes refunds and redemptions at different steps or stages for a given epoch, using multiple calls to the_ completeRedemptions_ function, any refunded amount will not be considered in subsequent calls to the function. This discrepancy could lead to users receiving fewer collateral tokens than expected, even if they redeemed the same amount of CASH tokens, resulting in a loss of funds for the users. The Ondo team worked with C4A to [resolve](https://github.com/code-423n4/2023-01-ondo-findings/issues/325#issuecomment-https://github.com/code-423n4/2023-01-ondo-findings/issues/325#issuecomment-1410627920https://github.com/code-423n4/2023-01-ondo-findings/issues/325#issuecomment-14106279201410627920) this vulnerability. 

Among the medium-severity findings, it is important to highlight the "[First Deposit Bug](https://akshaysrivastav.hashnode.dev/first-deposit-bug-in-compoundv2-and-its-forks)" vulnerability identified in Compound v2 smart contracts. This vulnerability enables exploiters to misappropriate funds from initial depositors of newly deployed cToken contracts. The Ondo team addressed this issue by enforcing a minimum deposit that cannot be withdrawn by minting a small number of cToken units to the 0x00 (dead) address during the first deposit.

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/c96d9b1b-091d-45cb-8f36-a3550151be45)

Flux Finance maintains an active [bug bounty program](https://immunefi.com/bounty/fluxfinance/) for its protocol smart contracts, hosted on ImmuneFi. The program offers a reward payout range between $1,000 and $550,000 USD, divided into four categories based on the severity or impact level of the discovered vulnerabilities:

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/855d9c4d-226c-4e40-822e-07d81dd0a55b)

Source: [ImmuneFi](https://immunefi.com/bounty/fluxfinance/)

Ondo Finance already paid one bug bounty reward of $25,000 for identifying a [high-risk vulnerability](https://iosiro.com/blog/high-risk-vulnerability-disclosed-to-ondo-finance) to security researcher [Ashiq Amien](https://ashiq.co.za/) on 26 January 2022. That issue was related to the [TrancheToken smart contract](https://etherscan.io/address/0xb279d1ed3848cee8ba6dba426be620a289ccef10#readContract), part of the first Ondo Finance product - [Ondo Vaults](https://v1.ondo.finance/). This was a finance protocol built on top of Uniswap that predates OUSG and has since been decomissioned.


### Governance Risk

Flux Finance employs a two-stage governance process involving a forum discussion and an on-chain vote to ensure community involvement and reduce potential risks. Governance proposals are typically initiated with a post on the [Flux Finance Governance Forum](https://forum.fluxfinance.com/), where community members and the team can provide feedback. Although not mandatory, this step increases the likelihood of a well-aligned and successful proposal.

A finalized proposal is submitted for a binding on-chain vote after the forum discussion. Flux Finance's DAO, a fork of Compound's Governor Bravo, manages on-chain voting through Tally. Voting power is determined by ONDO token ownership, and holders can delegate their voting power to other wallets.

Key DAO parameters include:

* Proposal Threshold: A minimum of 100,000,000 ONDO in voting power is required to submit a proposal, which helps to prevent spam or malicious proposals.
* Voting Period: A 3-day window during which community members can cast their votes.
* Quorum: Proposals require at least 1,000,000 ONDO in voting power to pass.
* Timelock: A 1-day delay period between the end of the voting period and the execution of successful proposals.

This governance structure ensures community engagement, minimizes risks, and promotes transparency in Flux Finance's decision-making process.

Upon examining Ondo DAO's voting power distribution on Tally, we observe that the governance appears to be highly centralized. The two governors, "[glassmarkets.eth](https://etherscan.io/address/0x78A8AE116443B61Dadb88D186A0d9d6630F61259)" and "[vexmachina.eth](https://etherscan.io/name-lookup-search?id=vexmachina.eth)," collectively hold approximately 34.91 million ONDO tokens (including delegated tokens). In comparison to the [proposal with the highest participation rate](https://www.tally.xyz/gov/ondo-dao/proposal/3), these two accounts collectively hold a substantial share of voting power, amounting to approximately 73.57%.

Additionally, it is worth noting that the distribution of voting power within the platform is relatively concentrated. Specifically, three wallets hold a combined 65.28% of the total voting power (eligible to vote at this moment). This concentration of influence may raise concerns regarding the platform's governance and decentralization, emphasizing the need for a more balanced distribution of voting power among participants.

This centralization of voting power raises concerns about these entities' influence over the decision-making process within Ondo DAO's governance. Especially entities such as GlassMarkets, where they only own 57 Ondo but have 894 delegated, making them one of the largest voters in the DAO.

![image](https://github.com/vefunder/protocol-research-review/assets/51072084/1c037af3-d6e2-4f82-ac09-31e4d3cb4e1d)

Source: [Tally - Ondo DAO profile](https://www.tally.xyz/gov/ondo-dao/proposals)


### Custody Risk

When assessing centralization risks, it's crucial to consider the underlying assets and infrastructure supporting the Ondo Finance ecosystem. OUSG is not directly backed by US Treasuries but by the SHV ETF, which tracks the ICE Short US Treasury Security Index. SHV is the [iShares Short Treasury Bond ETF](https://www.ishares.com/us/products/239466/ishares-short-treasury-bond-etf) managed by Blackrock, with approximately $23 billion in assets.

Another aspect of centralization risk within the Ondo Finance platform is its reliance on centralized exchanges like Coinbase and the Clear Street brokerage platform. This dependency on centralized service providers could expose the platform to additional counterparty risks from these institutions and exposure to regulatory uncertainty.

To address concerns about token backing and transparency, Ondo Finance leverages third-party service providers like NAV Consulting, a fund administration firm responsible for validating the fund's assets directly from its bank and custody accounts. Additionally, the fund undergoes an independent year-end audit by Richey May. While Ondo Finance manages the tokenization through its smart contracts, the fund administrator is responsible for maintaining off-chain records and providing monthly reports to investors. This process ensures that the token records and off-chain records are reconciled daily.


### Collateral Risk/Solvency Risk

There exists a possibility of bad debt accrual during extreme market volatility, although the risk can be considered quite low. Users should remain mindful of limitations and vulnerabilities that may contribute to the risk of insolvency.

Liquidations on Flux are similar to those on Compound V2, with accounts becoming subject to liquidation when their LTV becomes insufficient. At that point, a third-party liquidator can pay off some of the borrower's debt and seize the corresponding collateral at a discount. Unlike Compound, however, Flux liquidations respect OUSG's KYC requirements. **To liquidate an account using OUSG as collateral, the liquidator must be KYC'd and whitelisted to hold that token.** A restricted pool of authorized liquidators may increase the possibility that liquidations are not fulfilled in a timely manner.

Liquidations are expected to be rare. Flux currently only supports stablecoin markets, which are typically not volatile. However, in scenarios of extreme volatility, an account's equity might become negative when the LTV increases too quickly before it can be liquidated, causing the protocol and its lenders to accrue bad debt. Flux Finance's assets are generally very stable, making bad debt accrual extremely unlikely. As an additional safety mechanism, Flux's stablecoin oracles never price stablecoins above 1 USDC, reducing the risk of external oracle manipulation.

The Flux team on their assessment of the liklihood to experience bad debt:

> Bad debt accrual should be extremely unlikely on Flux since its assets, tokenized Treasuries, are generally very stable. The greatest weekly move ever in SHV, the short-term Treasuries ETF that OUSG invests into, is less than 0.5% since its inception in 2007. Considering that liquidations of loans against OUSG begin at 92% LTV, this provides a tremendous margin of safety to lenders on Flux.

Source: [MakerDAO Forum](https://forum.makerdao.com/t/mip119-onboard-dai-funds-to-the-flux-finance-dai-lending-pool/19885)

In the unlikely event of bad debt accrual, Flux's reserves in that market are first used to cover the losses. If there aren't enough reserves, some lenders may not be able to withdraw their assets.


### Oracle Risk

The protocol for tokenized securities employs [NAV Consulting](https://www.navconsulting.net/) services to provide a daily updated price feed mechanism, ensuring accurate valuation of the underlying collateral. This is only a temporary [solution](https://docs.ondo.finance/faq#is-there-an-observable-token-price), as the Ondo team is working on an on-chain oracle to provide live price updates.

NAV Consulting has limited API access to the Fund's accounts at Coinbase and Clear Street, which only allows them to view data without the ability to make any changes. On daily basis, NAV Consulting uses a specific method to compute the Net Asset Value (NAV) per Token, and can be described in this way (three steps):

* sum the present worth of all the fund assets (SHV shares, cash, and stablecoins)
* then subtracting fund’s accrued expenses and management fees 
* result is then divided by the total number of Tokens 

Using NAV Consulting calculation, Ondo updates the contract price on daily basis.

Flux Finance has recently implemented a [governance proposal](https://www.tally.xyz/gov/ondo-dao/proposal/7) to increase transparency in price feeds and reduce the protocol's dependence on the team. One of the proposal's key components is the deployment of a new oracle under the control of Ondo DAO. This oracle will serve as the Flux Finance protocol's primary mechanism to retrieve underlying asset prices. The proposal also implements a 100bps limit for daily price fluctuations of OUSG, effectively mitigating risks associated with price volatility.

The newly implemented price oracle, [FluxOracle](https://etherscan.io/address/0xa42e17f72aefc6ae585a08e6058a38ec036d37ec#code), is used for managing markets. This contract implements a hardcoded price for underlying asset of stablecoin fTokens (OracleType - 1), and RWAOracleRateCheck for checking underlying asset price of “permissioned” fTokens, currently only fOUSG (OracleType - 2). Additionally, the contract offers options to configure Chainlink oracles (OracleType - 3).

<img width="522" alt="Screen Shot 2023-05-17 at 6 49 27 AM" src="https://github.com/vefunder/protocol-research-review/assets/51072084/c78f007c-5ee4-4f75-bacf-be9ff4be4af8">

Source: [Contract Reader- FluxOracle](https://www.contractreader.io/contract/mainnet/0xA42e17F72aEFC6Ae585A08E6058A38ec036D37ec#fluxoracle-1-14-80)

FluxOracle contract also has implemented role-based access control with DEFAULT_ADMIN_ROLE that can set roles to an arbitrary address for each for each OracleType: 

* "STABLECOIN_HARDCODE_SETTER_ROLE"
* "TOKENIZED_RWA_SETTER_ROLE"
* "CHAINLINK_ORACLE_SETTER_ROLE"

All roles are set to the timelock contract controlled by Ondo DAO.

<img width="978" alt="Screen Shot 2023-05-17 at 6 50 27 AM" src="https://github.com/vefunder/protocol-research-review/assets/51072084/5f76b362-74c0-4d6d-8a0a-4c3e054d473d">

Source: [Contract Reader- FluxOracle](https://www.contractreader.io/contract/mainnet/0xA42e17F72aEFC6Ae585A08E6058A38ec036D37ec#fluxoracle-1-14-44)

Flux have been testing a Chainlink price feed for SHV/USD. This price feed has been deployed and they have a contract being tested on mainnet that constrains price updates based on the SHV/USD feed. [This contract](https://etherscan.io/address/0x0502c5ae08E7CD64fe1AEDA7D6e229413eCC6abe#code) will be used by the official Flux Oracle in the near future.


## Llama Risk Gauge Criteria

Centralization Factors

**1. Is it possible for a single entity to rug its users?**

While it is possible for a single entity to exploit the protocol, several safeguards are in place to minimize this risk. Ondo Finance employs three multi-signature wallets (Ondo management multi-sig, OUSG redemption multi-sig, and ONDO token holder multi-sig), each with a minimum execution threshold of three signatures. 

Although this setup theoretically allows for the coordination of the three multi-signature signers to exploit the system, the requirement of multiple signatories adds an additional layer of security. This structure helps to mitigate the risk of a single entity compromising the protocol and ensures that decision-making power is distributed among multiple parties.

**2. If the team vanishes, can the project continue?**

As a RWA issuer, OUSG is wholly dependent on the team's continued operation for management of Ondo I LP (the Fund). 

The Flux protocol currently requires manual price updates from the team, although this may transition to a Chainlink price feed in the near future. At that point, Flux could continue fully autonomously (although since the Flux team is also the Ondo team, the project is still reliant on Ondo's continued operation).

Economic Factors

**1. Does the project's viability depend on additional incentives?**

Ondo Finance does not depend on additional incentives for its continued viability. The project has been developed with an emphasis on its essential financial services, which indicates that its sustainability is not reliant on external incentives. However, it is essential to monitor any future developments or changes in the project's structure that could affect its risk profile is essential.

**2. If demand falls to 0 tomorrow, can all users be made whole?**

If demand falls to zero tomorrow, the OUSG token's backing by the SHV ETF aims to provide a foundation for redemptions. In such an event, the SHV ETF backing is designed to ensure that Ondo Finance has the capacity to continue honoring redemption requests, making all users whole and offering a level of financial security and reassurance. SHV is highly liquid, with >$300m/day in average volume, and the short-duration bonds are less susceptible to changing interest rates.

Usual fixed income risks remain present, with interest-rate and credit risks being among the primary concerns. Generally, as interest rates increase, there is a tendency for bond values to decrease. Credit risk pertains to the possibility that the bond issuer may not fulfill their obligations concerning principal and interest payments. It is essential for investors to understand that investments in the fund are not insured or guaranteed by the FDIC or any other government agency. These risks are associated with the US Treasury market in general and not with Blackrock/Ondo.

Security Factors

**1. Do audits reveal any concerning signs?**

The audit conducted by C4A on Ondo Finance's smart contracts did reveal several vulnerabilities, including one high-severity issue and five medium-severity issues. One of the medium-severity issues identified is the "First Deposit Bug," which could potentially be exploited to steal funds from initial depositors of a freshly deployed CToken contract. The bug involves the manipulation of the exchange rate due to a dependency on the ratio of CToken's total supply and the underlying token balance of the CToken contract. An attacker could exploit this vulnerability by following specific steps to redeem their entire CToken balance for the contract's underlying token balance.

However, it is important to note that the Ondo team has worked closely with C4A to address and resolve any critical vulnerabilities in the smart contracts. The key high-risk finding, titled "Loss of user funds when completing CASH redemptions," has been resolved in collaboration with the auditing team.

The proactive approach taken by the Ondo team to rectify these issues demonstrates their commitment to ensuring the security and stability of the platform.


## Risk Team Recommendation

Upon evaluating Ondo Finance and Flux Protocol, we recognize that there are areas where improvements can be made to enhance the platform's security, decentralization, and transparency. As Ondo Finance progresses, we advise the Curve DAO and community members to closely monitor its development. 

1. Address the centralization of governance and voting power in Ondo DAO. Implementing mechanisms to reduce the concentration of voting power can help promote a more decentralized and democratic governance system. It is crucial to ensure that decision-making processes are more inclusive and that influence is distributed among a larger group of participants.
2. Improve the security and stability of Ondo Finance by addressing potential risks in the smart contracts, oracles, and collateral. Regular audits and updates to the platform's security features will contribute to a more robust and reliable ecosystem. It is essential to ensure all identified vulnerabilities are resolved and measures are in place to prevent future issues.
3. Enhance the transparency of Ondo Finance's operations by providing more detailed documentation on the platform's functionality, risks, and mitigation strategies. This will allow users to make informed decisions about participating in the platform and contribute to a better understanding of the project's goals and potential risks.

Overall, Ondo and Flux appear to be refreshingly professional and take every reasonable precaution to ensure the security of their system and provide assurances to their users. Our view is that Flux constitutes an excellent example of onboarding regulatory compliant RWAs into DeFi and we would like to see further integrations with Curve.
