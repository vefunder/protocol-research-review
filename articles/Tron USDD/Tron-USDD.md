
# **Asset Risk Assessment: USDD**


#### A deep dive into TRON’s stablecoin.


# Useful links:


* [USDD website](https://usdd.io/) | [Docs](https://docs.usdd.io/) | [Blog](https://medium.com/@usddio) | [Coingecko](https://www.coingecko.com/en/coins/usdd) | [USDD Twitter](https://twitter.com/usddio) | [TDR Twitter](https://twitter.com/trondaoreserve)
* [TronDAO Reserve](https://tdr.org/#/)
* [USDD Peg Stability Module](https://usdd.io/#/psm)

Audits

* [TRX burning contract audit by SlowMist](https://tdr.org/MultiSigFundRaiser.pdf)
* [Issuance contract audit by SlowMist](https://tdr.org/MultiSigLocker.pdf)
* [Authorized contract](https://tdr.org/MultiSigAuthorizer.pdf) [audit by SlowMist](https://tdr.org/MultiSigAuthorizer.pdf)
* [PSM contract audit by SlowMist](https://tdr.org/SlowMistAuditReport-PSM.pdf)
* [SlowMist SC audit (TRC20)](https://usdd.io/SlowMistAuditReport-USDDTRC20.pdf)
* [SlowMist SC audit (ERC20)](https://usdd.io/SlowMistAuditReport-USDD.pdf)

Contracts - USDD on other chains

* [USDD on Ethereum](https://etherscan.io/token/0x0C10bF8FcB7Bf5412187A595ab97a3609160b5c6)
* [USDD on Tron](https://tronscan.org/#/token20/TPYmHEhy5n8TCEfYGqW2rPxsghSfzghPDn)
* [USDD on BSC](https://bscscan.com/token/0xd17479997f34dd9156deef8f95a52d81d265be9c)
* [Polygon](https://polygonscan.com/token/0xffa4d863c96e743a2e1513824ea006b8d0353c57) | [Fantom](https://ftmscan.com/token/0xcf799767d366d789e8b446981c2d578e241fa25c) | [Avalanche](https://snowtrace.io/token/0xcf799767d366d789e8b446981c2d578e241fa25c) | [Aurora](https://explorer.aurora.dev/token/0x941b45cb782CcEd9814821E9b684261fa353BCD5/token-transfers)

Articles

* [Nasdaq](https://www.nasdaq.com/press-release/more-whitelisted-members-wider-dex-integrations-higher-total-supply-usdd-thrives)
* [Messari Q4 Report](https://messari.io/report/state-of-usdd-q4-2022?referrer=all-research)
* [Coinmarketcap](https://coinmarketcap.com/community/articles/637615e238e8b54c28f2a219/)
* [Coindesk](https://www.coindesk.com/markets/2022/05/14/justin-sun-talks-usdd-stablecoin-in-wake-of-lunaust-unravel/)


# TL;DR

This report investigates the USDD stablecoin and the risks posed to Curve LPs in the [USDD/3CRV](https://curve.fi/#/ethereum/pools/factory-v2-116/deposit) and [USDD/FraxBP](https://curve.fi/#/ethereum/pools/factory-v2-135/deposit) pools. Both pools passed votes for a gauge in May and July 2022, respectively. Original governance proposal [here](https://gov.curve.fi/t/proposal-add-usdd-3crv-pool-to-the-gauge-controller/3891). 


## A quick TL;DR of our findings:

* USDD is an over-collateralized stablecoin natively issued on the TRON blockchain. Initially launched as a copycat of UST, it pivoted to an over-collateralization model shortly after Terra’s crash.
* Compared to other over-collateralized stablecoins, such as DAI, USDD is not permissionless or decentralized. The stablecoin can only enter circulation via a 5-of-7 multi-sig controlled by the seven whitelisted members of the Tron DAO Reserve (TDR). The TDR has full control over all smart contracts, custody of collateral and reserves, and minting of new USDD.
* The circulating supply of USDD ($725M) hasn’t changed since early July 2022. The TRX that is backing the circulating USDD only accounts for 85% of its value. However, the TDR has added reserves in BTC, USDT, and USDC. The collateral and reserves together account for a collateralization ratio of over 160%. Around 65% of the total backing is in Tron’s TRX.
* USDD has two stability mechanisms: A Peg Stability Module (PSM) and four Monetary Policy Instruments (MPI). However, the PSM has always remained empty (i.e no funds to trade against) and unutilized.
* Direct measures to reinforce the USDD peg are a rare occurance. There is no sign that collateral or reserves were used to directly support USDD’s price. Consequently, USDD has extensively traded below the $1 peg for most of its existence.
* The USDD team repeatedly expresses false and misleading statements. For instance, it falsely claims to be “the first over-collateralized and decentralized stablecoin”. Another example is the designation of the so-called “BurnContract”, which in reality is just a multi-sig holding the TDR collateral.
* Technical security appears to be taken seriously. USDD has undergone several audits by SlowMist. All the major components were assessed. There is also an active bug bounty program.
* There is no semblence of decentralized governance. There is no governance forum, nor is there snapshot voting or on-chain governance. The team behind USDD is also anonymous, with implicit ties to Justin Sun.


# USDD - Introduction

Decentralized USD (USDD) is the stablecoin of the TRON ecosystem launched on [May 5, 2022](https://www.theblock.co/post/145231/trons-new-stablecoin-usdd-goes-live). USDD was initially launched as an algorithmic stablecoin [inspired by]((https://www.coindesk.com/markets/2022/05/04/revolution-promised-by-trons-justin-sun-looks-like-clone-of-terras-algorithmic-stablecoin/)) the previous success of Terra's UST. As with UST, the plan was to incentivize USDD with a 30% APY. And as LUNA provided the algorithmic backing for UST, Tron’s native token TRX would serve as [collateral](https://medium.com/coinmonks/new-algorithmic-stablecoin-usdd-promises-30-apy-is-this-a-terra-ust-copycat-2d2787af14b2) for USDD. Following Terra's collapse, however, USDD pivoted to an over-collateralized stablecoin model that would be resilient to a similar death spiral in case of a bank run.

USDD differs from other crypto-collateralized stables like DAI in that USDD’s issuance is permissioned. USDD is minted by the Tron DAO Reserve (TDR): A group of seven whitelisted institutional members that can issue USDD by depositing collateral. Currently, the only accepted collateral is Tron’s native token TRX. In addition to the TRX backing, the TDR has also built up reserves in other currencies such as BTC, USDT, and USDC. The TDR has full control over all relevant smart contracts, including the minting of new USDD and the custody of the collateral and reserves. Current [members](https://tdr.org/#/) of the TDR include Poloniex, Multichain, and Amber Group among others. The now insolvent Alameda Research was originally a member but has since been removed.

Since its inception, USDD has struggled to keep its peg at the one Dollar mark. It has traded several percentage points below the peg for most of its history.


# A Dive into USDD

USDD is natively issued on the TRON blockchain and is also available on Ethereum, BSC, Avalanche, and Arbitrum. The cross-chain protocol BitTorrent Chain (BTTC) is used to transfer USDD from Tron to other chains. The BTTC [smart contract](https://tronscan.org/#/contract/TU1CmpmWbCrFXqLLqMaKL2Q1d34bJNYLJe/code) is also the second largest [holder](https://tronscan.org/#/token20/TPYmHEhy5n8TCEfYGqW2rPxsghSfzghPDn/holders) of USDD on Tron.

USDD has integrations with several DeFi applications. The most significant amount of TVL (~[$186M](https://tronscan.org/#/contract/TX7kybeP6UwTBRHLNPYmswFESHfyjm9bAS)) is held by [JustLend DAO](https://app.justlend.org/?lang=en-US#/market), a borrowing and lending protocol on Tron. On Ethereum, USDD can be traded on [Uni V3](https://etherscan.io/address/0x1c5c60bef00c820274d4938a5e6d04b124d4910b#tokentxns) and [Curve](https://etherscan.io/address/0xe6b5cc1b4b47305c58392ce3d359b10282fc36ea). USDD is also traded on Binance Smart Chain’s [Pancake Swap](https://bscscan.com/address/0x004a9ce83614551271bcbd48e5fa3b8648dbe312).

When launching USDD, the TRON DAO Reserve (TDR) issued and released $725M USDD from the initial smart contract. The issuance required a matching deposit of TRX from the TDR in order to execute. During the first few weeks, over 8.9B TRX tokens were deposited by TDR members, or nearly 10% of the circulating TRX supply. Almost all funding came from one single [address](https://tronscan.org/#/address/TV6MuMXfmLbBqPZvBHdwFsDnQeVfnmiuSi) which is tagged as a Binance address.

As of this report's publishing, the circulating supply hasn’t changed much. According to [DeFi Llama](https://defillama.com/stablecoin/usdd), the circulating USDD supply is around $725M. Most of it is on Tron (~70%), around 17% is on Ethereum, followed by BSC with about 12%.


![defillama-usdd-total-circulating](https://user-images.githubusercontent.com/89845409/220379825-351836d4-00ae-42cc-b761-82e89d3a7980.png)
(Source: [DeFi Llama](https://defillama.com/stablecoin/usdd))

USDD is also listed on a handful of centralized exchanges. For instance, Poloniex, KuCoin, and Huobi ([Justin Sun](https://en.wikipedia.org/wiki/Justin_Sun), founder of the Tron network, is an advisor and investor at [Huobi](https://decrypt.co/112118/huobi-token-moons-the-week-justin-sun-becomes-exchange-advisor)).

Recently, USDD became an officially [authorized digital currency](https://cryptoslate.com/tron-becomes-legal-tender-in-dominica/) and medium of exchange for the Commonwealth of Dominica, a small island developing state in the Caribbean. This was accomplished thanks to Mr. Sun's promotional activities. In late 2021, Justin Sun left his role as CEO at Tron to promote crypto adoption in developing states. He began wielding influence in the Caribbean and became a WTO [diplomat](https://www.bloomberg.com/news/articles/2021-12-17/crypto-wunderkind-sun-says-he-s-becoming-a-diplomat-for-grenada) for the state of Grenada.


## USDD Issuance

There are two ways for new USDD to enter circulation.
* USDD can be issued by the seven members of the TRON DAO Reserve (TDR). These whitelisted institutions can stake TRX, and withdraw USDD in return.
* Anyone can permissionlessly swap USDD for other stablecoins (e.g. USDT, USDC) via the Peg Stability Module (PSM).

At launch, the TDR pre-minted $999B of USDD. These sit in the “[MultiSigLocker contract](https://tronscan.org/#/contract/TRFGnuUqED3NDpMYgqZY1X3gAeVHNw1SDq)”. From there, $1B was transferred to a “[MultiSigAuthorizer contract](https://tronscan.org/#/contract/TTsASxQhMk4t3S5vZMVVJ7nR2GQjDXNRnq)”. The TDR controls both contracts via a 5-of-7 multi-sig. Once in the authorized contract, TDR members and regular users can issue or swap USDD.


![docs-usdd-issuing-process](https://user-images.githubusercontent.com/89845409/220379993-ae616d3a-2f4f-4069-bb06-51f6ec7aa761.png)
(source: [USDD Docs](https://docs.usdd.io/usdd-issuance/usdd-issuance))


**USDD issued by staking TRX (TDR members only)**:

1. TDR members deposit TRX into the [TRX Burning Contract](https://tronscan.org/#/contract/TNMcQVGPzqH9ZfMCSY4PNrukevtDgp24dK) (controlled by the 5-of-7 multisig of TDR members).
2. The TDR calculates the dollar value of TRX and transfers an equivalent dollar value of USDD from the [MultiSigAuthorizer contract](https://tronscan.org/#/contract/TTsASxQhMk4t3S5vZMVVJ7nR2GQjDXNRnq) to the “circulation account” via multi-sig (5-of-7).
3. The TDR converts the [TRC10 USDD](https://tronscan.org/#/token/1004777/transfers) in the “circulation account” into [TRC20 USDD](https://tronscan.org/#/token20/TPYmHEhy5n8TCEfYGqW2rPxsghSfzghPDn) and transfers the USDD to its member.


**USDD issued by swapping stablecoins (PSM)**:

1. The TDR releases the authorized and un-issued TRC10 USDD from the Authorized Contract to a [SafeVault Contract](https://tronscan.org/#/contract/TMgSSHn8APyUVViqXxtveqFEB7mBBeGqNP) ([minter](https://tronscan.org/#/contract/TUhQDzXJ3QsT6F2KiK5gZ3H673TV1516E9/code), [owner](https://tronscan.org/#/address/TPcnRbpuB89eKKkfH2E7iJooAqeEZPpdGb)) in the PSM.
2. When users swap other stablecoins for USDD, the PSM Contract converts TRC10 USDD in the SafeVault into TRC20 USDD and transfers it to users.

The Peg Stability Module supports USDD to arb its upside peg. In other words, if USDD trades above one US-Dollar, traders are incentivized to swap other stablecoins for USDD using the PSM. They can then sell USDD at another venue. Thus, making a profit and bringing down the price back to $1. However, the PSM is currently empty since [USDD](https://www.coingecko.com/en/coins/usdd) has not been trading above one Dollar in several months. As a result, there are no incentives to use the tool.

[Sidenote: USDD’s [documentation](https://docs.usdd.io/usdd-issuance/usdd-issuance) can be misleading, as the terms “staking” and “burn contract” are used for the same interaction. The “TRX Burning Contract”  is an incorrect designation. In reality, the [Burning Contract](https://tronscan.org/#/contract/TNMcQVGPzqH9ZfMCSY4PNrukevtDgp24dK) just stores the TRX. The “burned” TRX can always be redeemed by the TDR multi-sig.]


## Replenish the Reserves

Once the amount of USDD in the authorized contract falls below $500M, the TDR will replenish the reserves. The TDR simply releases more TRC10 from the Issuance Contract to the Authorized Contract via multi-sig. After the transaction receives five out of seven signatures, the USDD will be locked for a minimum of 10 days.

The [Issuance Contract](https://tronscan.org/#/contract/TRFGnuUqED3NDpMYgqZY1X3gAeVHNw1SDq) still holds $997.9B USDD of the $999B pre-minted, meaning that a single infusion of $1.1B USDD has been released to the [Authorized Contract](https://tronscan.org/#/contract/TTsASxQhMk4t3S5vZMVVJ7nR2GQjDXNRnq/code). Overall, the issuance of USDD has remained very constant. On-chain data and reports by Messari show that no new USDD was issued over the past six months.

![Messari-usdd-issuance-q4](https://user-images.githubusercontent.com/89845409/220380087-cb83c9e3-58fe-4c5e-896c-cc53c83703fd.png)

(source: [Messari](https://messari.io/report/state-of-usdd-q4-2022?referrer=all-research))

The USDD supply has remained unchanged at around $725M. This suggests that demand for USDD did not increase since its inception. This observation is further evidenced by a slow growth rate in user adoption, i.e. the number of wallets holding USDD stayed flat in the last quarter according to [Messari](https://messari.io/report/state-of-usdd-q4-2022?referrer=all-research).

![image](https://user-images.githubusercontent.com/51072084/224406624-87717c26-6473-4707-9372-8ad271fa90b4.png)

(source: [Messari](https://messari.io/report/state-of-usdd-q4-2022?referrer=all-research))


## Collateral backing USDD

USDD advertizes a high collateralization ratio (CR) on its website. At the time of writing, the CR hovered around 170%. The total collateral is supposedly valued at $1.25B, with $725M USDD in circulation.

![usdd-website-total-collateral](https://user-images.githubusercontent.com/89845409/220380284-5a46510f-8095-4af3-8b06-7dfe25c679f6.png)

(source: [Tron DAO Reserve](https://tdr.org/#/))

Moreover, USDD  proclaims that its stablecoin is backed by “multiple mainstream digital assets” like TRX, BTC, and USDT. To confirm this statement, the website lists all wallets that contain their collateral and reserves.

A quick look into those wallets reveals that USDD is mostly backed by the TRX held in the burn contract. In addition, three reserve accounts hold extra TRX, BTC, jUSDC, and jUSDT (i.e. USDC and USDT deposited into JustLend). In summary, 65% of the total collateral and reserves consist of TRX. BTC makes up 27%, while USDC and USDT account for about 6%.


<table>
  <tr>
   <td><strong>Wallet</strong>
   </td>
   <td><strong>Token</strong>
   </td>
   <td><strong>Number of Tokens</strong>
   </td>
   <td><strong>$-Amount</strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://tronscan.org/#/contract/TNMcQVGPzqH9ZfMCSY4PNrukevtDgp24dK/code">Burn Contract</a>
   </td>
   <td>TRX
   </td>
   <td><p style="text-align: right">
8,997,572,215</p>

   </td>
   <td><p style="text-align: right">
$616,333,697</p>

   </td>
  </tr>
  <tr>
   <td><a href="https://www.blockchain.com/explorer/addresses/btc/1KVpuCfhftkzJ67ZUegaMuaYey7qni7pPj">BTC Reserve</a>
   </td>
   <td>BTC
   </td>
   <td><p style="text-align: right">
14,041</p>

   </td>
   <td><p style="text-align: right">
$311,560,914</p>

   </td>
  </tr>
  <tr>
   <td><a href="https://tronscan.org/#/address/TZ1SsapyhKNWaVLca6P2qgVzkHTdk6nkXa/contracts">TRX Reserve</a>
   </td>
   <td>TRX
   </td>
   <td><p style="text-align: right">
971,958,097</p>

   </td>
   <td><p style="text-align: right">
$66,579,130</p>

   </td>
  </tr>
  <tr>
   <td rowspan="6" ><a href="https://tronscan.org/#/address/TSF2rqLdrrZG7PZkDxtvu6B2PTpofidMAX">other Reserve</a>
   </td>
   <td>TRX
   </td>
   <td><p style="text-align: right">
960,009,231</p>

   </td>
   <td><p style="text-align: right">
$65,760,632</p>

   </td>
  </tr>
  <tr>
   <td>jUSDC
   </td>
   <td><p style="text-align: right">
3,967,811,243</p>

   </td>
   <td><p style="text-align: right">
$39,958,657</p>

   </td>
  </tr>
  <tr>
   <td>jUSDT
   </td>
   <td><p style="text-align: right">
2,947,042,032</p>

   </td>
   <td><p style="text-align: right">
$29,978,884</p>

   </td>
  </tr>
  <tr>
   <td>USDJ
   </td>
   <td><p style="text-align: right">
20,000,000</p>

   </td>
   <td><p style="text-align: right">
$22,495,738</p>

   </td>
  </tr>
  <tr>
   <td>JST
   </td>
   <td><p style="text-align: right">
1,957,251</p>

   </td>
   <td><p style="text-align: right">
$53,237</p>

   </td>
  </tr>
  <tr>
   <td>ApeNFT
   </td>
   <td><p style="text-align: right">
48,832,840,665</p>

   </td>
   <td><p style="text-align: right">
$23,826</p>

   </td>
  </tr>
  <tr>
   <td><strong>Total</strong>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td><p style="text-align: right">
<strong>$1,152,744,715</strong></p>

   </td>
  </tr>
</table>

(source: [table](https://docs.google.com/spreadsheets/d/1LnNj2Jm9Dun4DMoKDfpTyIHy0JHTo2chxqGORZS49ek/edit#gid=135516406))

In total, $1.15B can be found by combining the collateral and the reserves (see table above). On its website, however, USDD claims that $1.25B is collateralizing its stablecoin, a difference of ~$100M.

This difference stems from an above market price calculation for TRX on the [USDD website](https://usdd.io/#/). The calculation of the above table was based on the TRX price from [Coingecko](https://www.coingecko.com/en/coins/tron) ($0.0685). The USDD website, on the other hand, applies a higher price per TRX (~$0.0806), a difference of plus 17.6%.

Subsequently, the CR is also not correct. Instead of 172%, the actual ratio is 159%, including all the reserves. By only looking at the burn contract, the $725M outstanding USDD is backed by $616M worth of TRX, or a CR of merely 85%.

Overall, USDD is sufficiently backed by several top-tier tokens. There is, however, a high concentration of TRX. Some of the reserves are either staked or deployed to other Tron-based protocols (e.g. JustLend), increasing the composability risk. Furthermore, there is an ever-present dependence on the TDR to always be actively defending the peg, since they have unilateral control over the USDD backing.


## USDD Cross-Chain

As mentioned earlier, USDD is supported on Tron, Ethereum, BSC, Avalanche, and Arbitrum. For cross-chain transactions, USDD utilizes the BitTorrent network. BitTorrent (BTTC) is a cross-chain [bridge](https://app.bt.io/bridge) protocol, which was bought by Justin Sun in 2018. It relies on a PoS (Proof of Stake) consensus mechanism and multi-node validation. Currently, the BTTC network is secured by 12 [validators](https://app.bt.io/staking). Among them are Binance and Huobi (another investment of Justin Sun).

Comparing the amount of USDD on Ethereum and BNB with the amount in the BTTC bridge, the numbers curiously don't add up, raising questions that USDD is not fully backed. For instance, the BTTC bridge for Ethereum contains [$116M USDD](https://scan.bt.io/#/contract/0xb602f26bf29b83e4e1595244000e0111a9d39f62), although according to Etherscan there is over [$124M USDD](https://etherscan.io/token/0x0C10bF8FcB7Bf5412187A595ab97a3609160b5c6) on Ethereum (a difference of ~7M USDD). The same is true for the Binance Smart Chain. While the BNB contract on the BTTC bridge contains: [$89M USDD](https://bttcscan.com/address/0x74e7cef747db9c8752874321ba8b26119ef70c9e), there is actually over  [$110M USDD](https://bscscan.com/token/0xd17479997F34dd9156Deef8F95A52D81D265be9c) on BNB (an even higher difference of over $21M). It’s not fully clear where the remaining amounts are coming from, but speculation is that they were transferred from a CEX.


## Peg Stability Mechanism

The initial plan for maintaining the peg was very [similar](https://www.coindesk.com/markets/2022/05/04/revolution-promised-by-trons-justin-sun-looks-like-clone-of-terras-algorithmic-stablecoin/) to Terra’s UST. Mainly the following two stability mechanisms were planned:


* **Short-term Arbitrage** - USDD was to maintain its price around a dollar, using the basic algorithm of 1 USDD = 1 dollar worth of TRX. In this scenario, TRX had a similar role to LUNA and would have been burned/minted when swapping USDD.
* **Building up Reserves** - A $10B reserve should help to defend the peg. It was predicted that the TDR would [raise $10B](https://trondao.medium.com/an-open-letter-to-our-community-on-establishing-the-tron-dao-reserve-3eda495f07c2) worth of highly liquid assets within six to twelve months, starting from May 2022 (USDD release date). While Terra [stated](https://twitter.com/stablekwon/status/1483973126161842176) the same intentions, the $10B reserve did not become a reality.

However, as mentioned earlier, the USDD algo-stable model never came to fruition. A few weeks after the UST crash, the TDR [launched](https://twitter.com/usddio/status/1554823567904149505?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1554823567904149505%7Ctwgr%5E1b5ae4f96116046b953f2beb17f50a00fd9c1fa6%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Ftronspark.com%2Ftrondao-psm%2F) the Peg Stability Module (PSM). A tool that enables users to swap between USDD and USDT, without slippage and fees. The PSM was supposed to serve as an additional stability mechanism, based on arbitrage activities. However, the PSM module is not currently active, as USDD has been suffering from a downside depeg for quite some time. The PSM can only protect USDD in case of an upside depeg (i.e USDD > 1 US-Dollar). In the current scenario, filling the PSM with “counter-swap” stablecoins would produce constant losses for the protocol.


![coingecko-usdd-offpeg](https://user-images.githubusercontent.com/89845409/220380464-55134e01-d76d-4550-8785-b46a96957c53.png)
(source: [Coingecko](https://www.coingecko.com/en/coins/usdd))

In addition to the PSM, the TDR put in place several [monetary policy instruments](https://docs.usdd.io/faq/what-monetary-policies-will-the-tdr-adopt-to-maintain-the-value-of-usdd) (MPI) to maintain USDD’s price stability. Below are the four main policy instruments applied by the TDR.


* **MPI 1: Setting the Interest Rate -** Setting interest rates is a mechanism often used to regulate the supply and demand of currencies and loans. The TDR claims to be working with its partner protocols, to set USDD’s benchmark interest rates. The initial interest rate was targeted to be 30%. Today, the rate is very different, depending on the protocol. On certain platforms, it can be higher than 30%. In certain instances, the rate is subsidized by the TDR, to attract new users and drive up its adoption.

* **MPI 2: Open Market Operations -** Through Open Market Operations, the TDR performs expansion and contraction of USDD's “monetary base”. The intention is to stabilize its value (supply and demand balance). To keep the USDD pegged to $1, the TDR claims to buy or sell USDD and reserve assets (TRX, BTC, USDT, and USDC). However, there is little on-chain proof of such transaction. Hence, it is assumed that these operations take place via CEXs.

* **MPI 3: Window Guidance -** The TDR has partnered with DeFi protocols (e.g. JustLend) and centralized exchanges (e.g. Poloniex, Huobi) to be able to more effectively control USDD. The mentioned partners have the ability to limit or even temporarily pause the lending of USDD and TRX, in order to protect the "USDD monetary system” from short-sellers.

* **MPI 4: Mint-and-Burn -** as mentione earlier, the TDR has a mint-and-burn mechanism in place. Depending on the situation, the TDR also uses additional methods such as enabling or disabling the minting process, adjusting the mint-burn ratio, and imposing upper limits on daily minting and burning activities. However, this method has not been used since its inception.

In conclusion, the TDR proclaims a set of tools called monetary policy instruments, that are supposed to keep USDD’s peg. However, as seen in the image above, this is not working perfectly. USDD has been off-peg since June 2022 and has been struggling to get back to the one Dollar mark ever since.

## Risk of Providing Liquidity on Sunswap

Given that the largest share of USDD usage happens on the Tron network, risks related to DeFi protocols on Tron need to be considered as well. Currently, the lending protocol [JustLend]([url](https://app.justlend.org/?lang=en-US#/marketDetailNew?jtokenAddress=TX7kybeP6UwTBRHLNPYmswFESHfyjm9bAS)) is the largest USDD holder. And as highlighted in the Collateral section, all stablecoins in the TDR reserves are deposited into JustLend. Users should be aware of the borrowing factors. That is the amount of USDT or USDC that is borrowed on JustLend. A high utilization could prevent fast withdrawals of USDT or USDC reserves, in case they’re needed to back USDD.

The 2nd and 3rd largest USDD holders are the BitTorrent cross-chain contracts for Ethereum and BSC. Risks related to BitTorrent were mentioned in the previous section.

The fourth largest holder is Sunswap. The DEX applies an [AMM model](https://docs.sun.io/swap/stableswap-overview) based on the [stable swap](https://xord.com/research/curve-stableswap-a-comprehensive-mathematical-guide/) invariant formula (designed by Curve Finance). Currently, the [USDD/USDT](https://tronscan.org/#/contract/TNTfaTpkdd4AQDeqr8SGG7tgdkdjdhbP5c/code) contract is the largest USDD pool.

There is a potential risk for USDD/USDT liquidity providers on Sunswap because of a vulnerability that was discovered in the old Curve smart contracts. In September 2020, Peter Zeits [revealed](https://medium.com/@peter_4205/curve-vulnerability-report-a1d7630140ec) that an update to the [amplification factor](https://curve.readthedocs.io/exchange-pools.html#amplification-coefficient) can expose liquidity providers to losses. If exploited, this could lead to significant losses for LPs. Curve rewarded Peter for the bug discovery and resolved the issue shortly after.

It is questionable whether SunSwap also took preventive measures concerning these A-factor updates. The recommendations - which Curve implemented after the bug discovery - was that updates need to happen granularly and ideally when the pool is balanced. Large changes to the A-factor are problematic. 

According to Sunswap’s [event logs](https://tronscan.org/#/contract/TXKQU4pV1nAVpdFLm31LNVVa8pCzNQDNhU/transactions), however, the A-factor has been adjusted very frequently; sometimes up to three times in one day. Moreover, it was adjusted in large increments. There appears to have been a fat finger incident in July 2022 when A was increased 50x from 200 to 10000 before quickly being reduced again to 1000. This [spreadsheet]([https://docs.google.com/spreadsheets/d/1UwezmtfkWGrZsVLyc8T249CN03zR251uFbO-k6x5afU/edit#gid=0](https://docs.google.com/spreadsheets/d/e/2PACX-1vRjL32X820GDM925Yil5fNa7w00HR4E-5IXXG9NdH4_Y3hOGnqouxZx2Q3YyY53jku30zEazvrGv7L6/pubhtml?gid=0&single=true)), created by the authors, shows in detail how the amplification factor was amended. Since contacting the team behind USDD or Sunswap was not successful, it is unclear whether they are aware of this potential issue. Hence, users should be cautious when providing liquidity.


## Misleading Marketing

While conducting this report, it became evident that Tron and USDD repeatedly use false or misleading wording and designations throughout their marketing and communications. Some examples are listed below (this list is not exhaustive).

**False Statements**


* USDD is obviously not the first over-collateralized and decentralized stablecoin (see image below). USDD first came into existence in 2022. Over-collateralized and decentralized stablecoins have existed since 2016. Moreover, USDD is far from decentralized with questionable collateral backing.

    

![twitter-usdd-frontpage](https://user-images.githubusercontent.com/89845409/220380580-58334394-83fd-4ac1-ae82-a4cbd80fed4c.png)
(source: [Twitter](https://twitter.com/usddio))
    

* On the [website](https://usdd.io/#/), USDD promises multiple times to be decentralized, tamper-proof, and independent from centralized entities. This shows a very unique interpretation of these terms, given that a 5-of-7 multi-sig fully controls USDD. The key holders are centralized entities and are completely responsible for maintaining stability of the system.

**Debatable Promises**

* Further, the promise of being “fully backed and over-collateralized by mainstream assets” is debatable. As eluded earlier, USDD is 65% backed by Tron’s native token TRX. Secondly, the actual “collateral” in the burn contract only adds up to a CR of 85%. The rest comes from reserves, which were voluntarily stacked up by TDR members.
* In consequence, the promised CR is also a matter of perspective. The TRX deposited by the TDR is currently worth less than the outstanding USDD. The collateral is not very well diversified, and an above-market price is used to measure TRXs’ collateral value on the website.
* In its outdated [docs](https://docs.usdd.io/usdd-issuance/over-collateralization-and-transparency), USDD claims to have a CR of over 319%.

**Misleading Naming**

* As mentioned above, the TRX deposited by the TDR to issue new USDD is not burned. Naming a simple multi-sig as a “burning contract” is misleading. This naming convention is also used at Tronscan (see image)

![tronscan-total-trx-burned](https://user-images.githubusercontent.com/89845409/223773543-59f6c7b5-28f7-4f0b-9a23-dccc83fa0e23.png)
(source: [Tronscan](https://tronscan.org/#/data/charts2/circulation/destroyCount))

* In their [docs]([url](https://docs.usdd.io/usdd-issuance/usdd-issuance)), the term staking is used instead of burning. However, staking would require that the assets are at risk of being slashed, and that stakers earn interest or rewards for putting their assets into a staking contract. Both do not apply to TDR members. They only deposit TRX into an account, to which they collectively have access.


# Risk Vectors


## Smart Contract Risk

All relevant components of the USDD tech stack have undergone an audit. All smart contract audits were conducted by SlowMist:

The [TRC-20 token audit](https://usdd.io/SlowMistAuditReport-USDDTRC20.pdf) did not find any issues. However, for the [ERC-20 token audit](https://usdd.io/SlowMistAuditReport-USDD.pdf), the report highlighted that the `PREDICATE_ROLE` can mint an unlimited number of tokens. In response, the project team stated that `PREDICATE_ROLE` is completely decentralized and held by the governance contract of BitTorrent Chain. BitTorrent Chain is governed by [12 validators](https://app.bt.io/staking). At least some of the validators are either controlled (e.g. Huobi) or tightly connected (e.g. Binance) to Justin Sun.

The [TRX Burning Contract](https://usdd.io/MultiSigFundRaiser.pdf) also passed the audit without any significant issues. The auditors concluded: “_This is the MultiSigFundRaiser contract that contains the BaseMultiSigWallet section. MultiSig users can redeem their TRX tokens to mint and issue USDD. SafeMath security module is used, which is a recommended approach. The contract does not have the Overflow and the Race Conditions issue._” 

Further, Slowmist looked at the [Issuance Contract](https://usdd.io/MultiSigLocker.pdf), the [Authorized Contract](https://usdd.io/MultiSigAuthorizer.pdf), and the [PSM](https://usdd.io/SlowMistAuditReport-PSM.pdf). While the first two did not reveal any concerns. The audit report on the PSM module found five suggestion vulnerabilities. Three of them were fixed while two were ignored.


## Depeg Risk

Since it was launched, USDD has struggled to keep its dollar peg. The reason for this can be summarized by one word: “timing”. As mentioned, USDD was released shortly before the UST crash, initially as a copy of Terra UST and with a peg design based on a dual-token model. In the aftermath of Terra’s crash, however, many algorithmic stablecoins were struggling. USDD faced additional tribulations in the wake of the Alameda Research collapse - who was still part of the TDR.

Despite USDD switching its stability mechanism from an algorithmic to an over-collateralized model, it was still hit hard after Terra’s downfall. The figure below shows the volatility of USDD over the past 12 months.


![tradingview-usdd-depeg](https://user-images.githubusercontent.com/89845409/220380962-69913c0b-4172-40d5-932d-ed9346591f56.png)
(source: [TradingView](https://www.tradingview.com/symbols/USDDUSD/?exchange=CRYPTO))

There are several possible contributors to USDD’s historical struggle with maintaining its peg. The most named reasons are:


* (1) The UST crash destroyed confidence in algorithmic stablecoins, especially anything associated with UST.
* (2) In the aftermath of the UST crash, FTX and Alameda Research experienced a liquidity crunch. Justin Sun [claimed](https://coinmarketcap.com/community/articles/637615e238e8b54c28f2a219/) that the depeg was caused by Alameda, as they sold USDD to cover liquidity at FTX. On-chain data shows that two whales caused a depeg by selling large amounts of USDD for USDT and USDC via the Curve [USDD-3CRV](https://curve.fi/#/ethereum/pools/factory-v2-116/deposit) metapool. This led to an imbalance (82.27% USDD share in the pool). The imbalance [continued](https://www.coindesk.com/markets/2022/12/12/trons-usdd-stablecoin-hits-lowest-since-june/) for over three months (The pool balance has since recovered).
* (3) The USDD team and the TDR appear unwilling to sell off TRX collateral or any reserves in an attempt to re-establish the peg. Instead, they continue to fill the reserves, attempting to bolster confidence. To make an impact, the TDR would need to allocate funds directly, e.g. depositing into the Curve pool, market buying USDD with its collateral, or putting USDC or USDT into the PSM module.

USDD traded below peg on Huobi for almost four months. The USDD price dipped again to ~$0.97 on January 6, 2023 as Huobi [announced](https://www.reuters.com/technology/crypto-exchange-huobi-lay-off-20-staff-justin-sun-2023-01-06/?utm_campaign=Research%20newsletter&utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-90Ssplxp4ox-c6QnYXD77LRkl03TkMD4VWmGvDL6pP89KqfnSqpOuDVmCSbPoqpAbQ1SYm)  that it was laying off 20% of its staff. That same day, Justin Sun [transferred](https://www.bloomberg.com/news/articles/2023-01-06/sun-moves-100-million-of-crypto-to-support-his-exchange-huobi?utm_campaign=Research%20newsletter&utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz-90Ssplxp4ox-c6QnYXD77LRkl03TkMD4VWmGvDL6pP89KqfnSqpOuDVmCSbPoqpAbQ1SYm) $100M in USDC and USDT from Binance to Huobi in a show of confidence.

Despite Justin Sun’s battle to support the peg, Tron’s stablecoin has had a hard time recovering from its bumpy start. One of the main problems is that the stability mechanisms are governed by the TDR members.  On the other hand, by taking action to defend the peg, the TDR would indirectly admit that USDD is dependent on its manual interventions. It would also put their treasury at risk, whereas nothing can happen to their collateral while in the TDR-controlled contracts.


## Custody Risk

All USDD reserve wallets, including the [TRX “Burning” contract](https://tronscan.org/#/contract/TNMcQVGPzqH9ZfMCSY4PNrukevtDgp24dK/code), are under the control of a 5-of-7 multi-sig wallet. The signers are the seven TDR members, all institutional crypto companies. The TRON DAO Reserve consists of the following [members](https://tdr.org/#/):



* [Amber Group](https://www.ambergroup.io/) - a crypto finance service provider that facilitates liquidity provision, trading, and asset management services.
* [Poloniex](https://poloniex.com/) - a centralized cryptocurrency exchange based in the United States. Justin Sun, the founder of the Tron network, is invested in [Poloniex](https://www.coindesk.com/markets/2019/11/12/despite-denials-tron-founder-confirms-investment-in-poloniex-crypto-exchange/).
* [Ankr](https://www.ankr.com/) - a decentralized web3 infrastructure provider that helps developers, decentralized applications, and stakers to interact easily with an array of blockchains.
* [Mirana](https://mirana.xyz/) - a venture, trading, and asset management firm.
* [Multichain](https://multichain.org/) - a cross-chain router protocol, that provides the infrastructure for arbitrary cross-chain interactions [see our previous [article](https://cryptorisks.substack.com/p/cross-chain-gauges-and-ecosystem) on Multichain].
* [FalconX](https://falconx.io/) - an institutional crypto trading platform built for larger financial institutions.
* [TPS Capital](https://www.tpscap.com/) - a trading company that develops and deploys its proprietary trading algorithms to take advantage of arbitrage opportunities.

Honorable mention: Alameda Research was one of the first members to manage the Tron DAO Reserves. After its collapse, however, its TDR membership was revoked.


![Nasdaq-tdr-alameda-member](https://user-images.githubusercontent.com/89845409/220381058-a045c942-1387-42bc-93b7-c88c0c044f7d.png)


(source: [NASDAQ article](https://www.nasdaq.com/press-release/more-whitelisted-members-wider-dex-integrations-higher-total-supply-usdd-thrives))


## Collateral Risk

In the current state, the only “official collateral” is TRX deposited into the BurnContract. This accounts for a collateralization ratio of about 85%. Although the Tron DAO Reserve website promotes that USDD is additionally backed by BTC, USDC, and USDT, the reality is a bit more complicated.

All accounts that contain USDD’s collateral and reserves are controlled by a 5-of-7 multi-sig. Hence, USDD holders must fully trust the TDR to react adequately in the event of a significant depeg (see next section). In November 2022, the TDR [announced](https://twitter.com/trondaoreserve/status/1590691862813478912?s=20&t=Cqz8wokUlnQmmQT4GfqRGQ) that it will purchase $1B USDT to “safeguard the crypto market”. This was done by using USDC from the USDD reserves. According to the announcement, the funds were moved to a centralized exchange, making it more difficult to follow what the funds were used for. However, as seen in the image below, reserves backing USDD shrunk drastically in Q4, 2022.


![Messari-reserve-withdrawal](https://user-images.githubusercontent.com/89845409/220381134-92a76b47-0bc2-4a17-95b9-cca9879091a2.png)


(source: [Messari](https://messari.io/report/state-of-usdd-q4-2022?referrer=all-research))

The BTC reserves are also under the control of the TDR members. In theory, those reserves will be used in case USDD depegs. However, this would require swift coordination of the five signers. It’s unclear how effective this mechanism is in case of emergency. Further, USDC and USDT reserves are rehypothecated as deposits on JustLend, which carries an additional risk.


## Governance Risk

USDD is not controlled via decentralized governance. The stability mechanisms not even permissionless. As eluded above, the TDR fully controls the issuance of USDD.

Everything else is handled by an unknown team. There’s no public information that points to the team behind USDD. Contacting them via Discord and email did not result in further insights. Hence, it can only be assumed that the team consists of Tron developers. And given Justin Suns’ communication, it is very likely that he’s also deeply involved.

In summary, USDD's well-being is fully dependent on an anon team, Justin Sun, and the seven TDR members. Plus the 27 [super representatives](https://developers.tron.network/docs/super-representatives) that make up the Tron validators (i.e. block producers of the Tron blockchain).


# Conclusion

After an initial growth phase, adoption has slowed down, and USDD is still struggling with problems from its bumpy start.

USDD does not inspire confidence in terms of its stability mechanisms. Large sums of money are transferred, without notice, followed by a [Twitter](https://twitter.com/trondaoreserve) announcement stating that it was done to “protect the crypto market”. However, most measures don’t directly impact the price. The strategy seems to revolve around making grand, symbolic gestures that assuage market participants, rather than directly expending the TDR's own reserves (e.g. BTC, USDT, USDC).

There are no attempts to decentralize any components. After over nine months of existence, the project is still centrally steered by an unknown team. There are no decentralized and permissionless elements, despite marketing claims to the contrary.

On a positive note, from a technical perspective, it appears that the team takes security seriously. The project underwent relevant measures to reduce the risks of a hack. All audit reports are public and there were no issues of high severity found.

In summary, potential concerns regarding USDD are mostly based on governance rather than technical issues. A long list of red flags include the intransparent project operations, anon team, control by a whitelisted group of institutional insiders, false advertising, non-diversified collateral, and 6-month long struggle with the peg.


# Resources

General



* [USDD website](https://usdd.io/) | [Docs](https://docs.usdd.io/) | [Blog](https://medium.com/@usddio) | [Coingecko](https://www.coingecko.com/en/coins/usdd) | [USDD Twitter](https://twitter.com/usddio) | [TDR Twitter](https://twitter.com/trondaoreserve)
* [TronDAO Reserve](https://tdr.org/#/)
* [USDD Peg Stability Module](https://usdd.io/#/psm)

Audits



* [TRX burning contract audit by SlowMist](https://tdr.org/MultiSigFundRaiser.pdf)
* [Issuance contract audit by SlowMist](https://tdr.org/MultiSigLocker.pdf)
* [Authorized contract](https://tdr.org/MultiSigAuthorizer.pdf) [audit by SlowMist](https://tdr.org/MultiSigAuthorizer.pdf)
* [PSM contract audit by SlowMist](https://tdr.org/SlowMistAuditReport-PSM.pdf)
* [SlowMist SC audit (TRC20)](https://usdd.io/SlowMistAuditReport-USDDTRC20.pdf)
* [SlowMist SC audit (ERC20)](https://usdd.io/SlowMistAuditReport-USDD.pdf)

Tron DAO Reserve multi-sig signers:



* [TLntW9Z59LYY5KEi9cmwk3PKjQga828ird](https://tronscan.org/#/address/TLntW9Z59LYY5KEi9cmwk3PKjQga828ird)
* [TPcnRbpuB89eKKkfH2E7iJooAqeEZPpdGb](https://tronscan.org/#/address/TPcnRbpuB89eKKkfH2E7iJooAqeEZPpdGb)
* [THdUXD3mZqT5aMnPQMtBSJX9ANGjaeUwQK](https://tronscan.org/#/address/THdUXD3mZqT5aMnPQMtBSJX9ANGjaeUwQK)
* [TBEp1Gndqp88SUWnS4NQH5CAMhduHgpoNY](https://tronscan.org/#/address/TBEp1Gndqp88SUWnS4NQH5CAMhduHgpoNY)
* [TXZafZLKKgcXWTAq4YpLp3oCD4T9smsgmV](https://tronscan.org/#/address/TXZafZLKKgcXWTAq4YpLp3oCD4T9smsgmV)
* [TXCu9ivZfybabh7r3aSmDPfLH6YybLjuvX](https://tronscan.org/#/address/TXCu9ivZfybabh7r3aSmDPfLH6YybLjuvX)
* [TA1FNwAaekqAULEZvkhAKzxvKVSL9fuQfZ](https://tronscan.org/#/address/TA1FNwAaekqAULEZvkhAKzxvKVSL9fuQfZ)

Contracts - USDD on other chains:



* [USDD on Ethereum](https://etherscan.io/token/0x0C10bF8FcB7Bf5412187A595ab97a3609160b5c6)
* [USDD on Tron](https://tronscan.org/#/token20/TPYmHEhy5n8TCEfYGqW2rPxsghSfzghPDn)
* [USDD on BSC](https://bscscan.com/token/0xd17479997f34dd9156deef8f95a52d81d265be9c)
* [USDD on Polygon](https://polygonscan.com/token/0xffa4d863c96e743a2e1513824ea006b8d0353c57)
* [USDD on Fantom](https://ftmscan.com/token/0xcf799767d366d789e8b446981c2d578e241fa25c)
* [USDD on Avalanche](https://snowtrace.io/token/0xcf799767d366d789e8b446981c2d578e241fa25c)
* [USDD on Aurora](https://explorer.aurora.dev/token/0x941b45cb782CcEd9814821E9b684261fa353BCD5/token-transfers)

Articles



* [Nasdaq](https://www.nasdaq.com/press-release/more-whitelisted-members-wider-dex-integrations-higher-total-supply-usdd-thrives)
* [Messari Q4 Report](https://messari.io/report/state-of-usdd-q4-2022?referrer=all-research)
* [Coinmarketcap](https://coinmarketcap.com/community/articles/637615e238e8b54c28f2a219/)
* [StakingBits post](https://stakingbits.medium.com/usdd-another-unstable-stablecoin-89dcf996afa8)
* [Cryptosy post](https://medium.com/@Cryptosy1/usdd-loses-dollar-peg-but-these-altcoins-are-holding-their-ground-3a8147a28692?source=search_post---------3----------------------------)
* [Crypto saving expert post](https://medium.com/@CryptoSavingExpert/tron-daos-usdd-inches-closer-to-reclaiming-its-1-peg-e497c52f3bc2?source=search_post---------5----------------------------)
* [Poloniex post - what is USDD?](https://medium.com/poloniex/what-is-usdd-f39ed4e75d14?source=search_post---------6----------------------------)
* [Web3 creator post](https://medium.com/@jacobvan/great-look-into-usdd-956ca1873c?source=search_post---------13----------------------------)
* [Defi educator post](https://medium.com/@DeFi_educator/30-apy-would-be-the-reason-to-use-it-75f3bd13e77?source=search_post---------18----------------------------)
* [AlphaPro post](https://medium.com/coinmonks/defi-insight-is-justin-suns-usdd-stablecoin-a-copycat-f0d4c483561e?source=search_post---------31----------------------------)
* [Coinmonks post](https://medium.com/coinmonks/usdd-a-decentralized-stablecoin-for-the-future-or-will-it-head-towards-an-inevitable-death-c40f048e6272?source=search_post---------32----------------------------)
* [Observers post](https://medium.com/@observer1/there-is-trust-in-stablecoins-active-development-of-tron-usd-e77b4e54e75?source=search_post---------33----------------------------)
* [Observers 2nd post](https://medium.com/@observer1/husd-and-usdd-lose-their-peg-7881587cb590?source=search_post---------35----------------------------)
* [BYDFi post](https://medium.com/@BYDFi/usdd-stablecoin-all-you-need-to-know-facc5ff9ab2d?source=search_post---------39----------------------------)
* [JF USDD post](https://medium.com/@John_Funderburker/great-article-on-usdd-e8c467fadac4?source=search_post---------40----------------------------)
* [DRIP strategy post](https://medium.com/@dripstrategy/so-basically-we-can-just-borrow-this-crap-swap-it-for-usdc-then-repay-in-worthless-usdd-if-the-f581e6d41914?source=search_post---------46----------------------------)
* [Messari state of USDD](https://messari.io/report/state-of-usdd-q4-2022?referrer=all-research)
* [ITB post](https://www.theblock.co/post/193990/tron-backed-usdd-loses-dollar-parity-as-stablecoin-dips-below-0-97)
* [CryptoCoinReport post](https://medium.com/@thecryptocoinreport/ustx-warp-how-to-bend-space-and-time-to-offer-30-apr-on-usdd-staking-bc2b61367686?source=search_post---------54----------------------------)
* [Observer 3rd post - about USDD launch](https://medium.com/@observer1/trons-stablecoin-launch-seems-to-be-a-big-deal-c9b2f68f81ce?source=search_post---------57----------------------------)
* [CNBC article](https://www.cnbc.com/2022/06/22/usdd-explained-why-another-stablecoin-losing-its-peg-isnt-terra-2point0.html)
* [Bloomberg article](https://www.bloomberg.com/news/articles/2022-06-05/tron-modifies-usdd-stablecoin-to-avoid-woes-of-terrausd)
* [Decrypt article](https://decrypt.co/113307/justin-sun-trons-usdd-is-taking-stablecoins-to-the-next-level)
* [Decrypt article 2](https://decrypt.co/116952/trons-justin-sun-deploying-more-capital-arrest-stablecoin-usdds-slide)
* [DailyCoin - about USDD](https://dailycoin.com/exclusive-is-justin-suns-tron-next-ftx)
* [Coindesk post](https://www.coindesk.com/markets/2022/05/14/justin-sun-talks-usdd-stablecoin-in-wake-of-lunaust-unravel/)
* [Cryptopotato post - USDD and Huobi relation](https://cryptopotato.com/huobis-market-share-crumbles-amid-usdd-depeg/)
* [Article USDD](https://www.linkedin.com/pulse/what-trons-usdd-alex-kind/)
