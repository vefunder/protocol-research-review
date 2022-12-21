Asset Risk Assessment - Monerium
===============
### A risk assessment of e-money, the Monerium (EURe) stablecoin, and its risks for Curve LPs
 
This research was spearheaded by [@evmknows](https://twitter.com/evmknows).
 
## Useful links
 
 [Website](https://monerium.com/)  
 [Documentation](https://monerium.dev/)  
 [Github](https://github.com/monerium/)  
 [Gauge request](https://gov.curve.fi/t/proposal-to-add-ageur-eure-on-ethereum-to-the-gauge-controller/4544)
 
## Abstract

In this asset risk assessment, we will cover the EURe stablecoin issued by [Monerium](https://monerium.com/) prompted by [the request to deploy an agEUR/EURe gauge](https://gov.curve.fi/t/proposal-to-add-ageur-eure-on-ethereum-to-the-gauge-controller/4544). Since the stablecoin is issued under the strict regulatory requirements imposed by the [e-money directive](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110) we decided to elaborate on the concept of e-money and its regulations. If you are familiar with the latter and only interested in the Monerium review you can skip to the part by clicking here.

A quick TL;DR of our findings:

* Monerium offers a EURe stablecoin which can be on- and off-boarded using an [IBAN](https://en.wikipedia.org/wiki/International_Bank_Account_Number) (International Bank Account Number). When registering  with Monerium you get a personal IBAN that you can fund in order to receive EURe on your on-chain wallet. In the same way, you can off-board EURe by funding your on-chain wallet and sending the EURe via the Monerium app to an IBAN. By doing that the EURe is directly burnt from your wallet.
* Monerium is regulated as an electronic money institution (EMI) for the issuance of e-money.
* An EMI must meet a number of regulatory requirements, such as over-collateralization with at least €350k or (whichever is more) a 2%  based on the outstanding issued amount of e-money, segregating the EMIs funds and reserves by using different custodians and investing the reserves into secure, low-risk assets denominated in euro.
* We identified two centralization vectors in the Monerium smart contracts that allow infinite mints of EURe. We communicated our concerns to the team and were able to work out solutions to mitigate these issues. According to Monerium, these will be implemented with high urgency in the coming weeks.

## What is e-money?

In the context of European regulation ([e-money directive 2](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110)), e-money (electronic money) represents a blanket term for any "electronic store of value ... that can be widely used to make payments to entities other than the e-money issuer." ([ECB, 1998](https://www.ecb.europa.eu/pub/pdf/other/emoneyen.pdf)). Here, an "electronic store of monetary value on a technical device" can be understood as the digitization of an asset such as cash or bank deposits. To obtain an EMI license (electronic money institution), explicit approval by the European central bank (ECB) and a national financial services supervisory authority like the German BaFin e.g. is required. Furthermore, an EMI must meet a number of regulatory requirements to be allowed to issue e-money, these include:
- (Own funds) [A minimum capital contribution of €350,000](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110#d1e519-7-1) or (whichever is more) [a 2 % buffer based on the avg. outstanding e-money](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110#d1e529-7-1)
- [Fund segregation between the EMIs funds and the reserves](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex:32007L0064#:~:text=they%20shall%20be%20insulated%20in%20accordance%20with%20national%20law%20in%20the%20interest%20of%20the%20payment%20service%20users%20against%20the%20claims%20of%20other%20creditors%20of%20the%20payment%20institution%2C%20in%20particular%20in%20the%20event%20of%20insolvency) backing the e-money.
- [Safeguarding of reserves](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110#d1e646-7-1) for example by nature of investing into secure, low-risk assets (denominated in Euro) falling into one of the categories set out in [Table 1 of point 14 of Annex I to Directive 2006/49/EC](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex:32006L0049#d1e251-219-1)

From an end-user perspective, the notion of e-money and the associated regulations may seem new, however, a person living in the EU is most likely already using it on regular basis without even realizing it. A classic example of an EMI is PayPal, whose euro offerings are regulated as e-money.

### What is it backed by?

As per [Article 7(1) of the e-money directive](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110#d1e646-7-1:~:text=issuing%20electronic%20money.-,Article%207,-Safeguarding%20requirements) an EMI is obliged to safeguard the reserves backing the e-money in accordance with [Article 9(1) and (2) of the payment services directive 1](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex:32007L0064#:~:text=with%20paragraph%C2%A01.-,Article%C2%A09,-Safeguarding%20requirements).

>[Article 9(1) of the payment services directive](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex:32007L0064#:~:text=Safeguarding%20requirements-,1,-.%C2%A0%C2%A0%C2%A0The%20Member%20States)
> 
>(a) they shall not be commingled at any time with the funds of any natural or legal person other than payment service users on whose behalf the funds are held .. and they shall be deposited in a separate account in a credit institution or invested in secure, liquid low-risk assets as defined by the competent authorities of the home Member State;
> 
>(b) they shall be insulated in accordance with national law in the interest of the payment service users against the claims of other creditors of the payment institution, in particular in the event of insolvency;
>
>or
>
>(c) they shall be covered by an insurance policy or some other comparable guarantee from an insurance company or a credit institution, which does not belong to the same group as the payment institution itself .. payable in the event that the payment institution is unable to meet its financial obligations.

> [Article 9(2) of the payment services directive](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex:32007L0064#:~:text=its%20financial%20obligations.-,2,-.%C2%A0%C2%A0%C2%A0Where%20a%20payment)
>
>Where a payment institution is required to safeguard funds .. to be used for future payment transactions .. shall also be subject to the requirements under paragraph 1. Where that portion is variable or unknown in advance, Member States may allow payment institutions to apply this paragraph on the basis of a representative portion assumed to be used for payment services provided such a representative portion can be reasonably estimated on the basis of historical data to the satisfaction of the competent authorities.
>
> >**In layman's terms:** The buffer to meet the daily redemptions e.g. needs to be safeguarded in accordance with the same requirements as laid out in paragraph 1.

To summarize the above: The EMI is obliged to hold **all** reserves in a bank account segregated from the EMI or have these invested into secure, low-risk assets, or have the reserves covered by an insurance policy.

Now the last outstanding question is probably, what exactly are secure, low-risk assets? For that we have to look at [paragraph 2 of Article 7 (EMD)](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110#d1e646-7-1):

> For the purposes of paragraph 1, secure, low-risk assets are asset items falling into one of the categories set out in [Table 1 of point 14 of Annex I to Directive 2006/49/EC](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex:32006L0049#d1e251-219-1) of the European Parliament and of the Council of 14 June 2006 on the capital adequacy of investment firms and credit institutions (10) for which the specific risk capital charge is no higher than 1,6 %, but excluding other qualifying items as defined in point 15 of that Annex.
> 
> For the purposes of paragraph 1, secure, low-risk assets are also units in an undertaking for collective investment in transferable securities ([UCITS](https://www.investopedia.com/terms/u/ucits.asp)) which invests solely in assets as specified in the first subparagraph.

[Table 1 of point 14 of Annex I to the Capital Adequacy Directive](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex:32006L0049#d1e251-219-1) reads, as far as relevant:

> Debt securities issued or guaranteed by central governments, issued by central banks, international organisations, multilateral development banks or Member States' regional government or local authorities which would qualify for credit quality step 1 or which would receive a 0 % risk weight under the rules for the risk weighting of exposures under [Articles 78 to 83 of Directive 2006/48/EC](https://eur-lex.europa.eu/legal-content/en/ALL/?uri=CELEX%3A32006L0048#:~:text=Subsection%201-,Standardised%20approach,-Article%2078).
>
> RISK CHARGE: 0%

> Debt securities issued or guaranteed by central governments, issued by central banks, international organisations, multilateral development banks or Member States' regional governments or local authorities which would qualify for credit quality step 2 or 3 under the rules for the risk weighting of exposures under [Articles 78 to 83 of Directive 2006/48/EC](https://eur-lex.europa.eu/legal-content/en/ALL/?uri=CELEX%3A32006L0048#:~:text=Subsection%201-,Standardised%20approach,-Article%2078), and debt securities issued or guaranteed by institutions which would qualify for credit quality step 1 or 2 under the rules for the risk weighting of exposures under [Articles 78 to 83 of Directive 2006/48/EC](https://eur-lex.europa.eu/legal-content/en/ALL/?uri=CELEX%3A32006L0048#:~:text=Subsection%201-,Standardised%20approach,-Article%2078), and debt securities issued or guaranteed by institutions which would qualify for credit quality step 3 under the rules for the risk weighting of exposures under [point 28, Part 1 of Annex VI to Directive 2006/48/EC](https://eur-lex.europa.eu/legal-content/en/ALL/?uri=CELEX%3A32006L0048#:~:text=Credit%20assessment%20based%20method), and debt securities issued or guaranteed by corporates which would qualify for credit quality step 1 or 2 under the rules for the risk weighting of exposures under [Articles 78 to 83 of Directive 2006/48/EC](https://eur-lex.europa.eu/legal-content/en/ALL/?uri=CELEX%3A32006L0048#:~:text=Subsection%201-,Standardised%20approach,-Article%2078).
> 
> RISK CHARGE:
> 
> 0,25 % (residual term to final maturity 6 months or less)
> 
> 1,00 % (residual term to final maturity greater than 6 and up to and including 24 months)
> 
> 1,60 % (residual term to final maturity exceeding 24 months)

Excursus:
A risk capital charge is a measure of the amount of money that a financial institution is required to hold in reserve in order to protect itself and its creditors against the possibility of losses on its investments or other activities. This capital is often referred to as "risk-weighted" because it is based on the level of risk associated with the institution's various assets and activities.

Now without going into the details of what step 1-3 under the rules of risk weighting exactly means and how it is calculated, we will boil down the variation of possible and relevant secure and low-risk liquid assets to the table below for the sake of simplicity:

> Debt securities issued or guaranteed by central governments, issued by central banks, international organisations, multilateral development banks or Member States' regional government or local authorities. For example government bonds.
> 
> With a risk capital charge of [0% which equals a AAA to AA- rating](https://www.bis.org/basel_framework/chapter/MAR/20.htm#:~:text=The%20specific%20risk%20capital%20requirements%20for%20%E2%80%9Cgovernment%E2%80%9D%20and%20%E2%80%9Cother%E2%80%9D%20categories%20will%20be%20as%20follows).

> The same type of debt securities as before but with a risk capital charge of 0.25% to 1.6% which equals A+ to BBB- rating.  
or debt securities issued by AAA to A-rated financial institutions  
or debt securities issued by AAA to A-rated corporates
>
> [Reference for the rating scale with regard to the credit quality step](https://www.eiopa.europa.eu/sites/default/files/publications/amended_draft_mapping_report_-_spread_research_abridged.pdf)

> A third option is [UCITS](https://www.investopedia.com/terms/u/ucits.asp) funds, which invest in the very same assets as specified above. This could be a fund vehicle provided by asset managers (like BlackRock) that can be used by an EMI to "outsource" the operations behind managing the reserves. 

### Risk Summary E-money:

It is important to note that there are residual risks associated with e-money, such as defaulting governments or banks. However, considering that an EMI is likely banking with systemic relevant banks and investing in AAA-rated gov. bonds the remaining risks can be practically disregarded. As a matter of fact, e-money is considerably less risky than the systemic relevant banks which operate in the EU. Firstly, unlike banks that operate under the fractional reserve system, e-money is over-collateralized with 2% of the outstanding issued e-money. Secondly, Thirdly, most of the assets are invested into high-quality liquid assets and only the necessary amount is kept at a bank to meet regular withdrawals (as part of a prudent practice which is also followed by Monerium), whereby the reserves are also segregated from the EMI. Fourthly, gov. bonds are secured through the ability of the government to tax its citizens, while banks do not have such an option. And lastly, you have diversification benefits by nature of investing in multiple different gov. bonds. All of these factors combined make the case for money that provides a more secure money scheme than demand deposits.

## Monerium

Similar to [Circle](https://www.circle.com/en/), [Monerium](https://monerium.com/) is a TradFi-rooted provider of stablecoins that to date is the only issuer of on-chain euro regulated as e-money under the [e-money directive (2009/110/EC)](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32009L0110). As previously discussed, e-money is at all times redeemable at par value and is issued under the premise of strict safeguarding measures of the reserves. For now, only the euro stablecoin is offered by Monerium, however other currencies will be added in the future. Like the euro, these will be following (as possible) the same high standards of the EMD regardless of the jurisdiction.

### How does it work?

First, a customer needs to register at https://monerium.com/ and go through the KYC process to finish the registration. After successful KYC, the customer receives a bank number (IBAN) that can then be connected to a corresponding on-chain address. This is achieved by signing a message which proves the ownership of the on-chain address. Once this is done, the customer can send euros to their IBAN via the [SEPA Instant scheme](https://www.paysera.com/v2/en/blog/what-is-sepa#:~:text=What%20is%20a%20SEPA%20Instant%20payment%3F) for example. Subsequently, the equivalent amount of EURe will be credited to their on-chain wallet within 1-2 minutes.

In a similar manner, EURe can be off-boarded. For that, a user needs to hold EURe on the on-chain address which corresponds to his IBAN. He then initiates the withdrawal process to a bank account, whereupon an equivalent amount is automatically burned from the on-chain wallet.

Currently, the process of burning/withdrawing the stablecoins is being approved by hand. In the future, this process will be scaled to allow automatic withdrawals up to a certain threshold that will also be cleared within seconds through SEPA Instant.

Furthermore, Monerium will add the option to change the on-chain address to enhance the privacy of its customers.

### Regulatory & Legal

Monerium is an authorised electronic money institution, granted authorization to issue electronic money according to Act no. 17/2013, on issuance and handling of electronic money, Icelandic Act implementing Directive 2009/110/EC. Monerium provides its services online but its registered office is at Bjargargata 1, 102 Reykjavík, Iceland (Company number 5711100240). The company is supervised by the Financial Supervisory Authority of the Central bank of Iceland. ([See here for further information](https://monerium.com/policies/business-terms-of-service/))

Additional [regulatory requirements](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593) ([Markets in Crypto Assets Regulation - MiCA](https://www.sidley.com/en/insights/newsupdates/2022/11/how-will-the-eu-markets-in-crypto-assets-regulation-affect-crypto-and-other-financial-services-firms)) are currently in the works which would impose further obligations on fiat-referenced tokens (e-money tokens). According to MiCA, such tokens should not only adhere to the EMD but also adhere to requirements such as [diligent marketing](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Article%2048%0A%C2%A0%C2%A0%C2%A0Marketing%20communications), [disclosure of risks](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=the%20risks%20relating%20to%20the%20issuer%20of%20e%2Dmoney%20issuer%2C%20the%20e%2Dmoney%20tokens%20and%20the%20implementation%20of%20the%20project%2C%20including%20the%20technology%3B), [prohibition of interest](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Prohibition%20of%20interests), and more. Currently, MiCA has not entered into force however it is expected that this will happen [somewhere around Q1 2023, after which it will come into effect in 2024](https://www.iflr.com/article/2a647jipe3beilngwqlmo/primer-markets-in-crypto-assets-regulation-mica#:~:text=The%20European%20Council%20published%20the,the%20requirements%20of%20the%20regulation.) when implemented by the member states of the EU. In this regard, Monerium is already coherent with future regulations by issuing their euro stablecoin under the EMD.

Beyond the 1B Eur mark of issued e-money [among other conditions for the classification of e-money tokens as "significant"](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=The%20EBA%20shall%20classify%20e%2Dmoney%20tokens%20as%20significant%20e%2Dmoney%20tokens%20on%20the%20basis%20of%20the%20criteria%20referred%20to%20in%20Article%2039(1)%2C%20as%20specified%20in%20accordance%20with%20Article%2039(6)%2C%20and%20where%20at%20least%20three%20of%20those%20criteria%20are%20met.), there are additional obligations to be considered under MiCA.

These [additional obligations](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Article%2052%0ASpecific%20additional%20obligations%20for%20issuers%20of%20significant%20e%2Dmoney%20tokens) emphasize [enhanced safeguarding measures](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=in%20Article%2019.-,Article%2033,-Custody%20of%20reserve) such as [due diligence to select a reasonable and appropriate partner bank](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Issuers%20of%20asset%2Dreferenced%20tokens%20shall%20exercise%20all%20due%20skill%2C%20care%20and%20diligence%20in%20the%20selection%2C%20appointment%20and%20review%20of%20credit%20institutions%20and%20crypto%2Dasset%20providers%20appointed%20as%20custodians%20of%20the%20reserve%20assets%20in%20accordance%20with%20paragraph%202.) for the custody of the funds. Other noteworthy  requirements include:

* [Another bank shall act as the custodian of the funds than the one used by the EMI.](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Issuers%20of%20significant%20asset%2Dreferenced%20tokens%20shall%20ensure,on%20a%20fair%2C%20reasonable%20and%20non%2Ddiscriminatory%20basis.)
* [The equirement to monitor liquidity needs to meet ad-hoc redemption requests.](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Issuers%20of%20significant%20asset%2Dreferenced%20tokens%20shall%20assess%20and%20monitor%20the%20liquidity%20needs%20to%20meet%20redemption%20requests%20or%20the%20exercise%20of%20rights%2C%20as%20referred%20to%20in%20Article%2034%2C%20by%20holders%20of%20asset%2Dreferenced%20tokens.)
* [The over-collateralization percentage shall be 3% instead of 2%](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=The%20percentage%20referred%20to%20in%20Article%C2%A031(1)%2C%20point%20(b)%2C%20shall%20be%20set%20at%203%25%20of%20the%20average%20amount%20of%20the%20reserve%20assets%20for%20issuers%20of%20significant%20asset%2Dreferenced%20tokens.)
* [A plan should be in place to be able to orderly wind down the company's activities.](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A52020PC0593#:~:text=Chapter%206-,Orderly%20wind%2Ddown,-Article%2042%0AOrderly)

### Smart Contracts

There were no smart contract audits being provided for Monerium's EURe. However, we decided to analyze the contracts ourselves, specifically looking for centralization vectors and elevated rights.

What we found was the following:

* [The owner](https://etherscan.io/address/0xA176a20E3698bAa36Ea09E4499e407e7dbFdfa0c) of the [EURe contract](https://etherscan.io/address/0x3231cb76718cdef2155fc47b5286d82e6eda273f#writeContract) is an externally owned account (EOA)
* [The owner](https://etherscan.io/address/0xA176a20E3698bAa36Ea09E4499e407e7dbFdfa0c) of the [EURe contract](https://etherscan.io/address/0x3231cb76718cdef2155fc47b5286d82e6eda273f#writeContract) has the ability to upgrade the controller. (And hence being able to infinitely mint EURe)
* [The owner](https://etherscan.io/address/0xA176a20E3698bAa36Ea09E4499e407e7dbFdfa0c) of the [EURe contract](https://etherscan.io/address/0x3231cb76718cdef2155fc47b5286d82e6eda273f#writeContract) has the ability to call the `selfdestruct` function, which kills the contract and transfers all EURe to the owner.
* [A SystemAccount](https://etherscan.io/address/0x882145b1f9764372125861727d7be616c84010ef) (within the controller) is an EOA.
* [A SystemAccount](https://etherscan.io/address/0x882145b1f9764372125861727d7be616c84010ef) is able to (infinitely) mint EURe.
* [The controller contract](https://etherscan.io/address/0x914C6a264bC3cC68966C33BBD6630F011E7a47cF) is not verified on etherscan.

Though all of these findings are technically non-critical for Monerium since (for now) withdrawals above a low 3-figure amount are manually reviewed they still pose critical risks for EURe LPs. By being able to infinitely mint EURe and selling into the pool it is possible (for the EOA or the SystemAccount) to rug LPs.

Followed by identifying the above, we reached out to the Monerium team to discuss the issues and how they could potentially be resolved. The result of this discussion was satisfactory in the sense that we were able to work out solutions to mitigate the issues. We were promised that these will be implemented in the coming weeks. This includes:

* Replacing the owner of the EURe contract with a multi-sig
* Keeping the SystemAccount as an EOA (for operational reasons > automating mints/burns), however implementing changes into the controller that would put a limit on how much EURe can be minted. This would greatly reduce the repercussions of a compromised SystemAccount.
* The hiring of an auditor to review the smart contracts

We will monitor the deliveries in the coming weeks and will notify the DAO should Monerium fail to live up to their promises. 

### Conclusion:

In principle, the stringent regulatory framework under which Monerium operates provides a sound foundation for a stablecoin. However, at this point, there are still a few issues to work through to reduce the non-negligible operational risks.
 
As for the risks regarding the elevated rights within the EURe contracts, these will be mainly confined within the powers of the multi-sig, yet to be deployed. Since trust assumptions can be made by virtue of the regulatory compliance and reputation of the company (as with Circle or Stasis), the associated risks can be considered minimal.

We are convinced that bridging regulated TradFi rooted form of money into DeFi is a good, inevitable, and the next logical step, especially against the background of uncertainties raised towards existing and unregulated stablecoin providers. We think Monerium is a good contender within the euro (and other) stablecoins and are looking forward to seeing them grow into the DeFi ecosystem.
