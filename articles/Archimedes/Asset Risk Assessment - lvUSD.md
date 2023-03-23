# Asset Risk Assessment - lvUSD (Archimedes)

#### (One sentence summary of the report)

### Index

* Context (Relation to Curve Finance)
* Archimedes Overview
  * Liquidity providers
  * Leverage taker (Borrowers)
* Origin Dollar (OUSD) Overview
* Risk Vectors
  * Risk with additional strategies
  * Smart contract risk
  * Custody risk
  * Pricing risk
  * Depeg risk
  * Collateral / Underlying asset risk
  * Risk exposure due to economic activities
* Conclusion

### Relation to Curve

Archimedes is a new platform that allows people to leverage farm in an experimental way. The platform is built on top of Curve, which has a special pool called lvUSD/3CRV that Archimedes uses to provide leverage to borrowers to farm the predefined yield strategies. People who lend money into this pool (called liquidity providers) can receive competitive yields, but they should also be aware of the risks involved with the funds being utilized in the leveraged yield strategies.&#x20;

We want to make sure that lenders (liquidity providers) are aware of these risks so that they can make informed decisions about whether or not to participate. While Archimedes is committed to offering better incentives than other stablecoin pools, it is important to be aware of the potential risks before investing.

## Archimedes Overview

#### Useful Links

* [Whitepaper (v2)](https://docs.archimedesfi.com/archimedes-finance-whitepaper-\(v2\))
* [Docs](https://docs.archimedesfi.com/)
* Archimedes [Blog](https://medium.com/archimedes-finance)
* [Github](https://github.com/thisisarchimedes/Archimedes\_Finance)
* Important [contracts](https://docs.archimedesfi.com/technical-documentation/contract-addresses)
* lvUSD/3CRV [Curve pool](https://curve.fi/#/ethereum/pools/factory-v2-268/deposit)
* Here you will find all of our published Audit reports:
  * [Smart Contract -Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Nov 2022
  * [Auctions - Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Auctions\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Dec 2022
  * [Zapper - Security Audit](https://github.com/thisisarchimedes/Archimedes\_Finance/blob/main/audit/Archimedes\_Finance\_Zapper\_Smart\_Contract\_Security\_Audit\_Report\_Halborn\_Final.pdf) - Jan 2023

Archimedes believes that a large portion of stablecoin liquidity sitting in AMMs is underutilized and power users in the ecosystem often struggle to access the volume of liquidity to meet their demand, as it appears to be scarce and expensive. Archimedes aims to use this underutilized liquidity to provide leveraged yield opportunities.

There are two participants in Archimedes

1. Liquidity providers:
2. Leverage farmers

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### The Liquidity Provider <a href="#095ac6548f6a4c5abeebff80ff1349f9" id="095ac6548f6a4c5abeebff80ff1349f9"></a>

The current process for LPs involves depositing 3CRV to the Curve Factory Pool at a rate of lvUSD / 3CRV and receiving fees in the form of $ARCH emission. It is important to note that the ability to engage with lvUSD is solely under the control of Archimedes, and there is no utility for someone to purchase lvUSD directly.

<figure><img src="../.gitbook/assets/discord1.PNG" alt=""><figcaption></figcaption></figure>

The provision of leverage to Leverage Takers (LT) is dependent on the availability of liquidity in the lvUSD/3CRV pool. To facilitate this, Archimedes may swap 3CRV for lvUSD, with the aim of generating yields for the LT via predetermined strategies. The success of these strategies relies heavily on sufficient liquidity in the pool.

To ensure the best yields for LPs, Archimedes utilizes dynamic ARCH emissions to stabilize the APY for the lvUSD pool. Currently, the sole means of compensation for LPs is via ARCH emissions from the protocol.

This is how their dynamic ARCH emission work:

<figure><img src="https://image-forwarder.notaku.so/aHR0cHM6Ly93d3cubm90aW9uLnNvL2ltYWdlL2h0dHBzJTNBJTJGJTJGbGgzLmdvb2dsZXVzZXJjb250ZW50LmNvbSUyRlRKYjhxc3YwRHYxSWJaNVBPV0hGWW8zemZFWU1zLXZWR1UwQkNMSndWRHlIZUl2b0ZYZnBIODRwQ0VqVVRDenBidWhhV3NGMS1vVVpQdmNPNHE4eVFWTDVpdGpyMWtIei14NUFkXzZnZVBZaHNqaTlUREtuNkF1Slc4Z2syVDlQMnA5dmdJdkQzZzE5VWMwMVR3ck83TTE0OUE1cEMwVmJ3RTZBX1hSSUlXdmVsbXVnV01tck5qc1ZxaENnenc_dGFibGU9YmxvY2smaWQ9ZjIwOWZmYTUtNDYyZS00NWExLTg1MmUtYjU0YzYyMTgzYzcwJmNhY2hlPXYyJndpZHRoPTE2MDA=" alt="Image without caption"><figcaption></figcaption></figure>

Source: [Docs - ARCH Dynamic Emission](https://docs.archimedesfi.com/tokenomics-and-ecosystem/arch-dynamic-emissions)

Basically, they compare other stablecoin pools and adjust their yield accordingly to attract liquidity.

* <mark style="color:yellow;">“Target APY” is calculated based on the predefined source (e.g.:</mark> [<mark style="color:yellow;">Defillama</mark>](https://defillama.com/yields?project=convex-finance\&attribute=stablecoins) <mark style="color:yellow;">data)</mark>
* <mark style="color:yellow;">\* If ARCH price isn’t available on Coingecko, the algorithm will use ARCH/ETH Uniswap pool data and Coingecko’s ETH/USD (taking the last 7 days average ETH/USD price).</mark>

It is important to note that manipulating the original on-chain data to affect the ARCH inflation is unlikely, as the use of an average of the collected data rather than absolute value in time makes it difficult to do so. However, there is a possibility that the data could be manipulated at the end of Defillama or Coingecko. Additionally, the team is currently determining the emission values with great care, which reduces the chances of ARCH emissions being different from what is intended.

The lvUSD supply is under the control of Archimedes, which means that they also control the price of lvUSD. Leverage takers can buy leverage and invest it in a yield strategy i.e. buying a ticket to mint outsized lvUSD, swapping it for 3CRV, and utilizing those funds to invest in a yield strategy. The assets obtained through swapping lvUSD for 3CRV can be viewed as a backing for lvUSD.&#x20;

The investment strategies are pre-defined and managed by Archimedes, and they have the option to close the strategic yield positions for leverage takers if necessary.

Investing in the lvUSD/3CRV pool exposes LPs to the risk of the underlying assets, including those under 3CRV and lvUSD. The risk exposure is directly related to all the predefined yield strategies. Currently, there is only one strategy that holds OUSD, but in the future, the risk exposure related to the strategies will depend on the investor's choice of strategy for yield farming.

### Leverage Takers

Archimedes offers leveraged strategies to its users, allowing them to choose from pre-decided options. Users pay a fee to borrow funds and provide collateral as a security deposit while opening a position. The protocol then executes the transaction on their behalf by borrowing the funds from the Curve pool and creating a leveraged position, which is represented by an NFT.

To obtain a leverage position, users provide ARCH tokens and the required collateral asset to Archimedes. The collected ARCH tokens from leverage takers are accumulated in the Archimedes treasury and will eventually be allocated to LPs through gauge allocation. The collateral asset required depends on the chosen strategy. Currently, there is only one strategy available (OUSD), and to use OUSD as leverage, OUSD is required as collateral. However, for the OUSD strategy, Archimedes is accepting assets in 3CRV, which will eventually be swapped to OUSD.

The leverage auction operates using a variant of a Dutch Auction, with Archimedes determining the amount of leverage to be auctioned and the initial price. This determines the amount of lvUSD that can be borrowed using 1 ARCH token as the minting power, with 10,000 lvUSD per ARCH token being an example.

Over time, the purchasing power of ARCH tokens increases with each block, allowing leverage takers to borrow more lvUSD per ARCH token. LTs have the option to either wait or purchase in, with the risk of missing out on available lvUSD if they choose to wait.

The auction will continue until all leverage has been auctioned to LTs, or the maximum limit of ARCH purchasing power is reached, which is decided by the team for each leverage round.

## Origin Dollar OUSD Overview

#### Useful Links

* [Docs](https://docs.ousd.com/)
* Origin Dollar [Blog](https://www.ousd.com/blog)
* [Governance](https://governance.ousd.com/proposals)
* [Github](https://github.com/originprotocol/origin-dollar)
* [Audits](https://docs.ousd.com/security-and-risks/audits)
* [OUSD Analytics](https://analytics.ousd.com/)
* [Yield Analytics](https://analytics.ousd.com/apy)

Origin Dollar (OUSD) was launched in September 2020 on the Ethereum network. Its design is superior to existing stablecoins because OUSD captures competitive yields while being passively held in wallets.

> OUSD is a stable currency that is backed 1:1 by other stablecoins like USDT, USDC and DAI. As a result, 1 OUSD should always be very close to 1 USD in value.
>
> Origin Dollar - [Docs](https://docs.ousd.com/how-it-works)

Origin Dollar DApp allows for the conversion of existing stablecoins (namely USDT, USDC, and DAI) to OUSD, which accrues compounding yield immediately. The platform also provides the option for users to convert their OUSD back into other stablecoins at any time.

Please note that a 0.25% exit fee is charged upon redemption with the vault, and this fee is distributed as an additional yield to the remaining participants in the vault.&#x20;

~~This fee serves as a security feature to prevent potential attackers from exploiting lagging oracles and siphoning stablecoins from the vault in the event of mispriced underlying assets.~~

When redeeming OUSD, the vault will determine which stablecoin(s) to return to the user, and in the current implementation, coins will be returned in the same ratio as the current holdings.&#x20;

OUSD deploys the stablecoins that have been deposited to the OUSD smart contract to other DeFi protocols such as Compound, Aave, and Curve to generate yields. The interest collected, trading fees, and rewards tokens are consolidated and converted to stablecoins to produce OUSD-denominated yields. To ensure that OUSD holders receive the best yield, the protocol moves assets in and out of different liquidity pools over time.

Here is the current allocation of funds for yield generation:

![](<../.gitbook/assets/image (2).png>)

Source: [OUSD Analytics](https://analytics.ousd.com/)

The returns generated by the protocol are shared with OUSD holders through constant rebasing of the money supply. OUSD adjusts the money supply based on the yield generated by the protocol, enabling the price of OUSD to remain at $1 while the balances in token holders' wallets reflect real-time yields earned by the protocol.

The principal is protected as long as nothing goes wrong with the underlying lending/AMM and stablecoin protocols.

## Past Exploits

On November 7th, 2020, OUSD was exploited for 7M USD due to a previously undetected reentrancy bug. You can read more details about the hack on our blog as well as the detailed compensation plan for taking care of the affected users. Origin Dollar was relaunched in December after completing multiple audits and security upgrades. You can learn more about the steps taken to secure the protocol in our relaunch announcement.

## Risk Vectors

### Risk with additional strategies.

Archimedes is designed to have multiple strategies but is only operating the OUSD strategy of writing this report. This section will guide the readers to get informed of the risk associated with the addition of the strategies and what are things to check for when a new strategy is included.

lvUSD is the one which will be used to provide leverage so 3CRV will be a core asset in defining a new strategy. If there is a new strategy that incorporated some vault tokens that accrue value, it is essential to check the underlying protocols and the principal allocation within those protocols for generating yield.

There will always be a risk associated with the assets under 3CRV as well as the risk associated with the factors that are controlled by the team manually. As of writing the report, it is upon the team's discretion to choose:

1. When to initiate a leverage round.
2. Available leverage for a particular leverage round.
3. Starting and ending price of leverage denominated in ARCH.
4. ARCH tokens available for emission towards LPs in lvUSD/3CRV pool

#### OUSD strategy

Understanding the strength and stability of OUSD is vital, as it is directly tied to the underlying stablecoin assets that support it. Any reduction in the value of the underlying stablecoins will have a corresponding impact on the value of OUSD. It is worth noting that although OUSD aims to maintain a 1:1 relationship between its supply and the number of supporting stablecoins, there is no guarantee regarding which stablecoins will form this backing, nor their value. Additionally, each supported stablecoin introduces a level of counter-party risk that should not be underestimated.

The OUSD smart contract functions by consolidating all stablecoin deposits from users into a single asset pool. This pool is then deployed into predetermined earning strategies based on specific allocations. The allocation of assets across these strategies is determined by veOGV holders, who participate in weekly snapshot voting that is conducted off-chain and is free from gas costs. Subsequently, the Strategist multi-sig, also referred to as "Strategists," will execute the results of the weekly poll on-chain, subject to their review and discretion.

### Smart Contract Risk

For Liquidity Providers (lenders), the relevant smart contracts are:

* Curve smart contracts
* Uniswap smart contracts
* OUSD smart contracts
* Archimedes’ smart contracts

The protocol smart contracts are audited by [Halborn Security](https://halborn.com/) to guarantee the quality and security of every change the team makes to its smart contracts. You can view the [Audits here.](https://docs.archimedesfi.com/audit-and-code-bounty/audits)&#x20;

**OUSD smart contract risk**

OUSD is [audited ](https://docs.ousd.com/security-and-risks/audits)and also offers a bug bounty of up to $250,000 OUSD. Origin has retained [Certora](https://www.certora.com/) to formally verify the various security properties of our contracts. They helped Origin to establish automated verifications that will run anytime we update our contract code. Origin has automated checking for common errors with [Slither](https://github.com/crytic/slither) and [Echidna](https://github.com/crytic/echidna) tests.&#x20;

However, it is important to note that even with formal audits, it is still possible for there to be logic errors that could lead to the loss of funds for OUSD holders.&#x20;

### Custody Risk

**OUSD Admin Privileges**

The primary admin is a 5 of 8 multisig contracts which is required to make any code changes to the protocol. OUSD can only be upgraded from this 5 of 8 multi-sig wallet. The keys to this multi-sig are held by individuals with close ties to the company, and not even the Origin founders acting together have enough control to execute owner functions on their own. In addition, the OUSD contracts are owned by a timelock which places a 48-hour time delay before any changes to the protocol can be made.&#x20;

Time-delayed admin actions give users a chance to exit OUSD if its admins become malicious, are compromised, or make a change that the users do not like.

Some functionality, such as rebalancing funds between strategies or pausing deposits, can be triggered without the timelock and with far fewer signers. This allows the Origin team to react more quickly to market conditions or security threats. These signers, known as Strategists,  have the ability to execute a limited number of functions __ with only 2 of 9 signers.

### Pricing Risk

#### OUSD strategy

The rebasing function treats 1 stablecoin as 1 OUSD for simplicity and to protect OUSD balances from being affected by the daily fluctuations in the price of the underlying stablecoins. Since the rebase function only counts coins, OUSD balances should only increase.&#x20;

In order to mint and redeem the appropriate number of OUSD on entry and exit, the smart contracts need to accurately price the USDT, USDC, and DAI that is entering and exiting the system.&#x20;

As an added precaution, OUSD never pays more than a dollar for a stablecoin, nor sells a stablecoin for less than a dollar. In situations where DAI, USDC, or USDT fall below the $1 peg, [OIP-4 disables the minting](https://github.com/OriginProtocol/origin-dollar/issues/1000) of additional OUSD tokens using the de-pegged asset. Oracles giving wrong prices will not result in a reduction in the number of stablecoins held. Gains that are collected as a result of stablecoins slipping from their peg are redistributed to the remaining holders of OUSD in the form of additional yield.

As a decentralized protocol, OUSD must rely on non-centralized sources for these prices. OUSD uses Chainlink oracles for pricing data for DAI, USDC, and USDT.

### Depeg Risk

There is no utility of lvUSD for the users and it is only meant to be utilized by Archimedes to facilitate leveraged position for the borrowers. lvUSD can break the peg to the 3CRV in the following situations:

1. Uncapped leverage distribution.
2. Unintended withdrawals of 3CRV by LPs
3. If assets under 3CRV lose their dollar peg.

There are mechanisms where Archimedes can force a borrower to close their position for controlling the lvUSD supply. Besides this, it is possible that LP would face a greater slippage at the time of exit as a rational LP would exit withdrawing funds solely in 3CRV.

> lvUSD is over collateralized by the underlying assets. In case the underlying asset loses its peg (<< $1), Archimedes automatically closes the LTs’ positions (no penalty to users - this isn’t liquidation, this is closing the position, returning the debt and making the remaining profit available to the LT) . All lvUSD debt is paid back and the borrowed 3CRV returns to the Curve pool.
>
> Source: [Archimedes Whitepaper](https://docs.archimedesfi.com/archimedes-finance-whitepaper-\(v2\)#16330cba61254d179a0c4dc52d0ca233)

<figure><img src="../.gitbook/assets/1233.png" alt=""><figcaption></figcaption></figure>

Though Archimedes can force close the borrowing positions, leverage takers also have the incentive to close their position at a discount if lvUSD is trading lower than a dollar.&#x20;

**Peg stabilization mechanism**

As long as lvUSD keeps its peg, LPs can enter and exit without incurring significant slippage. The protocol uses the following mechanisms to maintain the peg:

* Leverage is capped: Archimedes only opens a new auction when there is enough liquidity in the pool. Each auction is for a limited amount of leverage and is designed to maintain the balance of the pool.
* lvUSD >> $1: If lvUSD rises above $1, Archimedes will raise the leverage cap, thus more lvUSD will be minted and enter circulation, returning the lvUSD peg back to around $1.
* lvUSD << $1: If lvUSD drops below $1, LTs have an arbitrage opportunity by unwinding their position. LTs will pay back $1 worth of debt for less than $1, pocketing the instant arbitrage profits. This process burns lvUSD and increases the amount of 3CRV in the pool.

### Collateral Risk / Underlying Assets Risks

If the OUSD peg drops below $1 to a value where Borrowers do not have enough OUSD to pay their leveraged debt, this would typically cause a liquidation of the users’ collateral.&#x20;

In this scenario, Archimedes will instead lock the user's position until there is enough OUSD to pay the debt _(either by OUSD regaining its peg or by the position earning more yield over time)._ The user can still exit by selling their position NFT in an NFT marketplace, such as OpenSea.

**OUSD Third-party platform risk**

OUSD is built on top of other DeFi platforms like Aave, Compound, and Curve which add additional smart contract risk. There are no guarantees that the underlying third-party platforms will continue to work as intended, and any failure in an underlying strategy would potentially lead to a loss of funds for OUSD holders.

### Risk exposure due to economic activities

#### Slippage

LPs should be aware that entering and exiting the liquidity pool with single-sided liquidity would incur slippage. Moreover, the pool proportion at the time of entry and exit would impact the resulting slippage.

Archimedes can control the pool proportion by providing adequate lvUSD leverage. They intend to keep the pool proportion near 50% for lvUSD at the time of computing leverage rounds.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

**Inflation**

With dynamic emissions, Archimedes wants the TVL to grow constantly. They are providing emission rewards to the LPs in ARCH tokens for this intended outcome.&#x20;

So far, the leverage available per ARCH token is cheaper than the ARCH emitted to sustain the liquidity in the lvUSD/3CRV pool. In other words, the absorption of ARCH tokens (in the form of leverage fees) is lower than that of the emission provided. This creates inflation in the ARCH price and with more and more spread between the absorption and emission, the price of the ARCH token would drop.&#x20;

<figure><img src="../.gitbook/assets/image (1) (3).png" alt=""><figcaption></figcaption></figure>

Source:  [Dune Query](https://dune.com/queries/2238660)

There are events that can lead to a subsequent chain of events that would result in a price decline for ARCH tokens.&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

When the availability of leverage is less, the inflation on ARCH price will rise. This is possible when the pool ratio is heavily shifted towards lvUSD. At this time protocol will rely on the borrowers to close their position in order to provide further leverage to new leverage takers.

This again poses a risk to the LP because the gains made by the leverage taker when they close their position to repay the debt at a discount will be suffered by the LP in terms of slippage.

As discussed earlier, inflation is already a catalyst in the chain of these subsequent events.

## LlamaRisk Gauge Criteria

Centralization Factors

1.  Is it possible for a single entity to rug its users?

    Yes, because the protocol controls and owns all the funds invested in the yield strategies. But there is a timelock that might save users if they are active enough.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

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
