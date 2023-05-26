Asset Risk Assessment - Membrane Finance
===============
### A risk assessment of e-money, the Membrane (EUROe) stablecoin, and its risks for Curve LPs
 
This research was spearheaded by [@evmknows](https://twitter.com/evmknows).
 
## Useful links
 
 * [Membrane Finance Homepage](https://www.membrane.fi/) / [Twitter](https://twitter.com/euroemoney)
 * [EUROe Homepage](https://www.euroe.com/)
 * [Dev Docs](https://dev.euroe.com/)
 * [Audits](https://dev.euroe.com/docs/Stablecoin/audits)
 * [Github](https://github.com/membranefi/euroe-stablecoin)  
 * [Legal & Privacy](https://www.euroe.com/legal)
 * [Transparency & Regulation](https://www.euroe.com/transparency-and-regulation)
 * [FAQ](https://www.euroe.com/faq)
 * [EUROe contract](https://etherscan.io/token/0x820802fa8a99901f52e39acd21177b0be6ee2974)
 * [Contract addresses](https://dev.euroe.com/docs/Stablecoin/contract-addresses)
 * [Peckshield Audit](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-ERC20-eEURO-v1.0.pdf)
 * [Runtime Verification Audit](https://github.com/runtimeverification/publications/blob/main/reports/smart-contracts/Euroe%20Stablecoin%20Audit.pdf)
 * [Curve Governance Proposal](https://gov.curve.fi/t/proposal-to-add-ageur-euroe-on-ethereum-to-the-gauge-controller/8894)

## Relation to Curve

A [gauge proposal](https://gov.curve.fi/t/proposal-to-add-ageur-euroe-on-ethereum-to-the-gauge-controller/8894) was submitted in February 2023 for the [agEUR/EUROe](https://curve.fi/#/ethereum/pools/factory-v2-273/deposit) pool on Ethereum. The pool seeks to combine fiat on/off-ramp capability (EUROe) with a permissionless euro token which has a number of DeFi integrations and arbitrage routes (agEUR). The proposal has not yet gone to a vote. 

## Abstract

For readers interested in the regulatory details of e-money, a report by Monerium on the asset risk assessment of EURe and its compliance with e-money regulations is available for review below:

TL;DR of our findings:
- EUROe is a centralized stablecoin issued by Membrane Finance under the e-money regime.
- Membrane Finance is an authorized electronic money institution in Finland.
- Membrane Finance is prepared for upcoming Markets in Crypto Assets Regulation (MiCA) requirements.
- Reserves for EUROe are held at Bank Frick Liechtenstein and Osuuspankki Finland.
- EUROe's smart contracts have gone through two audits by PeckShield and Runtime Verification.
- The operational security measures for EUROe are on par with industry standards.

## EUROe / Membrane Finance

The EUROe is a centralized stablecoin issued by [Membrane Finance](https://www.membrane.fi/), a subsidiary of [Equilibrium Group](https://equilibrium.co/) - a blockchain development organization. Like EURe (Monerium), it is currently issued under the e-money regime. Membrane Finance is currently only onboarding institutions to directly mint and burn EUROe, as they do not initially wish to deal with high-volume KYC. However, natural persons can still acquire EUROe through other channels like DeFi, brokers, OTC, or centralized exchanges. As of writing, about 1.12m EUROe is issued across four chains, ETH Mainnet, Arbitrum, Polygon and Avalanche.

### How does it work?

![Mint and Burn Flow](https://dev.euroe.com/assets/images/mint-burn-flow-aaa798af286b07da46977c37cd4b9cef.png)([Source](https://dev.euroe.com/docs/Stablecoin/mint-and-burn-flow))

The mint and burn mechanism works similarly to those of other centralized stablecoins, however with the distinction that the corporate clients may utilize a personal IBAN.

### Regulatory & Legal

Membrane Finance is an authorised electronic money institution, granted authorization to issue electronic money according to Finnish legislation implementing Directive 2009/110/EC. Membrane Finance Oy provides its services online but its registered office is at Meritullinkatu 1, 00170 Helsinki, Finland (trade register No. 3236886-2). The company is supervised by the Finnish Financial Supervisory Authority.

Additional [regulatory requirements](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593) ([Markets in Crypto Assets Regulation - MiCA](https://www.sidley.com/en/insights/newsupdates/2022/11/how-will-the-eu-markets-in-crypto-assets-regulation-affect-crypto-and-other-financial-services-firms)) are currently in the works which would impose further obligations on fiat-referenced tokens (e-money tokens). According to MiCA, such tokens should not only adhere to the EMD but also adhere to requirements such as [diligent marketing](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Article%2048%0A%C2%A0%C2%A0%C2%A0Marketing%20communications), [disclosure of risks](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=the%20risks%20relating%20to%20the%20issuer%20of%20e%2Dmoney%20issuer%2C%20the%20e%2Dmoney%20tokens%20and%20the%20implementation%20of%20the%20project%2C%20including%20the%20technology%3B), [prohibition of interest](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Prohibition%20of%20interests), and more. Currently, MiCA is not yet in force, however, this is expected to happen [sometime around Q1 2023, after which it will come into effect in 2024](https://www.iflr.com/article/2a647jipe3beilngwqlmo/primer-markets-in-crypto-assets-regulation-mica#:~:text=The%20European%20Council%20published%20the,the%20requirements%20of%20the%20regulation.) when implemented by the member states of the EU. In this regard, Membrane is prepared to be compliant with future regulations by issuing their euro stablecoin under the EMD.

Like other centralized stablecoins, Membrane Finance need to comply with laws and regulations and hence reserves the right to block individual EOAs or contracts. Membrane will not deny access to Accounts except for when:

* Receiving a request to do so from a competent government body, authority, or supervisory agency;
* Receiving a request to do so from the owner & controller of an address, given sufficient proof; or
* Membrane deems denying access necessary to comply with a law, regulation, or legal order from a recognised Finnish or EU authority.

### Reserves management

In compliance with the European e-money regulations, the reserves backing e-money must be carefully managed by an Electronic Money Institution (EMI). Key requirements include maintaining a minimum capital of €350,000 or a 2% buffer based on average outstanding e-money (whichever is greater), segregating the EMI's own funds from the reserves, and safeguarding the reserves by investing in secure, low-risk assets.

The reserves can be held in a segregated bank account or invested into secure, low-risk assets such as debt securities issued or guaranteed by central governments, central banks, international organisations, multilateral development banks, or regional or local authorities within Member States. In addition, investment in debt securities issued by AAA to A-rated financial institutions or corporates are also permissible. Furthermore, they can be covered by an insurance policy or some other comparable guarantee, providing an additional layer of security.

As an alternative, the EMI can invest in Undertakings for Collective Investment in Transferable Securities (UCITS) funds that solely invest in the assets specified above. These funds, managed by entities like BlackRock, offer a means of "outsourcing" the operations of reserve management. For a more detailed overview of the reserve requirements imposed on e-money issuers, please refer to the [e-money section of our other review](https://cryptorisks.substack.com/p/asset-risk-assessment-monerium).

As per the [latest reserve attestation](https://assets.ctfassets.net/pfn8eh7nfdk1/3wf9ISddP2wD48H1iBPApn/dbb211d81ba4192964be833e4e6cb336/2023-04_Reserve_Attestation.pdf) the reserves are held in cash at two different banks domiciled in the European Economic Area, [Bank Frick (Liechtenstein)](https://www.bankfrick.li/en) and [Osuuspankki (Finland)](https://www.op.fi/).

#### Security & Control mechanisms

Membrane employs an array of control mechanisms to maintain its operations, enforce security, and respond to emergencies. These mechanisms are dictated by a series of defined roles, each granted different access levels to the EUROe stablecoin smart contracts. Key roles include the PROXYOWNER_ROLE, responsible for contract upgrades; the BLOCKLISTER_ROLE, which can assign and remove blocked roles; and the MINTER_ROLE, the sole role with minting privileges. Emergency roles such as PAUSER_ROLE and UNPAUSER_ROLE exist for critical situations, and a DEFAULT_ADMIN_ROLE oversees all other roles.

| Role                          | Description                                                                                                                                                             |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PROXYOWNER_ROLE               | Manages the proxy contract / contract upgrades.                                                                                                                         |
| BLOCKLISTER_ROLE              | May assign and remove the BLOCKED_ROLE to addresses. The role can also: a) block the 0 address, effectively disabling mint & burn; and b) unblock the contract address. |
| MINTER_ROLE                   | May mint new EUROe stablecoins using mint() or mintSet(). The only role with minting privileges.                                                                        |
| PAUSER_ROLE and UNPAUSER_ROLE | Controls pausing and unpausing of the stablecoin contract.                                                                                                              |
| DEFAULT_ADMIN_ROLE            | Administers other roles.                                                                                                                                                |
| RESCUER_ROLE                  | This address may call rescueERC20() to move misplaced ERC20 tokens from the smart contract address to another.                                                          |
| BURNER_ROLE                   | May burn EUROe stablecoins using burn(), burnFrom() and burnFromWithPermit() functions (subject to conditions defined in the smart contracts).                          |
| RESCUER_ROLE                  | This address may call rescueERC20() to move misplaced ERC20 tokens from the smart contract address to another.                                                          |
| DEFAULT_ADMIN_ROLE            | Role that administers other roles.                                                                                                                                      |
Role holder addresses of the respective role for each chain can be found [here](https://dev.euroe.com/docs/Stablecoin/contract-addresses)

EUROe's smart contracts are managed in-house with security protocols to restrict unauthorized access to critical roles. As part of those security measures, Membrane utilizes Multi-Party Computation (MPC) technology enabled by Fireblocks. The contracts are designed to be upgradeable for potential enhancements and modifications. While there are no timelocks at the smart contract level for quick adaptability, EUROe may use timelocks within its internal operations for added security.

Emergency procedures are outlined, with BLOCKED_ROLE assignment being the most common response to emergencies or black swan events. The extreme measure of pausing the EUROe stablecoin is reserved for imminent threats. Governance of EUROe is held entirely by Membrane Finance, with no on-chain governance or voting mechanisms present at this stage.

#### External dependencies

For operational efficiency, Membrane utilizes MPC for contract interactions. Since this service is provided by an external party with the MP computations being done off-chain there is no way to publicly verify and evaluate the robustness of this system. However, it is widely known that Fireblocks belongs to the set of the biggest infrastructure providers in the crypto industry. [Fireblocks implements multiple layers of security](https://www.fireblocks.com/blog/how-we-meet-institutional-standards-insurance-compliance-security/), with regular penetration testing conducted by third-party firms, ComSec and NCC Group, to identify and eliminate vulnerabilities. In addition, Fireblocks has received SOC 2 Type II Certification from Ernst & Young.

#### Smart contract audits

The EUROe Stablecoin smart contracts have been audited twice. The first audit, conducted by PeckShield in July 2022, identified one medium and one informational finding, both of which were addressed. The contracts were then updated and renamed from "eEURO Token" to "EUROe Stablecoin." The second audit, by Runtime Verification in December 2022, resulted in one high severity and four informational findings, all of which were addressed as well.

The full audit reports can be found on [PeckShield's GitHub](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-ERC20-eEURO-v1.0.pdf) and [Runtime Verification's GitHub](https://github.com/runtimeverification/publications/blob/main/reports/smart-contracts/Euroe%20Stablecoin%20Audit.pdf). A detailed rundown of [ commits and addressed findings can be found here.](https://dev.euroe.com/docs/Stablecoin/audits)

## Conclusion

In conclusion, Membrane Finance operates under a robust regulatory framework, adhering to Finnish legislation and prepared for the impending MiCA requirements. The company demonstrates transparency in its reserve management, with reserves held in two established banks within the EEA. The operational security measures are also on par with those of current incumbents.

The fact that EUROe is currently not available for retail clients does not pose much of an issue when it comes to assessing the risk of pool squatting. As long as EUROe is paired with another freely available asset with sufficient liquidity and balance, squatting is virtually impossible. Furthermore, it can be assumed that the presence of multiple (Membrane onboarded) market makers, or competition to be more precise, will naturally prevent this.

From a more general point of view, we believe that incentivizing EUROe pools could be a good step to enrich and diversify Euro liquidity across DeFi.
