Twitter Thread

1/ @InverseFinance is a DeFi protocol that has developed various financial tools, including a lending market and a stablecoin (DOLA).

We did a deep-dive to shed some light onto the complex inner workings of Inverse.

Here’s a summarizing thread:

LINK to Article

___

2/ DOLA is a hybrid collateralized/uncollateralized stablecoin.

It’s partially collateralized by deposits into Inverse's lending markets and partially issued by the protocol.

Inverse injects DOLA via their “Fed smart contracts” directly into various liquidity pools.

___

3/ DOLA’s main liquidity pools are on @CurveFinance and @Balancer.

Both LP-tokens can be staked on @ConvexFinance and @AuraFinance respectively. 

These pools serve as the main stability mechanism.

___

4/ Earlier this year, Inverse’s lending market Frontier was paused after two exploits.

The first exploit was over $15.6M and the second resulted in ~$5M in user funds being lost.

This left Inverse with significant bad debt.

___

5/ Since then, the collateral backing DOLA has dropped significantly.

As of today, there’s almost no exogenous collateral backing DOLA.

The peg is maintained through AMM pool management (DOLA expansion & contraction) via Fed contracts. 

___

6/ Since the exploits, Inverse has been working to reduce the resulting bad debt.

For instance, by selling bonds via an Olympus Pro integration.

Two DebtRepayment contracts were deployed to steadily repay affected users.

___

7/ The team just released a new product called FiRM. An improved version of Frontier.

FiRM comes with new features, increased security, and a new primitive for fixed-rate borrowing of DOLA.

It underwent a bug-bounty contest. But has not been audited by a reputable auditing firm yet.

___

8/ Despite these honorable efforts to reduce bad debt and repay affected users, our analysis unveils a rather fragile image when it comes to DOLA.

The stablecoin is heavily undercollateralized. Essentially it’s only “backed” by the counterparty assets in the respective pools.

___

9/ DOLA is fully reliant on active management by the Inverse team. Its supply needs to be actively managed via Feds.

And LP incentives are playing a key role for DOLA to keep its peg.

In other words, DOLA does not appear to be an organically sustainable stablecoin.

___

10/ For the complete assessment, our findings, and recommendation, read the full report here: LINK

This analysis was spearheaded by @Lavi_54 and @dabar_90. Give them a follow if you like what you read.
