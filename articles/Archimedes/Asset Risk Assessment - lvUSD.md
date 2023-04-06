# Asset Risk Assessment - lvUSD (Archimedes)

#### A look into the Archimedes leverage farming protocol, the OUSD strategy, and the lvUSD stablecoin.

### Index

- Relation to Curve
- Archimedes Overview
     - Product-Market Fit Exploration
     - Leverage Rounds
     - ARCH tokenomics
     - Archimedes Strategy Mechanics
     - Access Control
- Origin Dollar OUSD Overview
- Risk Vectors - OUSD
- Risk Vectors - Archimedes
- LlamaRisk Gauge Criteria
- Risk Team Recommendations


### Relation to Curve

Archimedes has a [lvUSD/3CRV](https://curve.fi/#/ethereum/pools/factory-v2-268/deposit) pool where LPs can provide liquidity in 3CRV to earn ARCH incentives through the Curve gauge. As of publishing time, Archimedes has not requested CRV gauge incentives. In this report we investigate the leverage farming strategy employed by Archimedes and the risks posed to LPs in the lvUSD/3CRV pool.


## Archimedes Overview

#### Useful Links

* [Whitepaper (v2)](https://docs.archimedesfi.com/archimedes-finance-whitepaper-\(v2\))
* [Docs](https://docs.archimedesfi.com/)
* Archimedes [Blog](https://medium.com/archimedes-finance)
* [Github](https://github.com/thisisarchimedes/Archimedes\_Finance)
* Important [contracts](https://docs.archimedesfi.com/technical-documentation/contract-addresses)
* lvUSD/3CRV [Curve pool](https://curve.fi/#/ethereum/pools/factory-v2-268/deposit)
* [Position locked Calculator by LlamaRisk](https://docs.google.com/spreadsheets/d/1AcDaQN4lNAXZKvilN1li10aU2XbegT9TfCQsiUki88E/edit?usp=sharing)
* [Access Control Spreadsheet by LlamaRisk](https://docs.google.com/spreadsheets/d/1GMlIJwBW6ns9eWuBmWTqZEt2NrjWzL9guzCz_8oM3uI/edit?usp=sharing)
* Audit Reports:
  * [Smart Contract -Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Nov 2022
  * [Auctions - Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Auctions\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Dec 2022
  * [Zapper - Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Zapper\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Jan 2023

Archimedes is a DeFi protocol that enables leveraged farming of yield-bearing assets. Foundational to the strategy mechanics are the lvUSD stablecoin and the Curve lvUSD/3CRV stableswap pool. The protocol periodically lends a limited quantity of newly minted lvUSD in auctions (called "leverage rounds"), which allows borrowers to amplify yield-farming returns up to 9.73x against a stablecoin collateral deposit. Only one strategy built on OUSD is currently active, but strategies may expand to other yield-bearing tokens in the future.


### Product-Market Fit Exploration

#### Problem

Archimedes argues that LPs on Curve have a suboptimal experience since CRV emissions make up the vast majority of APY, and consequently, LPs tend to experience high volatility in pool returns as protocols compete for a fixed amount of emissions. 

> We believe that CRV emissions are capping Curve's ability to scale. And we all want to see a stablecoin Curve pool that supports large investments without materially impacting the pool’s APY. Ideally, this pool provides relatively good yield, generated from real economic activity.
>
> Source: [Archimedes Docs - Motivation](https://docs.archimedesfi.com/tokenomics-and-ecosystem/motivation)

As APYs fluctuate between pools, users must switch pools to optimize their yield. High network fees make this activity inaccessible to smaller LPs.


#### Solution

Archimedes has built a strategy that makes use of Curve's stableswap pool, incentivized by its own dynamic emissions mechanism, that attempts to offer consistently superior returns to its LPs. The strategy involves protocol-owned liquidity, which gives Archimedes control of the pool's behavior, allowing it to offer its users access to profitable yield strategies.

Archimedes pays LPs in its native ARCH token. Liquidity Takers (LTs) must pay a fee (in ARCH tokens) when opening a leverage position. Demand to open a leveraged position thus produces a buy pressure on ARCH tokens. ARCH fees are then redistributed to LPs to incentivize deep liquidity and enable to protocol to mint greater amounts of lvUSD. 

ARCH thus serves as a utility token within the protocol that is required to access leverage. The ARCH yield provided by the protocol is [dynamically adjusted](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions) such that pool yields remain competitive against a subset of high TVL stable coin pools on Curve finance.


### Leverage Rounds

New [lvUSD](https://etherscan.io/address/0x94A18d9FE00bab617fAD8B49b11e9F1f64Db6b36) comes into circulation in batches minted by the protocol during leverage rounds. Archimedes periodically conducts [leverage rounds](https://docs.archimedesfi.com/taking-leverage-(borrower)/leverage-rounds) where it makes a limited supply of new lvUSD available for LTs to purchase leverage. The rounds do not have a fixed schedule, but the date and allocation of upcoming rounds are advertized on various Archimedes social channels (Twitter, Discord, Telegram etc.) prior to each round.

A leverage round is comprised of the following parameters (Note that all system parameters are subject to change):

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/ttributes.PNG)

#### Protocol Participant: Liquidity Provider

As a liquidity provider, a user simply deposits 3CRV into the lvUSD pool and receives ARCH incentives on top of swap fees generated on the pool. 3CRV LPs are the lenders of the Archimedes leverage system. Their liquidity is borrowed and deployed to a yield-generating strategy (in this case swapped into the OUSD stablecoin). LPs are making a leveraged bet that the underlying strategy will not incur a loss, and Archimedes rewards them for taking on that risk.

The protocol relies heavily on the 3CRV liquidity in the lvUSD/3CRV pool. A leverage round can only occur if the Curve pool is reasonably balanced, with the quantity of lvUSD minted dependent on liquidity depth. To ensure 3CRV liquidity, Archimedes uses a dynamic emission mechanism to consistently provide competitive yields.

#### Protocol Participant: Leverage Takers

Borrowers looking for leverage must participate in one of the leverage rounds conducted by Archimedes. 

During a leverage round, LTs can bid on available leverage. The amount of leverage available per ARCH token is determined through a Dutch auction, which starts with a low price and gradually increases until the desired amount of leverage is reached or there is no more leverage available. The ARCH leverage fee is an upfront payment to the Archimedes treasury which will eventually flow to LPs. The fee is deducted from the collateral itself and automatically swapped upon deposit, so the user does not need to own any ARCH. In addition to the dynamic leverage fee (paid in ARCH), there is a small origination fee paid in the collateral token (OUSD).

The LT provides collateral in one of the counterparty assets of the Curve pool (USDT, USDC, or DAI). This collateral serves as an input asset in the predefined strategy (e.g. Archimedes uses an OUSD strategy that converts deposits to OUSD).

#### Price stability of lvUSD

Every time a borrower takes leverage, lvUSD is minted and swapped for 3CRV. The increased quantity of lvUSD in the lvUSD/3CRV pool puts downward pressure on the price, which affects the realized loss/gain for borrowers at the time of repayment. lvUSD price also impacts liquidity providers, who can be affected by slippage and impermanent loss while depositing or withdrawing their 3CRV liquidity. 

The Archimedes team controls the amount and frequency of leverage given to the borrowers. This ensures the price of lvUSD stays near the $1 peg. The lvUSD Curve pool also has a notably high "A" value (800), which means the pool must become significantly imbalanced before inducing noticeable deviations from the peg. There is therefore a heightened responsibility placed on the team to manage the pool's balance.

The lvUSD peg is managed with the following mechanics:

* Leverage is capped: Archimedes opens a new auction only when there is enough liquidity in the pool. Each auction is for a limited amount of leverage and is designed to maintain the balance of the pool.
* lvUSD >> $1: If lvUSD rises above $1, Archimedes will raise the leverage cap. More lvUSD will be borrowed and enter circulation, returning the lvUSD peg.
* lvUSD << $1: If lvUSD drops below $1, LTs have an arbitrage opportunity by unwinding their position. LTs will pay back $1 worth of debt for less than $1, pocketing the instant arbitrage profits. This process burns lvUSD and increases the amount of 3CRV in the pool.

#### Leverage Farming Fees

There are 3 types of fees associated with the Archimedes Protocol, which are transferred to the protocol treasury multi-sig:

* Leverage Fee (ARCH Token) - When a user opens a position, they pay some ARCH tokens as an upfront fee to access leverage (lvUSD). It is not required for a user to hold ARCH tokens as Archimedes will take the fee from the collateral deposit.
* Origination Fee (OUSD) - When a user opens a position there is a one-time origination fee applied. This amount is applied to the borrowed amount and is collected from the underlying strategy asset (OUSD). The origination fee is 0.1% at the time of writing the report (down from an initial 0.5% fee).
* Performance Fee (OUSD) - The Performance Fee is 30% and it applies only to the interest earned over the lifetime of the loan (interest earned from Collateral and Leverage). This fee is taken only when the position is closed or when it expires.
* Open & Close exchange fee - 0.5% is charged on the funds deposited as collateral for leverage farming.

These fees are readable in the [ParameterStore](https://etherscan.io/address/0xcc6Ea29928A1F6bc4796464F41b29b6d2E0ee42C#readProxyContract) contract. Leverage fee = getArchToLevRatio / Origination fee = getOriginationFeeRate / Performance fee = getRebaseFeeRate. 
The effect of fees on a position profitability can be tracked on the Archimedes [calculator page](https://calculator.archimedesfi.com/?levPrice=0\&originFee=NaN).

At present, Archimedes is emitting more ARCH tokens than the leverage fee they are receiving, with the objective of preserving and attracting liquidity to their liquidity pool. However, it is important to note that the [Archimedes docs](https://docs.archimedesfi.com/taking-leverage-(borrower)/taking-leverage-explained) states that all fees denominated in OUSD are being used to incentivize LPs. As of the time of writing this report, this is not the case.

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/discord1.PNG)


### **ARCH Tokenomics**

Total Fixed Supply: 100,000,000 ARCH.

> All 100,000,000 ARCH pre-minted to Archimedes treasury to support dynamic emission schedules and experimenting with the correct way to conduct leverage auctions.
>
> Source: [Archimedes Docs - Tokenomics](https://docs.archimedesfi.com/tokenomics-and-ecosystem/tokenomics)

The ARCH token allocation is as follows:

* 50% Community: Liquidity mining incentives
* 30% Team and future team members: one-year cliff with total three linear vesting
* 15% Investors and future investors: one-year cliff with total three linear vesting
* 5% Foundation/Development program: development cost, audits, service provider, OPEX, and partnerships

#### Dynamic ARCH Emissions

Archimedes aims to provide ARCH incentives such that the overall yields are consistent. This is ensured by their dynamic emission mechanism. Here is the schematic on how dynamic emissions works:

<img width="834" alt="Screen Shot 2023-04-03 at 11 00 59 AM" src="https://user-images.githubusercontent.com/51072084/229590281-f4002f48-8eca-49bf-b869-f2890570b782.png">

Source: [Docs - ARCH Dynamic Emission](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions)

Basically, they compare other stablecoin pools and adjust their yield accordingly to attract liquidity. This mechanism is designed to make the lvUSD pool among the top stablecoin pools with the best yields.

* Target APY: It is decided based on the data of the top 10 stablecoin pools sorted by TVL. If the lvUSD pool is in the top 5 pools then the target APY is the average of the top 5 stablecoin pools sorted by APY. Otherwise the target APY is 1.2x the average of the top 5 stablecoin pools sorted by APY. The APY and TVL data are calculated from [Defillama](https://defillama.com/yields?project=convex-finance\&attribute=stablecoins) data.
* Target TVL: TVL value is also needed to make sure the APY is not diluted when liquidity comes. The target TVL is 1.1x the 7-day moving average of the lvUSD/3CRV pool.
* Benchmark ARCH price: To compute the amount of ARCH to release, the ARCH price is calculated. The ARCH price used for computation is a 7-day moving average of ARCH price data from coingecko (If ARCH price isn’t available on Coingecko, the algorithm will use ARCH/ETH Uniswap pool data and a 7-day average of Coingecko’s ETH/USD).

Currently, the computation of dynamic emission is done manually by the team, so the strategy should be viewed as a stated intention by the team and not as an immutable process. 

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/manual%20emission.png)

Moreover, there are emission ranges (guardrails) for every quarter to ensure sensible emission rates:

> For every year, the algorithm sets a minimum and maximum emissions to avoid edge cases, i.e. what we call emission guardrails. Otherwise significant drops in ARCH price or other unexpected event might drive ARCH to overinflate.
>
> Source: [Archimedes Docs - Dynamic Emission](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions)

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/guardrails.png)

Source: [Archimedes Docs - Dynamic Emission](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions)


### Archimedes Strategy Mechanics

Leverage takers can only invest funds in predefined strategies determined by the Archimedes team. There is currently only one active strategy that leverage farms the yield-bearing OUSD stablecoin. After paying for leverage, Archimedes mints an outsized amount of lvUSD against the LTs collateral deposit (8.73 lvUSD : 1 OUSD collateral). Archimedes currently offers a fixed 9.73x leverage, although there may be more leverage options in the future. The LT's collateral, as well as the newly minted lvUSD, are swapped within the strategy to the target asset (OUSD). 

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/image.png)


Archimedes holds the leveraged OUSD position to earn yields and mints an Archimedes Position Token NFT to the LT as a receipt of this position. The NFT represents the details of their unique position, including collateral amount, borrow amount, and expiry. The LT can redeem this position anytime during normal conditions (abnormal conditions will be covered in the Risk Vectors section below). 

#### Yield Strategy (OUSD)

Strategy logic sample tx: https://etherscan.io/tx/0xd401458a98715a7b4ed490154c4f80bfea59cb4ae06bedbbf050b159fd2ad5df

Archimedes Contracts Involved:
* [Zapper](https://etherscan.io/address/0x624f570c24d61ba5bf8fbff17aa39bfc0a7b05d8) - The contract users interact with to open a position.
* [Coordinator](https://etherscan.io/address/0x58c968fada478adb995b59ba9e46e3db4d6b579d) - In charge of overall flow of creating positions and unwinding positions. lvUSD minted within the system is managed here.
* [OUSD Vault](https://etherscan.io/address/0x4c12c57c37ff008450a2597e810b51b2bba0383a) - Holds OUSD managed by Archimedes and mints vault shares to Coordinator.
* [Exchanger](https://etherscan.io/address/0x823cf8a11c1eb28b0c00011515e1d2a28b362f09) - Interacts with Curve pools.
* [Parameter Store](https://etherscan.io/address/0xcc6Ea29928A1F6bc4796464F41b29b6d2E0ee42C) - This contract contains all system parameters, including fee rates.
* [CDPosition](https://etherscan.io/address/0x229a9733063eAD8A1f769fd920eb60133fCCa3Ef) - The ledger contract for all NFT positions and regular positions. CDP creates and destroy NFT and address positions. It keep tracks of how many tokens a user has borrowed. It keeps track of how much interest each position has accrued.
* [LeverageEngine](https://etherscan.io/address/0x03dc7Fa99B986B7E6bFA195f39085425d8172E29) - Runs leverage cycles and underwrites debt to a Position Token NFT.
* [Position Token NFT](https://etherscan.io/token/0x14c6a3c8dba317b87ab71e90e264d0ea7877139d) - The NFT containing unique attributes of an open position (collateral, debt, expiry etc.)
* [lvUSD](https://etherscan.io/address/0x94A18d9FE00bab617fAD8B49b11e9F1f64Db6b36) - The synthetic stablecoin used to attain leverage.
* [Treasury Multi-sig (Protocol Fees)](https://etherscan.io/address/0x29520fd76494fd155c04fa7c5532d2b2695d68c6) - The 2-of-3 multi-sig managed by Archimedes team collects protocol fees and periodically redistributes to LPs.

To demonstrate the mechanics of the OUSD strategy, we break down this [sample tx](https://etherscan.io/tx/0xd401458a98715a7b4ed490154c4f80bfea59cb4ae06bedbbf050b159fd2ad5df) of an LT opening a new leverage position:


![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/ArchOUSDstrategy_2.png)

* 1: User deposits USDC/USDT/DAI
* 2,3: Swap leverage fee amount to ARCH.
* 4,5: Swap deposit token to OUSD.
* 6: Pay leverage fee.
* 7: OUSD is sent to the Coordinator, which is in charge of the overall flow of creating positions and unwinding positions. It keeps track of funds in the vault, updating CDP as needed and transferring lvUSD inside the system. It is controlled (and called) by the leverage engine.
* 8: OUSD is sent to OUSD Vault, which holds OUSD managed by Archimedes under all positions. It mints shares for deposited OUSD.
* 9: Performance fee is taken from the OUSD vault based on all unhandled OUSD rebases in the vault since the last contract interaction. The fee is 30% of interest earned and can be collected once a position has been closed or expires.
* 10: Mint OUSD vault share to Coordinator.
* 11: 8.73x the amount of OUSD collateral in lvUSD is sent from Coordinator to Exchanger.
* 12,13,14,15: Exchanger swaps lvUSD to OUSD via two respective Curve pools.
* 16: OUSD received is sent to Coordinator.
* 17: .1% origination fee is paid.
* 18: Amount of OUSD minus fee is sent to the OUSD vault.
* 19: OUSD vault shares are minted to the Coordinator.
* 20: Unspent ARCH sent back to the user (The fee estimate is not always exact so ARCH dust is returned to the user).
* 21: Protocol mints an [Archimedes Position Token NFT](https://etherscan.io/nft/0x14c6a3c8dba317b87ab71e90e264d0ea7877139d/33) to user that represents their position (amount of collateral, amount borrowed, expiry).

#### Closing a Position

The position has a fixed expiry, but the position holder is able to close their position before expiration. To close a position, the user calls unwindLeveragedPosition() on the LeverageEngine contract, providing the ID of their position NFT and minimum OUSD required from the operation. the strategy sells enough OUSD to repay the lvUSD debt, and returns the remaining OUSD to the user. The user may receive less than their original deposit due to fees and slippage. 

+When a user is unable to repay the loan, Archimedes has the ability to lock a user's position, which allows for the position to accrue interest and be used to repay debts in the future. This can happen when the asset proportions in OUSD/3CRV & lvUSD/3CRV pool changes out of your favor. We created this [Google Sheet](https://docs.google.com/spreadsheets/d/1AcDaQN4lNAXZKvilN1li10aU2XbegT9TfCQsiUki88E/edit?usp=sharing) to help you gauge if you position is repayable or not and if it will get locked. Moreover, in certain rare situations where the slippage for swapping OUSD for lvUSD is greater than 0.2%, Archimedes may prevent the user from exiting the position.

The swapOUSDforLvUSD() function, shown below, calculates how much OUSD is needed to output enough lvUSD for debt repayment.

<img width="1172" alt="Screen Shot 2023-04-05 at 2 26 25 PM" src="https://user-images.githubusercontent.com/51072084/230215581-4ef259e8-69b9-4fa6-b0f6-2e3b8c1cf17c.png">

Source: [Archimedes Exchanger (Etherscan)](https://etherscan.io/address/0xeade82804c67365eae67095730e70b131efb2b4e#code)

This results in a hidden fee to the user when they close a position. Because the amount needed from the swap is an estimate, and all funds swapped to lvUSD are returned to/burned by the protocol, users actually repay more lvUSD than they initially borrow. 

To illustrate this scenario, consider the following [example](https://etherscan.io/nft/0x14c6a3c8dba317b87ab71e90e264d0ea7877139d/36): The user initially borrowed 485,892 and paid back 487,965, 0.427% more lvUSD than it should be because of the estimation done by the contracts. While the position realized a yield of 9.64%, it would have earned 13.36% had this hidden fee not been incurred, resulting in a 28% reduction in profitability. You can view the relevant details of the position here for more information.

<img width="510" alt="Screen Shot 2023-04-05 at 2 51 09 PM" src="https://user-images.githubusercontent.com/51072084/230220770-ebfe8494-df73-44c1-85cd-48d48d66414a.png">

Archimedes should communicate this contract behavior to their users, and ideally, should return any lvUSD in excess of the original position debt to the user.

### Access Control

Archimedes uses a proxy pattern for all of its contracts and divides access control between the following roles:
* Admin - Can upgrade contracts, add or remove addresses from all other roles, and set dependencies (change which contracts are called)
* Governor - Can change system parameters in the parameter store contract
* Executive - Should always be a contract, used for internal contract calls within the system
* Guardian - Can pause and unpause the system, emergency backstop
* Auctioneer - Can open and close auctions from the Coordinator
* Minter - Can mint lvUSD

The [Access Control docs](https://docs.archimedesfi.com/technical-documentation/access-control-and-timelocks) describe the privileged roles that control each contract, along with the team's standards for operation of the protocol. The team appears to be making frequent updates to access controls, as contracts have been live on mainnet for some time and the team becomes confident in implementing more restrictive controls.

This disconnect between statements made in the docs and the actual access controls of deployed contracts may cause confusion. The docs claim that all admin actions (including upgrade of the contracts) are protected behind a 72-hour timelock. However, there is currently no timelock on this action or almost any other change to the protocol (with the exception that a timelock contract was [recently granted the admin role](https://etherscan.io/tx/0xe5be2e9c3c5cb4b844b054f092a77ad47e6868055f95a1bf9cdd6686e1d7dfd9#eventlog) of the lvUSD token). Furthermore, the docs claim the Guardian role, Admin role and Governor role must always be a multi-sig. In reality, the Governor is an EOA, the Zapper contract Admin is an EOA, and the Guardian is an EOA. 

<img width="704" alt="Screen Shot 2023-03-30 at 1 02 28 PM" src="https://user-images.githubusercontent.com/51072084/228952994-21b3cc4e-2003-42d1-bd30-b808c0720f1d.png">

Source: [Archimedes Access Control Docs](https://docs.archimedesfi.com/technical-documentation/access-control-and-timelocks)

While the team's intention may be to incrementally move toward the security measures outlined in their docs, their description of the protocol mechanics as they currently stand is rife with false and misleading statements. Users should take care to verify access controls within the system before participating as an LP or LT. [This Google spreadsheet](https://docs.google.com/spreadsheets/d/1GMlIJwBW6ns9eWuBmWTqZEt2NrjWzL9guzCz_8oM3uI/edit?usp=sharing) lays out the core Archimedes contracts and access control within the current implementations, as of this report's publishing.

#### Notable Access Control Findings

Importantly, the Admin role across most of the Archimedes system is this [2-of-3 multi-sig](https://app.safe.global/home?safe=eth:0x84869Ccd623BF5Fb1d18E61A21B20d50cC786744) composed of team members. It is responsible for minting lvUSD, and has the power to upgrade almost all contracts in the system without a timelock (with exception of the lvUSD contract). This is therefore a critical component of the system access control, and users should take into consideration the security of the multi-sig parameters and their trust in the Archimedes team before participating as an LP or LT.

The [Timelock contract](https://etherscan.io/address/0x01D3Aa4C9a61f5fB4b3EF5aD90C0e02ccF861842) recently granted the Admin role of the lvUSD token now requires the multi-sig to delay before changing Admin, changing Minter (also the multi-sig), or changing the mint destination. Note that there is no timelock currently for minting lvUSD to the current mint destination (Coordinator contract). The Timelock currently has a minDelay of [1.2 hours](https://etherscan.io/address/0x01D3Aa4C9a61f5fB4b3EF5aD90C0e02ccF861842#readContract#F6), so users should monitor this value that it should be increased to 72 hours as specified in the Archimedes docs.

The [Zapper](https://etherscan.io/address/0x624f570c24d61ba5bf8fbff17aa39bfc0a7b05d8#readProxyContract) contract currently has an [EOA](https://etherscan.io/address/0x68AFb79D25C9740e036b264A92d26eF95B4B9Ae7) with the Admin role, meaning someone with access to a that account can upgrade the Zapper contract without any timelock. This contract is where LTs deposit collateral into the system, so an EOA in this role puts new depositers at risk. Since Archimedes operates in leverage rounds, a malicious actor with access to the Admin private key could target these events. We recommend to change Admin to the team multi-sig for consistency within the system and to improve security assurances to new users.


## Origin Dollar OUSD Overview

#### Useful Links

* [Docs](https://docs.ousd.com/)
* Origin Dollar [Blog](https://www.ousd.com/blog)
* [Governance](https://governance.ousd.com/proposals)
* [Github](https://github.com/originprotocol/origin-dollar)
* [Audits](https://docs.ousd.com/security-and-risks/audits)
* [OUSD Analytics](https://analytics.ousd.com/)
* [Yield Analytics](https://analytics.ousd.com/apy)

Origin Dollar (OUSD) was launched in September 2020 on the Ethereum network. It is a stablecoin that natively generates yield and allows holders to passively earn interest directly to their wallets. The returns generated by the protocol are shared with OUSD holders through constant rebasing of the money supply. OUSD adjusts the money supply based on the yield generated by the protocol, enabling the price of OUSD to remain at $1 while the balances in token holders' wallets reflect real-time yields earned by the protocol.

It accepts stablecoin deposits to the OUSD smart contract and deploys them into other DeFi protocols such as Compound, Aave, and Curve to generate yields. The interest collected, trading fees, and rewards tokens are consolidated and converted to stablecoins to produce OUSD-denominated yields. The protocol moves assets in and out of different liquidity pools over time to optimize yield.

> OUSD is a stable currency that is backed 1:1 by other stablecoins like USDT, USDC and DAI. As a result, 1 OUSD should always be very close to 1 USD in value.
>
> Origin Dollar - [Docs](https://docs.ousd.com/how-it-works)

Here is the current allocation of funds for yield generation:

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/ousd%20strategies.png)

Source: [OUSD Analytics](https://analytics.ousd.com/)

The distribution of assets among the supported strategies is determined by veOGV holders through weekly snapshot voting. After the weekly poll, the results are implemented on-chain by the [Strategist multi-sig](https://etherscan.io/address/0xF14BBdf064E3F67f51cd9BD646aE3716aD938FDC) group (known as "Strategists") at their discretion and subject to their review. If over 50% of the weighted votes are in favor of the current allocation, Strategists may opt not to take any action. However, if the Strategists consider any of the allocations to be unsafe for the funds behind OUSD, they have the authority to withhold their execution. Note that Strategists do not have the authority to introduce new strategies or withdraw funds without undergoing the timelock process.


### Past Exploits

On November 7th, 2020, OUSD was [exploited for 7M USD](https://medium.com/@matthewliu/urgent-ousd-has-hacked-and-there-has-been-a-loss-of-funds-7b8c4a7d534c) due to a previously undetected reentrancy bug. Origin Dollar was relaunched in December after completing multiple audits and security upgrades. You can learn more about the steps taken to secure the protocol in their [relaunch announcement](https://medium.com/@joshfraser/origin-dollar-ousd-relaunches-to-offer-hassle-free-defi-returns-b8ee0c601dad).


## Risk Vectors - OUSD 


### Smart contract risk

OUSD is [audited ](https://docs.ousd.com/security-and-risks/audits)and also offers a bug bounty of up to $250,000 OUSD. Origin has retained [Certora](https://www.certora.com/) to formally verify the various security properties of their contracts. Certora helped Origin to establish automated verifications that will run anytime they update their contract code. Origin has automated checking for common errors with [Slither](https://github.com/crytic/slither) and [Echidna](https://github.com/crytic/echidna) tests.

However, even with formal audits, it is still possible for there to be logic errors that could lead to the loss of funds for OUSD holders.


### Custody Risk

There is a primary admin contract, a [5-of-8 multi-sig](https://etherscan.io/address/0xbe2AB3d3d8F6a32b96414ebbd865dBD276d3d899) which is required to make any code changes to the protocol. OUSD can only be upgraded from this 5-of-8 multi-sig wallet. The keys to this multi-sig are held by individuals with close ties to the company, and not even the Origin founders acting together have enough control to execute owner functions on their own. In addition, the OUSD contracts are owned by a [timelock](https://etherscan.io/address/0x72426BA137DEC62657306b12B1E869d43FeC6eC7) which places a 48-hour time delay before any changes to the protocol can be made.

Time-delayed admin actions give users a chance to exit OUSD if its admins become malicious, are compromised, or make a change that the users do not like.

Some functionality, such as rebalancing funds between strategies or pausing deposits, can be triggered without the timelock and with far fewer signers. This allows the Origin team to react more quickly to market conditions or security threats. These signers, known as Strategists, have the ability to execute a limited number of functions with only [2-of-9 signers](https://etherscan.io/address/0xF14BBdf064E3F67f51cd9BD646aE3716aD938FDC). The strategist multi-sig can do the following actions on the vault:

* reallocate - move funds between strategies
* setVaultBuffer - adjust the amount of funds held outside strategies for cheaper redeems.
* setAssetDefaultStrategy - which strategy mints and redeems pull from for a particular strategy
* withdrawAllFromStrategy - remove funds from a single strategy and send them to the vault
* withdrawAllFromStrategies - remove funds from all active strategies and send them to the vault
* pauseRebase - pause all rebases
* pauseCapital - pause all mints and redeems
* unpauseCapital - allow all mints and redeems


### Depeg Risk

Any reduction in the value of the underlying stablecoins will have a corresponding impact on the value of OUSD. Although OUSD aims to maintain a 1:1 relationship between its supply and the quantity of underlying stablecoins, there is no guarantee regarding which stablecoins will form this backing, nor their value. Additionally, each supported stablecoin introduces a level of counter-party risk that should not be underestimated.

> OUSD rebasing will only increase supply since the amount of OUSD minted is tied to the realized gains earned by the underlying strategies. Your principal is protected as long as nothing goes wrong with the underlying lending/AMM and stablecoin protocols. Your OUSD balance will never decrease, but the value could drop if there's a failure in the underlying systems.
>
> Source: [OUSD Docs](https://docs.ousd.com/core-concepts/elastic-supply)

OUSD is built on top of other DeFi platforms like Aave, Compound, and Curve which add additional smart contract risk. There are no guarantees that the underlying third-party platforms will continue to work as intended, and any failure in an underlying strategy would potentially lead to a loss of funds for OUSD holders.

The design of OUSD bears resemblance to another stablecoin recently affected by an exploit in its underlying strategy: Angle Protocol's agEUR. It similarly deployed agEUR collateral backing into various DeFi protocols, and had funds in Euler when the protocol was [exploited for $197M](https://rekt.news/euler-rekt/). This [forced Angle to pause the protocol and agEUR experienced a prolonged depeg](https://twitter.com/LlamaRisk/status/1636460843381723136). Fortunately, the attacker has returned most funds, so users will likely be made whole. It does serve as a reminder of the dangers involved with composable yield-farming strategies, so users should always be aware of their risk exposure in the underlying strategies. 


## Risk Vectors - Archimedes

### Smart Contract Risk

For Liquidity Providers (lenders), the relevant smart contracts are:

* Curve smart contracts
* Uniswap smart contracts
* OUSD smart contracts
* Archimedes’ smart contracts

The Archimedes protocol smart contracts are audited by [Halborn Security](https://halborn.com/) to guarantee the quality and security of every change the team makes to its smart contracts. You can view the [Audits here.](https://docs.archimedesfi.com/audit-and-code-bounty/audits)

Three audits were conducted: (1) the general system, (2) the auctions, and (3) the Zapper contract, between November 2022 and January 2023. Notable findings were one critical issue in the Auction system that would allow a user to acquire leverage at the minimum auction price on a subsequent leverage round (SOLVED). Several medium and low issues were not resolved, including risk of frontrunning the auction by checking bids in the mempool, and insufficient parameter precision and inconsistent parameter formatting in the ParameterStore contract.


### Depeg Risk

The only utility of lvUSD for borrowers is to be utilized by Archimedes to facilitate leveraged position for the borrowers. The only utility for lenders is to access ARCH incentives. lvUSD can break its peg to 3CRV in the following situations:

1. Uncapped leverage distribution.
2. Unintended withdrawals of 3CRV by LPs
3. Assets in the underlying strategy (OUSD) lose their dollar peg.

Since all the decisions regarding the leverage rounds are at the team's discretion, lvUSD depeg due to uncapped leverage distribution relies on how actively the leverage distribution is managed. A significant component of peg management is trust in the team to responsibly conduct leverage rounds such that the Curve pool reamins reasonably balanced.

Yields can incentivize against unintended withdrawals of 3CRV, but there is also no guarantee that the team's dynamic emissions strategy will consistently produce the desired outcome. A decrease in the price of ARCH or alternative yield opportunities may affect the desirability of supplying 3CRV liquidity.

It is also possible that lvUSD depegs as a result of a depeg in the underlying strategy (OUSD), as liquidity providers would likely pull their capital out of the pool on the news of OUSD depeg.


### Collateral Risk / Underlying Assets Risks

If an underlying asset (i.e. OUSD/DAI/USDC/USDT) loses its peg and causes the position to be insolvent, Archimedes does not currently have a liquidation process (although the team has plans for one in the future). Instead, it will lock the user's position until the value is sufficient to repay the borrowed capital _(either by OUSD regaining its peg or by the position earning more yield over time)._ 

![photo_2023-03-31 11 13 37](https://user-images.githubusercontent.com/51072084/229198181-e8cc5f48-42d5-4716-9d4d-963c3f334012.jpeg)

If the underlying asset experiences a depeg that does not recover, insolvent positions may become permanently locked in Archimedes. It is still possible for the leverage taker to sell their Position Token NFT on the open market through OpenSea, although in such a situation the position is likely worth near 0.

If the OUSD peg drops below $1, lvUSD would likely also depeg since OUSD is the only strategy used by Archimedes. Assuming both stables depeg simultaneously, leverage takers may remain solvent and not have their position locked (loss from OUSD->3CRV swap is offset by gain from 3CRV->lvUSD swap). Use this [Google Sheet](https://docs.google.com/spreadsheets/d/1AcDaQN4lNAXZKvilN1li10aU2XbegT9TfCQsiUki88E/edit?usp=sharing) to help determine situations where an LT would have their position locked as a result of a depeg in the underlying strategy. 

Both the liquidity provider and the leverage taker may experience a loss, but as the liquidity provider's funds were used to provide leverage, they will be impacted the most. For example, say an LP supplys 1m 3CRV to the lvUSD pool. An LT borrows the 3CRV, converting funds to OUSD. OUSD and lvUSD both depeg to $.50, so the LT is still able to close their position. The LT's loss is offset by the depeg in both OUSD and lvUSD, but only 500k 3CRV is returned to the lvUSD pool. It is the 3CRV liquidity provider, in this case, who bears the loss.


### Slippage and Fees (Economic Risk)

LPs should be aware that entering and exiting the liquidity pool with single-sided liquidity can incur a loss from slippage and fees. Moreover, the pool proportion at the time of entry and exit would impact the realized slippage. 

This [calculator](https://calculator.archimedesfi.com/) can help users determine the expected APY from their position. Bear in mind the leverage fee and origination fee when opening a position, as well as swap fees in the underlying strategy, will require the position to remain open for some length of time before becoming profitable. Users should consider the opportunity cost of opening a position vs. supplying liquidity.

Archimedes can control the pool proportion by providing adequate lvUSD leverage. They intend to keep the pool proportion near 50% lvUSD when computing leverage rounds.

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/pool%20limit%20for%20leverage.png)


#### Inflation

With dynamic emissions, Archimedes wants their Curve pool TVL to grow continuously. They are providing emission rewards to the LPs in ARCH tokens for this intended outcome.

So far, the leverage available per ARCH token is cheaper than the ARCH emitted to sustain the liquidity in the lvUSD/3CRV pool. In other words, the absorption of ARCH tokens (in the form of leverage fees) is lower than that of the emission provided. This creates inflation in the ARCH supply and with greater spread between the absorption and emission, the price of the ARCH token may drop.

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/dune1.png)

Source:  [Dune Query](https://dune.com/queries/2238660)

There is the potential for a chain of events that would result in a price decline for ARCH tokens.

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/loop%20archimedes.png)

When there is less leverage available, the inflation of ARCH supply will increase. This is possible when the pool ratio is heavily shifted towards lvUSD, making leverage rounds unviable. At this time, the protocol relies on existing leverage takers to close their position in order to provide further leverage to new leverage takers.

This again poses a risk to the LP because the gains made by the leverage taker when they close their position to repay the debt at a discount may be at the expense of the LP in terms of slippage.


## LlamaRisk Gauge Criteria

Centralization Factors

1.  Is it possible for a single entity to rug its users?

Yes, the team controls access to lvUSD through periodic leverage rounds and control many system functions. There are critical roles controlled by the team's 2-of-3 multi-sig with no timelock (e.g. upgrade contracts) and by team-controlled EOA accounts without timelock (e.g. Guardian role can globally pause the system, Governor role can change system parameters). 

2.  If the team vanishes, can the project continue?

No, the system depends on manual action taken by the team (opening new leverage rounds) and the team makes the decision off-chain for many moving parts of the protocol.

Economic Factors

1.  Does the project's viability depend on additional incentives?

The protocol refrains from using CRV rewards as of the publishing of this report. Incentives in the form of their native ARCH token is a fundamental component of the system design to incentivize LPs.
    
2.  If demand falls to 0 tomorrow, can all users be made whole?

Depends. If the underlying assets (3CRV) are stable and pegged, and the underlying strategies are performing as intended (OUSD has not lost peg), users can be made whole. In ordinary circumstances, reduced demand for lvUSD incentivizes borrowers to repay their debt at a discount.

Security Factors

1. Do audits reveal any concerning signs?

Several [audits](https://docs.archimedesfi.com/audit-and-code-bounty/audits) have been performed between November 2022 and January 2023 by [Halborn Security](https://www.halborn.com/). There was one critical vulnerability found in the Auction system, which has been resolved. Several medium to low issues were identified that have not been resolved, but overall audits appear thorough and do not reveal any concerning signs. 


## Risk Team Recommendation

Archimedes is a young protocol that is in the process of incrementally refining its system security. At this time, it should be regarded as a project in beta and used with caution. Their team has not made any proposal to receive CRV emissions, but there should be some improvements made before any consideration is made to submit a proposal:
* The Archimedes team should transfer all access controls to their multi-sig (including Zapper Admin, Guardian, and Governor) and implement a timelock on all critical functions- most importantly to put contract upgrades (Admin role) behind a timelock. 
* Archimedes should strive to reduce dependence on team actions by automating leverage rounds and fee/emissions distribution, and introducing token governance.
* The Archimedes docs should be improved to accurately and transparently reflect the functionality of the protocol (access controls, disclosure of all protocol fees, explanation of the leverage farming strategy).

Their team has informed us that a v2 is planned that will introduce a liquidation mechanism. We look forward to such features which serve to improve the resilience of the protocol and offer greater assurances to its users. 

