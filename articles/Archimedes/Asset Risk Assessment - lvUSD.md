# Asset Risk Assessment - lvUSD (Archimedes)

#### (One sentence summary of the report)

### Index

\[WIP]


### Relation to Curve

Archimedes has a [lvUSD/3CRV](https://curve.fi/#/ethereum/pools/factory-v2-268/deposit) pool where LPs can provide liquidity in 3CRV to earn ARCH incentives provided by Archimedes through the Curve gauge. As of publishing time, Archimedes has not approached the Curve community for CRV rewards allocation. In this report we investigate the leverage farming strategy employed by Archimedes and risks posed to holders of lvUSD and Curve LPs.


## Archimedes Overview

#### Useful Links

* [Whitepaper (v2)](https://docs.archimedesfi.com/archimedes-finance-whitepaper-\(v2\))
* [Docs](https://docs.archimedesfi.com/)
* Archimedes [Blog](https://medium.com/archimedes-finance)
* [Github](https://github.com/thisisarchimedes/Archimedes\_Finance)
* Important [contracts](https://docs.archimedesfi.com/technical-documentation/contract-addresses)
* lvUSD/3CRV [Curve pool](https://curve.fi/#/ethereum/pools/factory-v2-268/deposit)
* Audit Reports:
  * [Smart Contract -Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Nov 2022
  * [Auctions - Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Auctions\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Dec 2022
  * [Zapper - Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Zapper\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Jan 2023

Archimedes is a leveraged yield-farming DeFi protocol. The core of the strategy involves the leveraged USD (lvUSD) stablecoin and the Curve lvUSD/3CRV stableswap pool. The protocol periodically conducts sales of newly minted lvUSD called "leverage rounds" (pending favorable token balance in the Curve pool), which allows borrowers to amplify yield farming returns up to 9.73x against a stablecoin collateral deposit. Currently, the underlying strategy relies on yield from OUSD (a yield-bearing stablecoin), but strategies may change or expand to other yield-bearing tokens in the future.


### Product-Market Fit Exploration

Archimedes believes liquidity providers on Curve generally have a suboptimal experience because CRV emissions make up the vast majority of APY and consequently LPs tend to experience high volatility in pool returns as many pools compete for a fixed amount of emissions. 

> We believe that CRV emissions are capping Curve's ability to scale. And we all want to see a stablecoin Curve pool that supports large investments without materially impacting the pool’s APY. Ideally, this pool provides relatively good yield, generated from real economic activity.
>
> Source: [Archimedes Docs - Motivation](https://docs.archimedesfi.com/tokenomics-and-ecosystem/motivation)

Fluctuating APYs between pools requires users to switch pools to get consistently better yields. As network fees continue to increase, the game has become dominated by whales to the detriment of smaller LPs.


#### Solution

Archimedes has built a strategy that makes use of Curve's stableswap pool, in conjunction with its own dynamic incentive mechanism, to attempt to offer consistently superior returns to its LPS. The startegy involves protocol-owned liquidity, which gives Archimedes control of the pool's behavior, allowing it to offer its customers (leverage takers, or LTs) access to attractive yield strategies.

Archimedes offers yield to the liquidity providers in its native ARCH token and LTs (borrowers) must pay a fee (in ARCH tokens) when opening a leverage position. Demand to open a leveraged yield farming position thus produces a buy pressure on ARCH tokens. ARCH fees are then redistributed to LPs to incentivize deep liquidity and enable to protocol to mint greater amounts of lvUSD. 

ARCH thus serves as a utility token within the protocol that is required to access leverage. The ARCH yield provided by the protocol is [dynamically adjusted](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions) such that pool yields remain competitive against a subset of high TVL stable coin pools on Curve finance.



### Leverage Rounds

New [lvUSD](https://etherscan.io/address/0x94A18d9FE00bab617fAD8B49b11e9F1f64Db6b36) comes into circulation in batches minted by the protocol during leverage rounds. Archimedes periodically conducts [leverage rounds](https://docs.archimedesfi.com/taking-leverage-(borrower)/leverage-rounds) where it makes a limited supply of new lvUSD available for LTs to purchase leverage. The rounds do not have a fixed schedule, but the date and allocation of upcoming rounds are advertized on various Archimedes social channels (Twitter, Discord, Telegram etc.) prior to each round.

A leverage round is comprised of the following parameters:

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/ttributes.PNG)

(Note that Archimedes usually provides leverage with a position lifetime of 370 days.)

The protocol relies heavily on the 3CRV liquidity in the lvUSD/3CRV pool. A leverage round can only occur if the Curve pool is reasonably balanced, and the amount of lvUSD made available to LTs is dependent on liquidity in the pool. To ensure this liquidity, Archimedes uses a dynamic emission mechanism to consistently provide competitive yields. This dynamic emission mechanic checks TVL and yields of other stablecoin pools, and takes yield dilution of the lvUSD/3CRV pool into consideration.

During an active round, LTs can bid for the available leverage using their ARCH token. The protocol uses a Dutch Auction strategy by beginning the round with a high fee and reducing it until all lvUSD has been acquired. The ARCH leverage fee is an upfront payment to the Archimedes treasury which will eventually flow to LPs. In addition to the dynamic leverage fee (paid in ARCH), there is a small origination fee paid in the collateral token and a 30% performance fee on the yield generated by the strategy.

The borrower provides collateral in one of the counterparty assets of the Curve pool (USDT, USDC, or DAI). This collateral serves as an input asset in the predefined strategy (e.g. Archimedes uses an OUSD strategy that uses the OUSD token as collateral).


### Protocol Participant: Leverage Takers

For a borrower looking for leverage, they must participate in one of the leverage rounds conducted by Archimedes. The ARCH fee to buy leverage will be deducted from the provided collateral itself.

The amount of leverage available per ARCH token is determined through a dutch auction, which starts with a low price and gradually increases until the desired amount of leverage is reached or there is no more leverage available. This auction is called the Archimedes Leverage Round.


#### Leverage Farming Fees

There are 3 types of fees associated with the Archimedes Protocol and will be transferred to the protocol treasury:

* Leverage Fee
* Origination Fee
* Performance Fee

All fees are collected and sent to the Archimedes Treasury to later be used to incentivize and reward the Liquidity Providers (Lenders) of the 3CRV/lvUSD Curve Pool.&#x20;

* Leverage Fee (ARCH Token) - When a user Opens a Position they pay some ARCH tokens as an upfront fee to access the leverage (lvUSD). It is not required for a user to hold ARCH tokens as Archimedes will take the fee from the funds provided as principal.
* Origination Fee (Collateral Asset) - When a user Opens a Position there is a one-time 0.5% origination fee applied. This 0.5% amount is applied to the leverage amount (borrowed amount) and is collected from the collateral asset (OUSD). The origination fee is 0.1% at the time of writing the report.&#x20;
* Performance Fee (Collateral Asset) - The Performance Fee is 30% and it applies only to the Interest Earned from the full position (interest earned from Collateral and Leverage). This fee is deducted from the interest earned and is taken only when the position is closed or when it expires.
* Open & Close Exchange Fees - 1% of the collateral is charged as Open & Close Exchange Fee

These fees can be tracked on the Archimedes [calculator page](https://calculator.archimedesfi.com/?levPrice=0\&originFee=NaN).


#### Price stability of lvUSD

Every time a borrower takes leverage, lvUSD is minted and swapped for 3CRV. Thus the lvUSD quantity in the lvUSD/3CRV increases, pushing the price lower. The price of lvUSD w.r.t 3CRV affects the realized loss/gain for borrowers at the time of repayment. lvUSD price also matters to liquidity providers as it directly impacts the slippage on their trade while depositing or withdrawing their liquidity in 3CRV.

Archimedes controls the amount and frequency of leverage given to the borrowers. This ensures the price of lvUSD stays near the $1 peg.

* Leverage is capped: Archimedes opens a new auction only when there is enough liquidity in the pool. Each auction is for a limited amount of leverage and is designed to maintain the balance of the pool.
* lvUSD >> $1: If lvUSD rises above $1, Archimedes will raise the leverage cap. More lvUSD will be borrowed and enter circulation, returning the lvUSD peg.
* lvUSD << $1: If lvUSD drops below $1, LTs have an arbitrage opportunity by unwinding their position. LTs will pay back $1 worth of debt for less than $1, pocketing the instant arbitrage profits. This process burns lvUSD and increases the amount of 3CRV in the pool.


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

* Target APY: It is decided based on the data of the top 10 stablecoin pools sorted by TVL. If the lvUSD pool is in the top 5 pools then the target APY is the average of the top 5 stablecoin pools sorted by APY. Otherwise the target APY is 1.2x the average of the top 5 stablecoin pools sorted by APY. The APY and TVL data are fetched from Defillama (_\* “Target APY” is calculated based on_ [_Defillama_](https://defillama.com/yields?project=convex-finance\&attribute=stablecoins) _data_).
* Target TVL: TVL value is also needed to make sure the APY is not diluted when liquidity comes. The target TVL is 1.1x the 7-day moving average of the lvUSD/3CRV pool.
* Benchmark ARCH price: To compute the amount of ARCH to release, the ARCH price is calculated. The ARCH price used for computation is a 7-day moving average of ARCH price data from coingecko (\*\* _If ARCH price isn’t available on Coingecko, the algorithm will use ARCH/ETH Uniswap pool data and Coingecko’s ETH/USD (taking the last 7 days average ETH/USD price)_).

Currently, the computation of dynamic emission is done manually by the team, so the strategy should be viewed as a stated intention by the team and not as an immutable process. 

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/manual%20emission.png)

Moreover, there are emission ranges (guardrails) for every quarter to ensure the adequate emission

> For every year, the algorithm sets a minimum and maximum emissions to avoid edge cases, i.e. what we call emission guardrails. Otherwise significant drops in ARCH price or other unexpected event might drive ARCH to overinflate.
>
> Source: [Archimedes Docs - Dynamic Emission](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions)

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/guardrails.png)

Source: [Archimedes Docs - Dynamic Emission](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions)


### Protocol Participant: Liquidity Provider

As a liquidity provider, a user can simply deposit 3CRV in the lvUSD pool and will receive ARCH incentives on top of swap fees generated on the pool.

Archimedes aims to provide ARCH incentives such that the overall yields are consistent. This is ensured by their dynamic emission mechanism. Here is the schematic on how dynamic emissions works:

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/dynamic%20emissions.png)

Source: [Docs - ARCH Dynamic Emission](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions)

Basically, they compare other stablecoin pools and adjust their yield accordingly to attract liquidity. This mechanism is designed to make the lvUSD pool among the top stablecoin pools with the best yields.


#### Incentives for Liquidity providers

Liquidity providers receive ARCH incentives from Archimedes and swap fees from Curve.

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/discord1.PNG)

ARCH incentives are redistributed to LPs from the Archimedes treasury multisig. This address collects other fees (origination and performance fees) denominated in OUSD, which are currently not distributed. The [Archimedes docs](https://docs.archimedesfi.com/taking-leverage-(borrower)/taking-leverage-explained) erroneously claims all fees are used to incentivize LPs, and while that may be a future intention, it is the case today that only ARCH fees are redistributed.


### Archimedes Strategy Mechanics

Leverage takers can only invest funds in predefined strategies decided by Archimedes. There is currently only one active strategy that leverage farms the yield-bearing OUSD stablecoin. After paying for leverage, Archimedes mints an outsized amount of lvUSD (no. of lvUSD > no. of OUSD). (Archimedes now offers a fixed 9.73x leverage, although there may be more leverage options in the future.) The LT's collateral, as well as the newly minted lvUSD, are swapped within the strategy to the target asset (OUSD). 

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/image.png)


Archimedes holds the leveraged OUSD position to earn yields and provides an NFT to the LT as a receipt of this position. The Archimedes Position Token NFT represents the details of their unique position, including collateral amount, borrow amount, and expiry. The LT can redeem this position anytime during normal conditions (abnormal conditions will be covered in the Risk Vectors section below). 


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
* [Treasury Multisig (Protocol Fees)](https://etherscan.io/address/0x29520fd76494fd155c04fa7c5532d2b2695d68c6) - The 2-of-3 multisig managed by Archimedes team collects protocol fees and periodically redistributes to LPs.

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
* the 21: Protocol mints an [Archimedes Position Token NFT](https://etherscan.io/nft/0x14c6a3c8dba317b87ab71e90e264d0ea7877139d/33) to user that represents their position (amount of collateral, amount borrowed, expiry).

### Access Control

Archimedes uses a proxy pattern for all of its contracts and divides access control between the following roles:
* Admin - Can upgrade contracts, add or remove addresses from all other roles, and set dependencies (change which contracts are called)
* Governor - Can change system parameters in the parameter store contract
* Executive - Should always be a contract, used for internal contract calls within the system
* Guardian - Can pause and unpause the system, emergency backstop
* Auctioneer - Can open and close auctions from the Coordinator
* Minter - Can mint lvUSD

The [Access Control docs](https://docs.archimedesfi.com/technical-documentation/access-control-and-timelocks) describe the privileged roles given to each contract along with the team's standards for operation of the protocol. This is certainly the early stage of the project, and the team appears to be making frequent updates to access controls as they become confident the system will perform with more restrictive controls.

This disconnect has resulted in many false statments made in the docs. They claim all admin actions (including upgrade of the contracts) are protected behind a 72 hour timelock, but there is currently no timelock on this action or almost any other change to the protocol (with the exception that a timelock contract was [recently granted the admin role](https://etherscan.io/tx/0xe5be2e9c3c5cb4b844b054f092a77ad47e6868055f95a1bf9cdd6686e1d7dfd9#eventlog) of the lvUSD token). Furthermore, the docs claim the Guardian role, Admin role and Governor role must always be a multisig. In reality, the Governor is an EOA, the Zapper contract Admin is an EOA, and the Guardian is an EOA. 

<img width="704" alt="Screen Shot 2023-03-30 at 1 02 28 PM" src="https://user-images.githubusercontent.com/51072084/228952994-21b3cc4e-2003-42d1-bd30-b808c0720f1d.png">

Source: [Archimedes Access Control Docs](https://docs.archimedesfi.com/technical-documentation/access-control-and-timelocks)

While the team's intention may be to incrementally move toward the security measures outlined in their docs, their description of the protocol mechanics as they currently stand is rife with false and misleading statements. Users should take care to verify access controls within the system before participating as an LP or LT. [This Google spreadsheet](https://docs.google.com/spreadsheets/d/1GMlIJwBW6ns9eWuBmWTqZEt2NrjWzL9guzCz_8oM3uI/edit?usp=sharing) lays out the core Archimedes contracts, and access control within of the current implementations as of this report's publishing.

Importantly, the Admin role across most of the Archimedes system is this [2-of-3 multisig](https://app.safe.global/home?safe=eth:0x84869Ccd623BF5Fb1d18E61A21B20d50cC786744) composed of team members. It is responsible for minting lvUSD, and has the power to upgrade almost all contracts in the system without a timelock (with exception of the lvUSD contract). This is therefore a critical component of the system access control, and users should take into consideration the security of the multisig parameters and their trust in the Archimedes team before participating as an LP or LT.

The [Timelock contract](https://etherscan.io/address/0x01D3Aa4C9a61f5fB4b3EF5aD90C0e02ccF861842) recently granted the Admin role of the lvUSD token now requires the multisig to delay before changing Admin, changing Minter (also the multisig), or changing the mint destination. Note that there is no timelock currently for minting lvUSD to the current authorized destination (Coordinator contract). The Timelock currently has a minDelay of [1.2 hours](https://etherscan.io/address/0x01D3Aa4C9a61f5fB4b3EF5aD90C0e02ccF861842#readContract#F6), so users should monitor this value that it should be increased to 72 hours as specified in the Archimedes docs.


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

The distribution of assets among the supported strategies is determined by veOGV holders through weekly snapshot voting. After the weekly poll, the results are implemented on-chain by the Strategist multi-sig group (known as "Strategists") at their discretion and subject to their review. If over 50% of the weighted votes are in favor of the current allocation, Strategists may opt not to take any action. However, if the Strategists consider any of the allocations to be unsafe for the funds behind OUSD, they have the authority to withhold their execution. Note that Strategists do not have the authority to introduce new strategies or withdraw funds without undergoing the timelock process.


### Past Exploits

On November 7th, 2020, OUSD was [exploited for 7M USD](https://medium.com/@matthewliu/urgent-ousd-has-hacked-and-there-has-been-a-loss-of-funds-7b8c4a7d534c) due to a previously undetected reentrancy bug. Origin Dollar was relaunched in December after completing multiple audits and security upgrades. You can learn more about the steps taken to secure the protocol in their [relaunch announcement](https://medium.com/@joshfraser/origin-dollar-ousd-relaunches-to-offer-hassle-free-defi-returns-b8ee0c601dad).


## Risk Vectors - OUSD Strategy


### **Smart contract risk**

OUSD is [audited ](https://docs.ousd.com/security-and-risks/audits)and also offers a bug bounty of up to $250,000 OUSD. Origin has retained [Certora](https://www.certora.com/) to formally verify the various security properties of their contracts. Certora helped Origin to establish automated verifications that will run anytime they update their contract code. Origin has automated checking for common errors with [Slither](https://github.com/crytic/slither) and [Echidna](https://github.com/crytic/echidna) tests.

However, even with formal audits, it is still possible for there to be logic errors that could lead to the loss of funds for OUSD holders.


### Custody Risk

There is a primary admin contract, a [5-of-8 multisig](https://etherscan.io/address/0xbe2AB3d3d8F6a32b96414ebbd865dBD276d3d899) which is required to make any code changes to the protocol. OUSD can only be upgraded from this 5 of 8 multi-sig wallet. The keys to this multi-sig are held by individuals with close ties to the company, and not even the Origin founders acting together have enough control to execute owner functions on their own. In addition, the OUSD contracts are owned by a [timelock](https://etherscan.io/address/0x72426BA137DEC62657306b12B1E869d43FeC6eC7) which places a 48-hour time delay before any changes to the protocol can be made.&#x20;

Time-delayed admin actions give users a chance to exit OUSD if its admins become malicious, are compromised, or make a change that the users do not like.

Some functionality, such as rebalancing funds between strategies or pausing deposits, can be triggered without the timelock and with far fewer signers. This allows the Origin team to react more quickly to market conditions or security threats. These signers, known as Strategists,  have the ability to execute a limited number of functions with only [2-of-9 signers](https://etherscan.io/address/0xF14BBdf064E3F67f51cd9BD646aE3716aD938FDC). The strategist multisig can do the following actions on the vault:

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

The design of OUSD bears resemblance to another stablecoin recently affected by an exploit in its underlying strategy: Angle Protocol's agEUR. It similarly deployed agEUR collateral backing into various DeFi protocols, and had funds in Euler when the protocol was [exploited for $197M](https://rekt.news/euler-rekt/). This [forced Angle to pause the protocol and agEUR experienced a prolonged depeg](https://twitter.com/LlamaRisk/status/1636460843381723136). Fortunately, the attacker has returned most funds, so users will likely be made whole. It does serve as a reminder of the dangers involved with aggressive yield-farming strategies, so users should always be aware of their risk exposure in the underlying strategies. 


## Risk Vectors - Archimedes

### Smart Contract Risk

For Liquidity Providers (lenders), the relevant smart contracts are:

* Curve smart contracts
* Uniswap smart contracts
* OUSD smart contracts
* Archimedes’ smart contracts

The Archimedes protocol smart contracts are audited by [Halborn Security](https://halborn.com/) to guarantee the quality and security of every change the team makes to its smart contracts. You can view the [Audits here.](https://docs.archimedesfi.com/audit-and-code-bounty/audits)

Three audits were done on the general system, the auctions, and the Zapper contract between November 2022 and January 2023. Notable findings were one critical issue in the Auction system that would allow a user to acquire leverage at the minimum auction price on a subsequent leverage round (SOLVED). Several medium and low issues were not resolved, including risk of frontrunning the auction by checking bids in the mempool, and insufficient parameter precision and inconsistent parameter formatting in the ParameterStore contract.


### Depeg Risk

The only utility of lvUSD for users is to be utilized by Archimedes to facilitate leveraged position for the borrowers. lvUSD can break its peg to 3CRV in the following situations:

1. Uncapped leverage distribution.
2. Unintended withdrawals of 3CRV by LPs
3. If assets under 3CRV lose their dollar peg.

Since all the decisions regarding the leverage rounds are at the team's discretion, lvUSD depeg due to uncapped leverage distribution relies on how actively the leverage distribution is managed. A significant component of peg management is trust in the team to responsibly conduct leverage rounds such that the Curve pool reamins reasonably balanced.

Yields can incentivize against unintended withdrawals of 3CRV, but there is also no guarantee that the team's dynamic emissions strategy will consistently produce the desired outcome. A decrease in the price of ARCH or alternative yield opportunities may affect the desirability of supplying 3CRV liquidity.

It is also possible that lvUSD depegs as a result of a depeg in the underlying strategy (OUSD). This would be caused by liquidity providers pulling their capital out of the pool on the news of OUSD depeg.


### Collateral Risk / Underlying Assets Risks

If an underlying asset (an asset that backs lvUSD i.e. OUSD or DAI, USDC, USDT) loses its peg, causing the position to be insolvent, Archimedes does not currently have a liquidation process (although the team has plans for one in the future). Instead, it would lock the user's position until the value is sufficient to repay the borrowed capital _(either by OUSD regaining its peg or by the position earning more yield over time)._ 

![photo_2023-03-31 11 13 37](https://user-images.githubusercontent.com/51072084/229198181-e8cc5f48-42d5-4716-9d4d-963c3f334012.jpeg)

If the underlying asset experiences a depeg that does not recover, insolvent positions may become permanently locked in Archimedes. It is still possible for the leverage taker to sell their Position Token NFT on the open market through OpenSea, although in such a situation the position is likely worth near 0.

If the OUSD peg drops below $1, the value of lvUSD would also go down, since currently, OUSD is the only strategy used by Archimedes. In the case of OUSD depeg, leverage takers would be able to withdraw funds without worrying about Archimedes locking their position. This is possible as all the lvUSD is backed by the OUSD strategy and if OUSD and lvUSD depegs to a similar price or lower, the loan repayment would not require any extra funds. The effect of asset depeg will be seen while swapping from OUSD to 3CRV and while swapping from lvUSD to 3CRV. Here both, the liquidity prover, as well as leverage taker, would suffer a loss but as the liquidity provider's funds were used to provide leverage, they will be impacted the most.


### Risk exposure due to economic activities

#### Slippage

LPs should be aware that entering and exiting the liquidity pool with single-sided liquidity would incur slippage. Moreover, the pool proportion at the time of entry and exit would impact the resulting slippage.

Archimedes can control the pool proportion by providing adequate lvUSD leverage. They intend to keep the pool proportion near 50% for lvUSD at the time of computing leverage rounds.

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/pool%20limit%20for%20leverage.png)

**Inflation**

* [ ] _<mark style="color:blue;">Inflation- The dynamic emissions scheme is kind of fishy, should also look into that more</mark>_

With dynamic emissions, Archimedes wants the TVL to grow constantly. They are providing emission rewards to the LPs in ARCH tokens for this intended outcome.&#x20;

So far, the leverage available per ARCH token is cheaper than the ARCH emitted to sustain the liquidity in the lvUSD/3CRV pool. In other words, the absorption of ARCH tokens (in the form of leverage fees) is lower than that of the emission provided. This creates inflation in the ARCH price and with more and more spread between the absorption and emission, the price of the ARCH token would drop.&#x20;

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/dune1.png)

Source:  [Dune Query](https://dune.com/queries/2238660)

There are events that can lead to a subsequent chain of events that would result in a price decline for ARCH tokens.&#x20;

![](https://github.com/DiligentDeer/Assets/blob/main/lvUSD/spiral.png)

When the availability of leverage is less, the inflation on ARCH price will rise. This is possible when the pool ratio is heavily shifted towards lvUSD. At this time protocol will rely on the borrowers to close their position in order to provide further leverage to new leverage takers.

This again poses a risk to the LP because the gains made by the leverage taker when they close their position to repay the debt at a discount will be suffered by the LP in terms of slippage.

As discussed earlier, inflation is already a catalyst in the chain of these subsequent events.

## LlamaRisk Gauge Criteria

Centralization Factors

1.  Is it possible for a single entity to rug its users?

    Yes, because the protocol controls and owns all the funds invested in the yield strategies.

2.  If the team vanishes, can the project continue?

    No, because there are many mechanisms that are not active yet and the team makes the decision off-chain for many moving parts of the protocol.

Economic Factors

1.  Does the project's viability depend on additional incentives?

    The protocol refrains from using CRV rewards based on their communication in the protocol docs as of writing the report.
2.  If demand falls to 0 tomorrow, can all users be made whole?

    Depends. If the underlying assets (3CRV) are stable and pegged, subsequently if the assets associated with the yield-earning strategies are performing as intended, users can be made whole.

Security Factors

1. Do audits reveal any concerning signs?

\#Risk Team Recommendation (Don't worry about this section in the first draft, we will discuss together and with the protocol team to determine our final recommendation)

## Appendix

<mark style="color:blue;">leverage APY and profit calc: https://docs.google.com/spreadsheets/d/132CKzt68FhRl2UF43SxYtdyf08xr7Rj7do\_6Mi8DbWU/edit#gid=1117646091</mark>


save for later:
--------------
> Underlying asset losses peg: lvUSD is over collateralized by the underlying assets. In case the underlying asset loses its peg (<< $1), Archimedes automatically closes the LTs’ positions (no penalty to users - this isn’t liquidation, this is closing the position, returning the debt and making the remaining profit available to the LT). All lvUSD debt is paid back and the borrowed 3CRV returns to the Curve pool.
>
> Source: [Archimedes Whitepaper](https://docs.archimedesfi.com/archimedes-finance-whitepaper-\(v2\)#0d5b93c7b9844a5090c31c1cf190a4ae)

<mark style="color:red;">Archimedes also talks about the case when an underlying asset (OUSD or any asset under 3CRV) loses its peg. This case is discussed later in the report.</mark>
