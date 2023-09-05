# Proposal to Fund Several Curve SubDAOs from the Community Fund

## **Summary**: 

This proposal is to create multiple CRV vests from the [Curve Community Fund](https://etherscan.io/token/0xD533a949740bb3306d119CC777fa900bA034cd52?a=0xe3997288987e6297ad550a69b31439504f513267) to fund the following Curve subDAOs:
- Curve Research | 10m CRV
- Curve Risk | 5m CRV
- Curve Analytics | 5m CRV
- Dev Tooling | 10m CRV

The vests will fund subDAOs for a period of 2 years from Q1 '24 until Q1 '26.

## **Abstract**:

The responsibilities of each subDAO are described in the Specifications section. In general, each subDAO will fund its core operations and other teams and projects working within the Curve ecosystem. At times there may be a crossover between each subDAO's area of focus, in which case multiple subDAOs may coordinate on funding and project management responsibilities. For example, Llama Risk, Curve Research, and Curve Monitor recently worked together to manage a project by Xenophon Labs to conduct research on predicting stablecoin depegs with deliverables being a research paper and API that could be integrated into a risk dashboard (see: [arvix](https://arxiv.org/abs/2306.10612), [github](https://github.com/xenophonlabs/curve-lp-metrics/tree/1a62de3bce4962c2be09676e996ea927fd5714ed), [tweet](https://twitter.com/XenophonLabs/status/1671549347174154242)).  

Each subDAO will receive a vest to a multisig composed of trusted individuals working in the respective discipline. Some of these subDAOs already have a long history working on behalf of the DAO (Curve Research, Curve Risk, and Curve Analytics). The Dev Tools subDAO has been recently created, as seen in the [announcement](https://gov.curve.fi/t/introducing-the-dev-tools-subdao/9565). The CRV will vest linearly over 365 days from the Community Fund, and the DAO retains the right to cancel the remainder of the vest at any point. In case not all funds have been used by Q1 '26, subDAOs agree to either return the remainder to the Curve community fund or roll the remaining funds over to a subsequent round, pending DAO approval.

Each subDAO is responsible for any legal wrapper they require to conduct their operations and handling the IP ownership for any product produced by the subDAO. Each subDAO is expected to share quarterly operations updates to ensure continued accountability to the DAO. 

## **Motivation**:

Over the years, we have recognized an apparent need for various departments to support the continued development of Curve. The current funding process for these groups involves proposing a scope of work to the Curve Grant Council to periodically secure budgets, usually on a quarterly or annual basis depending on the requested budget size. There are a couple of downsides to this approach: less direct oversight of subDAOs from the Curve DAO and less certainty about continued funding for the recipients of the grants. 

This alternative approach would give subDAOs greater autonomy, not only to conduct their own internal operations but also to fund other projects related to their focus area. For instance, independent teams conducting Curve research could petition the Curve Research subDAO for funding their Curve-related project. The expectation is that teams coordinating in this way, within a particular discipline, are better suited to recognize value adds and produce valuable collaborations. Well-managed subDAOs should attract talent within the community and experience contributor growth.

## **Specification**:

Vests can be initiated by calling `deploy_vesting_contract()` to the Vesting Escrow Factory with a minimum duration of 365 days. The DAO can disable the vest by calling `toggle_disable()` on the deployed Simple Vesting Escrow contract during the vesting period. 

Details about each proposed sub-DAO are below:

### **Curve Research**
- **Budget**: 10m CRV vested over 365 days for operations from Q1 '24 to Q1 '26.
- **3-of-5 multisig**: 0x2Ba80b37e0beF5c92A0C3cb780f8583372D773Ac
    - Naga - Curve Researcher
    - Chanho - Curve Researcher 
    - Fiddy - Curve Team
    - PilotVietnam - Curve Team
    - Michael - Curve Founder
- **Team**: The Curve Research subDAO currently consists of researchers Chanho, Naga, 0xStan, 0xMc, and Paco with additional support from Xenophon Labs.
- **Responsibilities**: Curve research identifies and focuses on high-impact quantitative initiatives throughout the Curve ecosystem. In addition to providing the Curvesim simulation tool to ease analysis and integration with Curve, the research team liaisons with multiple Curve ecosystem participants to help configure Curve products and resolve quantitative issues arising from unexpected market events. Finally, Curve Research aims to improve users' understanding and usage of Curve products by publishing user guides, explainers, and market-relevant analyses.
     - Develop and maintain the Curvesim simulation package
     - Perform simulations and risk assessments of parameter change proposals
     - Liaison with other protocols to optimize parameters for new and already-existing pools
     - Collaborate with Llama Risk, Curve Analytics, and third-party research teams on quantitative aspects of their respective work
     - Publish practical guides and in-depth explainers of Curve products, as well as quantitative analyses of developments in the Curve ecosystem


### **Curve Risk**
- **Budget**: 5m CRV vested over 365 days for operations from Q1 '24 to Q1 '26.
- **3-of-6 multisig**: 0xa2482aA1376BEcCBA98B17578B17EcE82E6D9E86
     - Wormhole - Head Llama Herder
     - Knows - Llama Risk Advisor
     - Naga - Curve Researcher
     - Chanho - Curve Researcher
     - Fiddy - Curve Team
     - Amadeo - Grants Council Member
- **Team**: Llama Risk currently consists of managers Wormhole, Knows, and Fiddy, technical lead Marin, legal counsel Svetlin, and our Llama Risk analyst team.
- **Responsibilities**: Llama Risk conducts risk assessments and analysis relevant to Curve and its LPs. It began its operations in Q4 '21 as a community initiative and has been operating as a DAO-funded nonprofit ever since. It typically reviews stablecoin, RWA, and LST protocols that have Curve integrations and has recently begun assessing existing and prospective collateral types for crvUSD. In addition to its own in-house assessments, it works with third-party risk and analytics teams such as Xenophon Labs and Chaos Labs to provide relevant risk-related services to Curve. It has also recently begun work in regulatory affairs to help regulators understand the state of DeFi and the projects building in it.
     - Investigate and publish risk assessments relevant to Curve DAO, LPs, and crvUSD users
     - Point of contact with protocol teams integrating with Curve
     - DAO housekeeping- monitor the state of gauges and DAO votes for issues requiring action
     - Regulatory Affairs- outreach with regulators and web3 working groups
     - Fund risk analysis teams to provide dashboards/analytics for Curve users 

### **Curve Analytics**
- **Budget**: 5m CRV vested over 365 days for operations from Q1 '24 to Q1 '26.
- **2-of-3 multisig**: 0xAeF6ea60f6443BAD046E825C1d2b0C0B5eBC1f16
     - Benny - Curve Monitor Lead
     - Alunara - Curve Monitor front end
     - Phillip - Curve Monitor back end
- **Team**: Curve Analytics currently consists of developers Benny, Alunara, and Phillip.
- **Responsibilities**: The Analytics team was established in Q2 '23 with the objective of providing open, transparent, and reliable data streams encompassing all aspects of the Curve ecosystem. Our work includes the development of an analytics platform <a href="http://www.curvemonitor.com">Curve Monitor<a/>, which offers real-time dashboards featuring pool metrics, MEV monitoring, revenue tracking, and information on crvUSD rates and markets. Additionally, we are responsible for developing and maintaining back-end services that are used by Curve's official front-end, Curve subDAOs, and third-party protocols working within the Curve ecosystem. We also maintain bots on messaging apps that provide real-time tracking of user activity and MEV on Curve.
     - Develop and maintain user-facing analytics services through Curve Monitor and messaging app bots.
     - Develop and maintain back-end API services (such as curve-prices API, Llama Airforce API, and subgraphs), ensuring their reliability and performance while providing detailed, up-to-date documentation for each.
     - Collaborate with the Curve core team, Curve subDAOs, third-party protocols, and the wider Curve community to ascertain existing and upcoming data requirements and gather feedback for potential improvements to Curve Monitor and other analytics services.
     - Manage multiple independent analytics teams to provide useful and accurate analytics for Curve.
     - Conduct regular reviews and audits of the data to ensure accuracy.
     - Monitor inaccurate Curve-related metrics on other platforms and liaise with said platforms to provide corrections when needed.
     - Use data to provide insights and reports on MEV, gas usage, user behavior, etc. to the core team and DAO.
     - Maintain an open codebase and onboard new analysts and contributors


### **Dev Tools**
- **Budget**: 10m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- **3-of-6 multisig**: 0x637351b0a32bC7bb7F6A19686a76D84233710Ad9
     - Fiddy - Curve Dev Tools Lead
     - Charles - Vyper + Titanoboa Lead
     - Fubuloubu - Vyper, Yearn Dev and ApeWorx Founder
     - Wavey - Yearn Dev
     - Pascal - Vyper (Snekmate) Dev
     - Wormhole - Dev Tools Advisor
- **Team**: Dev Tools will be a newly formed group composed of developers working within the Curve and Vyper ecosystem.
- **Responsibilities**: The Dev Tools group will be responsible for building and maintaining essential tooling for Curve including Vyper, ApeWorx and others. The group will identify the most significant needs and manage the allocation of funds toward creating and maintaining such tools. Its priority is to ensure Vyper is adequately funded and supervise its development, as the programming language and its associated tooling is a core technology underpinning Curve. Currently, funds are planned to be used for the following endeavors:
     - Fund development and maintenance of the Vyper language and associated Titanoboa interpreter, including bugfixes, maintenance, features and performance improvements
         - Ongoing, regular audits of the Vyper compiler from multiple tier 1 audit firms (tentatively: ChainSecurity, Statemind, Ottersec and Certora)
         - Funding support from FV tools and static analyzers, including Certora, Slither, Halmos, Mythril and HEVM
         - Regular audit/review competitions from Codehawks, Sherlock and Code4rena
         - Bug bounty programs administered through Immunefi
     - Otherwise allocating funds toward developers, audits, and bug bounty programs as needed
     - Supervise the progress of teams receiving funding
     - Coordinate with teams and people building tooling for Curve
     - Monitor for future dev tooling needs and proactively explore suitability for Curve
     - Allocate funds toward developers, audits, and bug bounty programs as needed
  

