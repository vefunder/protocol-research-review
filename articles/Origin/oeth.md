![](https://hackmd.io/_uploads/S1Fgg708n.png)

# Asset Risk Assessment: Origin Ether (OETH)

### A look at OETH: Maximizing ETH Liquid Staking Derivatives (LSDs) yield through DeFi incentives

## Useful Links

- Websites: [oeth.com](https://www.oeth.com) | [originprotocol.com](https://www.originprotocol.com)
- Documentation: [Gitbook](https://docs.oeth.com) | [Github](https://github.com/originprotocol/origin-dollar) | [Audits](https://github.com/OriginProtocol/security/tree/master/audits)
- Social: [Twitter](https://twitter.com/OriginProtocol) | [Blog](https://www.oeth.com/blog) | [Discord](https://discord.com/invite/ogn) | [Telegram](https://t.me/originprotocol)
- Contracts: [OETH token](https://etherscan.io/address/0x856c4Efb76C1D1AE02e20CEB03A2A6a08b0b8dC3), [Contracts architecture](https://docs.oeth.com/smart-contracts/registry/oeth-registry)
- Governance: [Proposals](https://governance.ousd.com/proposals) | [OGV token](https://etherscan.io/token/0x9c354503c38481a7a7a51629142963f98ecc12d0) | [Timelock](https://etherscan.io/address/0x72426BA137DEC62657306b12B1E869d43FeC6eC7) | [Multi-sig (admin)](https://etherscan.io/address/0xbe2AB3d3d8F6a32b96414ebbd865dBD276d3d899) | [Multi-sig (strategist)](https://etherscan.io/address/0xF14BBdf064E3F67f51cd9BD646aE3716aD938FDC)
- Curve: [OETH/ETH Factory Pool](https://curve.fi/#/ethereum/pools/factory-v2-298/swap) | [Gauge Proposal](https://gov.curve.fi/t/proposal-to-add-eth-oeth-to-the-gauge-controller/9188)
- Dashboard: [OETH (Dune)](https://dune.com/rugolini/oeth)

# Introduction

In May 2023, the Origin Protocol launched Origin Ether (OETH). This rebasing token dynamically combines ETH Liquid Staking Derivatives (LSDs), specifically Rocket Pool ETH (rETH), Lido Staked ETH (stETH), and Frax Ether (frxETH). Leveraging the OUSD vault codebase, a stablecoin launched in 2020 and reached $300M in TVL at its peak, OETH incorporates yield-generating collateral tokens using price oracles from Chainlink. The system optimizes APYs between LSDs and DeFi liquidity provision strategies. It uses its protocol-owned liquidity (POL) and automated market operations (AMO) to boost yield generation through the OETH-ETH Curve pool. Ultimately, Origin aims to provide OETH holders with diversified risk exposure, augmented by trading fees and rewards tokens, in addition to validator rewards from the LSDs.

## TLDR of our findings

- Yield accrual is done through token rebasing, sourced from Liquid Staking Derivatives (LSDs), DeFi strategies, and protocol fees. The allocation of assets between LSDs and strategies is handled manually via permissionned functions.
- Origin utilizes an automated market operation (AMO) system, modeled after Frax, to sustain the peg, enhance capital efficiency, and optimize yields for OETH holders. The protocol claims to remain fully collateralized even as the money supply programmatically expands and contracts based on market conditions.
- The contracts' access controls are managed by 5-of-8 (admin) and 2-of-8 (strategist) multi-sigs. The identity of these signers is not disclosed. The multi-sigs oversee vital functions, including fee setting, contract pausing, asset allocation, and strategy deployment. Origin has plans to transfer many of these responsibilities to governance token holders (OGV) in the future.
- OETH is safeguarded by timelock contracts (48h) but retains upgrade capabilities for urgent vulnerabilities through its admin multi-sig. Other operational functions are controlled by strategist multi-sig with no timelock.
- Origin draws a significant portion of OETH yield from Curve Gauge incentives, with over 75% of the OETH/ETH pool originating from Origin's protocol-owned liquidity (POL).

# Relation to Curve

Origin Protocol [submitted a proposal](https://gov.curve.fi/t/proposal-to-add-eth-oeth-to-the-gauge-controller/9188) to add the [OETH-ETH pool](https://curve.fi/#/ethereum/pools/factory-v2-298) to the Curve Gauge Controller, allowing users to assign gauge weight and mint CRV. This proposal aims to enhance liquidity and usage of the pool and boost the yield offered to OETH holders. Profits generated from the gauge represent a substantial part of OETH yield. The proposal successfully passed an [on-chain vote on April 22nd, 2023](https://dao.curve.fi/vote/ownership/323).

![](https://hackmd.io/_uploads/ByBJHQRLh.png)
Gauge rewards are an important part of OETH yield ([OETH-ETH Pool](https://curve.fi/#/ethereum/pools/factory-v2-298) on June 7th, 2023)

# Origin Ether (OETH) Introduction

## History

In July 2021, the Origin Protocol introduced the Origin Dollar (OUSD), a yield-bearing stablecoin that did not require staking or asset locking. OUSD has a total market cap of approximately $27m, down from an all-time high of $300m in early 2022 (pre-UST era). This was followed by the Origin Governance Token (OGV), fostering decentralization and inclusive decision-making applied to the OUSD token. In May 2023, the protocol expanded its offerings with Origin Ether (OETH), an interest-earning token linked to Ethereum's value, in an attempt to capitalize on the Liquid Staking Derivatives (LSDs) trend and to provide a hassle-free yield to its holders.

# Protocol Overview

## Protocol Mechanics

### Mint and burn

OETH can be minted through [Origin Dapp](https://app.oeth.com/) by supplying ETH, WETH, stETH, rETH, frxETH, or sfrxETH. The Dapp integrates several contracts (Curve, OETH vault, and OETH zapper) to find the optimal route, factoring in slippage and gas expenses. Rather than creating new OETH coins from the vault, the router acquires OETH already in circulation, for instance, from the Curve pool or through Uniswap. OETH can be converted back to its composite or individual assets via the Dapp. An exit fee of 0.5% is charged on withdrawal unless routed via the Curve pool (no fee aside from slippage) and returned to the OETH holders.

## AMO

Origin employs an Automated Market Operations (AMO) design [initially pioneered by Frax](https://docs.frax.finance/amo/overview). The AMO operates by depositing funds into the Curve pool, allocating liquidity to both sides of the pool (ETH and OETH). Its primary function is maintaining the peg, enhancing capital efficiency, and optimizing yields for OETH holders.

The LP tokens are staked into the [Curve Gauge](https://etherscan.io/address/0xd03be91b1932715709e18021734fcb91bb431715) to maximize earned rewards (CRV & CVX). The resulting collateral is added to the vault when these rewards are swapped to ETH. Conversely, the protocol can remove excess OETH from the pool to preserve price stability. Ultimately, the AMO can independently institute monetary policies within a closed system, provided they don't negatively impact the peg. The protocol claims to remain entirely collateralized even as the money supply responsively expands and contracts based on market conditions. This is because the protocol-owned OETH supplied in the Curve pool is not backed by LSDs or other DeFi strategies.

![](https://hackmd.io/_uploads/H1L8vmRL2.png)
Flowchart of deposit to Curve pool throught AMO ([Origin's github repo](https://github.com/OriginProtocol/origin-dollar/tree/master/contracts/docs/plantuml))

OETH tokens minted by the AMO are unique as they aren't backed by collateral from the vault. One could think of this system as the vault pre-minting some OETH for Curve to sell on its behalf, with those tokens becoming 100% backed the minute they enter circulation. These tokens are self-backed and are only circulated once collateralized. Users adding or removing OETH from the Curve pool influences the balance like a minting or redemption process due to the strategy's ability to burn or create new supply. OETH token can be redeemed at any time for underlying collateral on a 1:1 basis, ensuring the protocol remains 100% collateralized.

![](https://hackmd.io/_uploads/H1WG5XA83.png)
The AMO can mint up to 2x the amount of OETH ([ConvexEthMetaStrategy.sol](https://www.contractreader.io/contract/mainnet/0xA52C14701f7ad3E7B70D05078AE2ebE3Fd283449))


## Rebasing

OETH generates yield, which is systematically passed on to token holders by rebasing. This process adjusts the total money supply in alignment with the yield earned by the protocol. OETH dynamically modifies its money supply to ensure its value stays equivalent to 1 ETH. In tandem, the token balance in holders' wallets fluctuates in real time, mirroring the yield the protocol accrues. The rebasing is "up-only". In case of losses due to strategies reallocation (trading fees and slippage), rebasing stops until the yield catches up to pay off the debt. ==@todo: what happens in case of a hack (e.g., Morpho strat goes to zero)?==

For yield to be earned, smart contracts must proactively opt into the protocol via the `rebaseOptIn()` function. This enables optimized capital use by the protocol, enhancing the integration with DeFi protocols that weren't originally designed to handle changing balances. Therefore, OETH operates as a conventional ERC-20 token within other DeFi protocols until an explicit request for change is made. Standard EOA wallets, on the other hand, are automatically enrolled in the system, circumventing the need for an explicit opt-in. The OETH managed by the AMO does not rebase. 

To verify whether a specific address is configured to receive yield, a public function on the OETH contract can be invoked. This function returns an indicator showing if the address has opted in or out, irrespective of the type of wallet to which the address pertains.

![](https://hackmd.io/_uploads/SyeihXCI3.png)
Rebasing param ([OETH docs](https://docs.oeth.com/core-concepts/elastic-supply/rebasing-and-smart-contracts)) 

### Wrapped OETH

Origin also offers wOETH, a [ERC-4626](https://ethereum.org/en/developers/docs/standards/tokens/erc-4626/)compliant wrapped version of OETH, which appreciates while maintaining a fixed quantity. This feature makes wOETH compatible with other contracts and may provide tax advantages in certain regions instead of rebasing. Since the user already owns the wrapped version, unwrapping wOETH to OETH involves no approvals or constraints such as minimum term or lock-in period. Despite the static quantity of wOETH tokens, the equivalent OETH that can be unwrapped from wOETH progressively increases. The redeem function allows for the specification of the number of wrapped tokens to be unwrapped, with Etherscan's withdraw function as an alternative for specifying the amount of OETH to be retrieved. 

wOETH remains a marginal token with very few holders:

![](https://hackmd.io/_uploads/Sk0jRmAIn.png)
wOETH on June 7th, 2024 ([Etherscan](https://etherscan.io/token/0xDcEe70654261AF21C44c093C300eD3Bb97b78192))


### The Zapper

The OETH Zapper is a smart contract designed to assist users in minting OETH using Ether (ETH) and Frax Staked Ether (sfrxETH). This setup enhances system security and optimizes the gas cost during minting. The Zapper allows for minting OETH in a single transaction, automatically routing ETH transactions toward the contract that provides the best outcome. This can either be an exchange of ETH for OETH via the Curve protocol or depositing ETH into the Zapper, which, in turn, deposits WETH into the Vault to create OETH. Currently, minting OETH using sfrxETH can only be accomplished via the Zapper. 

### The Dripper

The dripper contract is designed to gradually allocate all of the yield produced by the protocol to OToken holders over one week. This method evens out any abrupt fluctuations in yield and deters potential attacks by eliminating the ability to anticipate significant liquidity events within the protocol.

## Markets

OETH primarily liquidity is found on Curve's [OETH-ETH pool](https://curve.fi/#/ethereum/pools/factory-v2-298), which boasts over 5,300 ETH worth of liquidity. On the contrary, the liquidity of OETH on Uniswap is relatively smaller, with approximately 100 ETH in the [Uniswap pool 0.05% (v3)](https://info.uniswap.org/#/pools/0x52299416c469843f4e0d54688099966a6c7d720f).

![](https://hackmd.io/_uploads/rJ_uyVA82.png)
Curve OETH-ETH pool on June 7th, 2023 ([Curve](https://curve.fi/#/ethereum/pools/factory-v2-298))

![](https://hackmd.io/_uploads/BkjuJV0I3.png)
Uniswap V3 OETH/ETH 0.05% pool on June 7th, 2023 ([Uniswap](https://info.uniswap.org/#/pools/0x52299416c469843f4e0d54688099966a6c7d720f))

### Peg

OETH aims to remain pegged to the value of Ethereum, allowing holders to have exposure to Ethereum's price movements while earning a stable yield. Since launch, OETH has maintained peg reasonably well.

### Chart from Coingecko:
![](https://hackmd.io/_uploads/ryr-z4R83.png)

### Chart from Curve on-chain data:
![](https://hackmd.io/_uploads/r1wWEN0L3.png)

In early June, the peg was jeopardized by a large holder (victim of the atomic wallet hack) selling into the Curve pool. The pool balance was restored as other buyers quickly seized the opportunity to buy OETH at a discount, returning the pool to peg.

![](https://hackmd.io/_uploads/S1yecECU3.png)
Large transaction bringing the pool balance back [Etherscan](https://etherscan.io/tx/0x365e6584f604a7ccab360f7631e05e0cfcb54b612c5f7b4f036b1aae192e6c83))

### Stablecoin/pegged asset

![](https://hackmd.io/_uploads/BkiglBALn.png)
Collateral on June 7th, 2023 ([oeth.com](https://www.oeth.com))

## Sources of Yield
![](https://hackmd.io/_uploads/B1PWWHR82.png)
Allocation of assets on June 7th, 2023 ([oeth.com](https://www.oeth.com))

### Curve/Convex Yield

Convex ETH+OETH: Origin predominantly supplies liquidity to the ETH-OETH Curve pool. The liquidity provider (LP) token is contributed to the gauge, then staked on Convex, enabling Origin to collect trading fees and protocol token rewards (CRV and CVX). This approach, an algorithmic market operations controller (AMO), allows OETH to safely boost its deposits to enhance returns and sustain the pool's balance.

![](https://hackmd.io/_uploads/Sy05yHAUn.png)
OETH allocation for Convex Gauge on June 7th, 2023 ([oeth.com](https://www.oeth.com/))

![](https://hackmd.io/_uploads/H1Cc1HA83.png)
Convex finance for ETH+OETH staking on June 7th, 2023 ([convexfinance.com](https://www.convexfinance.com/stake))

### Liquid Staking Derivatives (LSDs) Yield

1. **Staked Frax Ether (sfrxETH)**: Employing a dual-token model, Frax amplifies yields from staking validators. OETH deposits frxETH into the Frax Ether staking contract to magnify this yield further.
2. **Lido Staked Ether (stETH)**: As a liquid staking solution for Ethereum, Lido permits ETH staking without locking tokens or maintaining infrastructure. OETH holds stETH, earning staking rewards from the Ethereum network with the added advantage of automatic compounding.
3. **Rocket Pool Ether (rETH)**: As a community-owned, decentralized protocol, Rocket Pool mints rETH, an ETH wrapper that accrues interest. OETH holds rETH to earn yield and manages the accounting by distributing additional tokens directly to the users' wallets.

### Yield from DeFi Strategies

Other DeFi strategies, such as the Morpho AAVE WETH strategy, can power yield generation in the OETH protocol. Morpho augments platforms like Compound and Aave by integrating a peer-to-peer layer that optimizes the pairing of lenders and borrowers, thus offering superior interest rates. If no appropriate pairings exist, the funds are directed straight to the base protocol. 

### Unallocated Capital

When OETH is minted, collateral is placed into the Origin Vault, where it remains until the allocate function is invoked. This process is automatic for more significant transactions, which aren't as affected by increased gas costs.

# Governance

The Origin Protocol uses the Origin DeFi Governance Token (OGV) to allow decentralized decision-making within its ecosystem. Like OUSD, Origin Ether will ultimately be governed by veOGV holders, with any upgrades to the contracts being time-delayed by a 48-hour timelock. A 5-of-8 timelock will control OETH for the next few weeks if any critical issues are discovered. 

## Multisig:

The contracts' access controls are managed by 5-of-8 (admin) and 2-of-8 (strategist) multi-sigs. The identity of these signers is not disclosed. Origin claims they are all unique trusted individuals with close ties to Origin. 

**Signers (same for both)**:
* [0xAbBca8bA6d2142B6457185Bec33bBD1b22746231](https://etherscan.io/address/0xAbBca8bA6d2142B6457185Bec33bBD1b22746231)
* [0xce96ae6De784181d8Eb2639F1E347fD40b4fD403](https://etherscan.io/address/0xce96ae6De784181d8Eb2639F1E347fD40b4fD403)
* [0x336C02D3e3c759160E1E44fF0247f87F63086495](https://etherscan.io/address/0x336C02D3e3c759160E1E44fF0247f87F63086495)
* [0x6AC8d65Dc698aE07263E3A98Aa698C33060b4A13](https://etherscan.io/address/0x6AC8d65Dc698aE07263E3A98Aa698C33060b4A13)
* [0x617a3582bf134fe8eC600fF04A194604DcFB5Aab](https://etherscan.io/address/0x617a3582bf134fe8eC600fF04A194604DcFB5Aab)
* [0x244df059d103347a054487Da7f8D42d52Cb29A55](https://etherscan.io/address/0x244df059d103347a054487Da7f8D42d52Cb29A55)
* [0xab7C7E7ac51f70dd959f3541316dBd715773158B](https://etherscan.io/address/0xab7C7E7ac51f70dd959f3541316dBd715773158B)
* [0xe5888Ed7EB24C7884e821b4283472b49832E02f2](https://etherscan.io/address/0xe5888Ed7EB24C7884e821b4283472b49832E02f2)

**Admin (5 of 8)**: [0xbe2AB3d3d8F6a32b96414ebbd865dBD276d3d899](https://etherscan.io/address/0xbe2AB3d3d8F6a32b96414ebbd865dBD276d3d899)

![](https://hackmd.io/_uploads/HymuMBCLn.png)
Admin multi-sig permissions ([pod.xyz](https://pod.xyz/podarchy/0xbe2AB3d3d8F6a32b96414ebbd865dBD276d3d899?sidebar=0&selectedNode=%220xbe2AB3d3d8F6a32b96414ebbd865dBD276d3d899%22))

The admin multi-sig currently has full ownership over the contracts, but all contract changes must go through the timelock. This is merely temporary since OETH is less than a month old, and the team wanted to be able to respond quickly to any unforeseen issues. Full governance will soon be transferred to veOGV stakers like OUSD today. Once that transfer happens, the admin multi-sig will have no special powers. 

**Strategist (2 of 8)**:
[0xF14BBdf064E3F67f51cd9BD646aE3716aD938FDC](https://etherscan.io/address/0xF14BBdf064E3F67f51cd9BD646aE3716aD938FDC)

Certain features, like adjusting funds among strategies or temporarily stopping deposits, require less time and fewer authorizations, enabling the team associated with Origin to respond swiftly to changes in market circumstances or potential security issues. Strategists can carry out a restricted set of functions with the approval of only 2 of 8 authorized signers. The strategist can only allocate funds between previously approved strategies.

The strategist multi-sig can do the following actions on the vault:
* `reallocate` - move funds between strategies
* `setVaultBuffer` - adjust the funds held outside strategies for cheaper redeems.
* `setAssetDefaultStrategy` - which strategy mints and redeems pull from for a particular strategy
* `withdrawAllFromStrategy` - remove funds from a single strategy and send them to the vault
* `withdrawAllFromStrategies` - remove funds from all active strategies and send them to the vault
* `pauseRebase` - pause all rebases
* `pauseCapital` - pause all mints and redeems
* `unpauseCapital` - allow all mints and redeems

# Team

The core team at Origin Protocol comprises experienced professionals with backgrounds in various industries and companies, including Coinbase, YouTube, Google, Paypal, Dropbox, and Pinterest. The founders and several team members are serial entrepreneurs who have founded and exited successful ventures.

Here are brief profiles of the core team members:

* **Josh Fraser**: Co-founder, started coding at ten and has co-founded three other venture-backed companies: EventVue, Torbit (acquired by Walmart Labs), and Forage.
* **Matthew Liu**: Co-founder, was the 3rd Product Manager at YouTube (acquired by Google) and VP PM at Qwiki (acquired by Yahoo) and Bonobos (acquired by Walmart).
* **Franck Chastagnol**: Head of Engineering - has led engineering teams at Inktomi, Paypal, YouTube, Google, and Dropbox.
* **Micah Alcorn**: Director of Product, was the technical co-founder of WellAttended, a bootstrapped box office management platform.
* **Linus Chung**: VP of Product, previously worked as Director of Product at Coinbase and lead product at companies like Tesla, Pinterest, and LinkedIn.
* **Andra Nicolau**: Head of BD, OUSD, is a longtime crypto veteran. She was most recently the Head of Growth at 1inch, where she raised over $250M.
* **Justin Charlton**: Head of Finance, started investing in middle school, designed budgets for launching satellites into space, and advised tech clients on M&A and strategy for seven years at PwC and KPMG.

Source: https://www.originprotocol.com/community

# Smart Contracts

## Contract architecture

![](https://hackmd.io/_uploads/B1VXQBAI3.png)
Extract from [Contract Registry and Dependencies Chart](https://docs.oeth.com/smart-contracts/registry/oeth-registry)

### Oracles

OETH relies on Chainlink oracles to ensure appropriate Liquid Staking Tokens (LSTs) pricing to avoid overpayment. As of April 2023, Chainlink provides pricing oracles for stETH and rETH, while frxETH is hardcoded to a 1:1 ratio. Origin's team plans to implement Curve's time-weighted price oracle for frxETH.

The protocol also uses Chainlink oracles when selling reward tokens for additional yield. This helps ensure the sale price slippage remains within acceptable limits, which is also applied to OGV buybacks conducted using generated yield.

The contract call sets a minimum required amount for minting and redemption. This safeguards transactions from underperformance due to price changes, causing the process to fail if the return of OUSD/OETH or stablecoins/LSTs is insufficient.

**Oracles addresses**:
* stETH/ETH: [0x86392dc19c0b719886221c78ab11eb8cf5c52812](https://etherscan.io/address/0x86392dc19c0b719886221c78ab11eb8cf5c52812)
* rETH/ETH: [0x536218f9e9eb48863970252233c8f271f554c2d0](https://etherscan.io/address/0x536218f9e9eb48863970252233c8f271f554c2d0)
* CRV/ETH: [0x8a12be339b0cd1829b91adc01977caa5e9ac121e](https://etherscan.io/address/0x8a12be339b0cd1829b91adc01977caa5e9ac121e)
* CVX/ETH: [0xC9CbF687f43176B302F03f5e58470b77D07c61c6](https://etherscan.io/address/0xC9CbF687f43176B302F03f5e58470b77D07c61c6)

# Audits

A complete list of audits can be found here: https://github.com/OriginProtocol/security/tree/master/audits

Two notable audits were explicitly made on OETH: 
* [OpenZeppelin - Origin Dollar OETH Integration - May 2023](https://github.com/OriginProtocol/security/blob/master/audits/OpenZeppelin%20-%20Origin%20Dollar%20OETH%20Integration%20-%20May%202023.pdf)
* [Narya - Origin OETH Report - May 2023](https://github.com/OriginProtocol/security/blob/master/audits/Narya%20-%20Origin%20OETH%20Report%20-%20May%202023%20-%20Initial%20Report.pdf)

Only low-security findings, besides an Oracle issue (date feeds may be outdated), were brought up, which was corrected.

Other audits on protocols systems and related strategies can be found here: https://docs.oeth.com/security-and-risks/audits

# Bug Bounties

Bug bounties are granted at the full discretion of Origin Protocol. The rewards range from $100 OUSD for minor issues to $250,000 OUSD for major vulnerabilities. Currently, the bounty program only applies to OUSD and OETH and not other products from Origin. The bounty program is currently administered by Immunefi. As of early June 2023, Origin has paid out $154,850 in bounties and has one of the fastest response times on Immunefi. More details here: https://immunefi.com/bounty/origindefi/ 

## Risks

### Smart Contract Risk

Despite having undergone rigorous audits and being based on the OUSD code base, the OETH protocol encompasses many complex elements, increasing the potential smart contract risk. Additionally, the DeFi strategies employed within the system constantly evolve, with Origin capable of implementing or removing strategies as needed. This dynamic nature of the strategy adds another layer of complexity and potential risk. 

#### Risks and tradeoffs with the AMO system

Origin uses the AMO system to provide large amounts of liquidity at a lower cost to the protocol. Yet, it's crucial to remember the risk of bad debt. The system's ability to prevent bad debt from infiltrating largely hinges on the effectiveness of the AMO's interactions with the OETH/ETH Curve pool. If the overall impact of the AMO's transactions with the pool (i.e., deposits and withdrawals) results in positive slippage, they generate a profit and maintain full financial backing. Importantly, while the AMO funnels all yield farming revenue towards OETH, it also has the potential to generate additional profits or suffer losses through arbitrage within the pool.

Liquidity Providers (LPs) also face certain risks within this system. For instance, if a substantial amount of ETH is removed from the pool, as seen in early June, the ensuing imbalance can amplify the profit opportunity for the AMO through arbitrage. Conversely, bad debt can creep into the system if the AMO's transactions lead to an imbalance in the pool, thereby causing negative slippage. In such circumstances, the AMO's best strategy is to let the pool imbalance escalate. Eventually, they can capitalize on this situation by restoring balance to the pool while profiting from the losses sustained by those who withdrew their ETH.

### Custody Risk

The trust in the OETH system lies with the signers who hold the multi-sig keys. These signers are responsible for properly handling the assets within the system and must be trusted not to engage in actions such as infinite minting, which could destabilize the system. However, to provide a layer of security and trust, a 48-hour timelock mechanism is in place. This ensures that significant actions cannot be executed immediately, offering a window of time for potential issues to be identified and addressed. 

OETH also leverages DeFi platforms such as Aave, Compound, and Curve, introducing notable smart contract risks. While we collaborate with platforms managing billions in assets and conduct due diligence regarding their security, there is no absolute certainty of their continued flawless operation. Any malfunction in these underlying strategies could potentially result in a loss for OETH holders.

### Oracle Risk

A key concern regarding oracles is Origin using a hard-coded 1:1 ratio for frxETH/ETH. The implications are that the Strategists' multi-sig would need to manually pause deposits if frxETH were to experience a major de-pegging. Losses could be incurred if the multi-sig takes too long to react or if the de-peg perdures. ==@todo need to expand on this==

### Depeg Risk

There are several scenarios in which OETH could risk de-pegging from its intended value. These include significant additions or removals from the Curve pool, a strategy becoming loss-inducing, or an exploit related to the Automated Market Maker Operations (AMO). For instance, if a large holder were to sell into the Curve pool, it could temporarily destabilize the peg. However, the AMO is designed to restore the peg in such circumstances, and all funds remain secure during this process. Additionally, if the amount of ETH in the pool decreases, it becomes more profitable for the AMO strategy to increase its allocation to restore balance. This serves as a dynamic response mechanism to maintain the stability of the OETH value.

### Collateral Risk

OETH holders are exposed to the underlying strategies and Liquid Derivative Staking (LDS) tokens (e.g., slashing) deployed within the system, making risk assessment challenging. Given the intricate and evolving nature of these strategies and LDSs, understanding and quantifying the potential risk factors can be complex. Users must closely monitor the underlying assets and remain vigilant to system strategy changes. While the system is designed with robustness and security in mind, the complexity of its elements underscores the importance of cautious engagement and a thorough understanding of its operational mechanics.

The security of OETH collateral could significantly depend on the multi-sig's prompt response. For instance, on June 2nd, 2023, the strategist multi-sig temporarily halted funding to the newly launched Morpho strategy to address a potential issue with their interest rate model. It's important to note that there is no collateral buffer.

Origin also makes available risk analysis for some of their LSDs: 

**frxeth-sfrxeth**:
https://docs.oeth.com/security-and-risks/risk-reports/frxeth-sfrxeth

**reth**:
https://docs.oeth.com/security-and-risks/risk-reports/reth

### Governance Risk

A crucial aspect of this risk is associated with multi-signature wallets (multi-sigs). Multi-sigs can increase security if the key signing process is distributed and diversified across multiple participants. Including external entities in the list of signers can further amplify this security measure. However, a low threshold for signers, such as is the case for OETH, can present a risk, as it could potentially allow for the addition of a harmful strategy.

Manual allocation of funds is another potential governance risk. Currently, the capability to change the composition of the collateral has yet to be added. Consequently, any deposited ETH either goes into Convex or remains idle. 

In terms of future governance, additional strategies are anticipated, but these could introduce new risks—for instance, the recent inclusion of Morpho, which was promptly halted.

# LlamaRisk Gauge Criteria

## Centralization Factors
1. **Is it possible for a single entity to rug its users?** No, however, the Strategist and Admin multi-sigs have significant powers. Signers are not publicly known, and users must trust the admin multi-sig not to perform malicious actions, like infinite mint. 
2. **If the team vanishes, can the project continue?** The project is currently highly dependent on manual operations (e.g., rebalancing) performed by the multi-sigs.

## Economic Factors
1. **Does the project's viability depend on additional incentives?** The outsized yield offered by OETH highly depends on gauge incentives. The floor APY should be the yield generated by the LSDs under Origin's custody. The protocol-owned flywheel tokens ensure that there will always be extra rewards as long as the gauge is live and CRV & CVX remain valuable.
2. **If demand falls to 0 tomorrow, can all users be made whole?** Yes.

## Security Factors
1. **Do audits reveal any concerning signs?** No.

# Risk Team Recommendation
==@todo==



