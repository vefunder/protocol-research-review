# Proposal to Fund Several Curve Sub-DAOs from the Community Fund

## **Summary**: 

This proposal is to create multiple CRV vests from the [Curve Community Fund](https://etherscan.io/token/0xD533a949740bb3306d119CC777fa900bA034cd52?a=0xe3997288987e6297ad550a69b31439504f513267) to fund the operations for the following Curve sub-DAOs:
- Curve Research | 6m CRV
- Curve Risk | 2m CRV
- Curve Analytics | 2m CRV
- Dev Tooling | 2m CRV
- Curve Ventures | 20m CRV

The vests will fund operations for each sub-DAO for a period of 2 years from Q1 '24 until Q1 '26.

## **Abstract**:

Vests can be initiated by calling `deploy_vesting_contract()` to the Vesting Escrow Factory with a minimum duration of 365 days. The DAO can disable the vest by calling `toggle_disable()` on the deployed Simple Vesting Escrow contract during the vesting period.  

Each sub-DAO will receive their vest to a multisig composed of trusted individuals working in the respective discipline. Some of these sub-DAOs already have a long history working on behalf of the DAO (Curve Research, Llama Risk, and Curve Analytics) and some proposed sub-DAOs will be newly created as part of this proposal (Dev Tooling and Curve Ventures). The responsibilities of each sub-DAO are described in the Specifications section.
 
## **Motivation**:

Over the years, we have recognized an apparent need for various departments to support the continued development of Curve. The current funding process for these groups involves proposing a scope of work to the Curve Grant Council to periodically secure budgets, usually on a quarterly or annual basis depending on the requested budget size. There are a couple of downsides to this approach: less direct oversight of sub-DAOs from the Curve DAO directly and less certainty about continued sub-DAO funding.

This alternative approach would give sub-DAOs greater autonomy, not only to conduct their own internal operations but also to additionally fund other projects related to their focus area. They would be allocated a discretionary budget to act as a discipline-focused grant council. For instance, independent teams conducting Curve research could petition the Curve Research sub-DAO for funding their project. The expectation is that teams coordinating in this way, within a particular discipline, are better suited to recognize value adds and produce useful collaborations. Well-managed sub-DAOs should attract talent within the community and experience contributor growth.

Note that the proposed sub-DAO model is not intended to supersede the existing Grant Council. Rather sub-DAOs are meant to supplement it by making more opportunities available to potential Curve contributors.

## **Specification**:

### **Curve Research**
- Budget: 6m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- 3-of-5 multisig: 0x2Ba80b37e0beF5c92A0C3cb780f8583372D773Ac
    - Naga - Curve Researcher
    - Chanho - Curve Researcher 
    - Fiddy - Curve Team
    - PilotVietnam - Curve Team
    - Michael - Curve Founder
- Team: The Curve Research Sub-DAO currently consists of researchers Chanho, Naga, 0xStan, 0xMc, and Paco with additional support from Xenophon Labs.
- [TODO: responsibilities]

### **Curve Risk**
- Budget: 2m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- 3-of-6 multisig: 0xa2482aA1376BEcCBA98B17578B17EcE82E6D9E86
     - Wormhole - Head Llama Herder
     - Knows - Asst. Head Llama Herder
     - Naga - Curve Researcher
     - Chanho - Curve Researcher
     - Fiddy - Curve Team
     - Amadeo - Grants Council Member
- Team: Llama Risk currently consists of managers Wormhole and Knows, technical lead Marin, legal counsel Svetlin, and our Llama Risk analyst team.
- Responsibilities: Llama Risk conducts risk assessments and analysis relevant to Curve and its LPs. It began its operations in Q4 '21 as a community initiative and has been operating as a DAO-funded non-profit since then. It typically reviews stablecoin and LSD protocols that have Curve pools and has recently begun assessing existing and prospective collateral types for crvUSD. In addition to its own in-house assessments, it works with third-party risk and analytics teams such as Xenophon Labs and Chaos Labs to provide relevant risk-related services to Curve. It has also recently begun work in regulatory affairs to help regulators understand the state of DeFi and the projects building in it.
     - Investigate and publish risk assessments relevant to Curve DAO, LPs, and crvUSD users
     - Point of contact with protocol teams integrating with Curve
     - DAO housekeeping- monitor the state of gauges and DAO votes for issues requiring action
     - Regulatory Affairs- outreach with regulators and web3 working groups
     - Fund risk analysis teams to provide dashboards/analytics for Curve users 

### **Curve Analytics**
- Budget: 2m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- 2-of-3 multisig: 0xAeF6ea60f6443BAD046E825C1d2b0C0B5eBC1f16
     - Benny - Curve Monitor Lead
     - Alunara - Curve Monitor front end
     - Phillip - Curve Monitor back end
- Team: Curve Analytics currently consists of developers Benny, Alunara, and Phillip.
- Responsibilities: The Analytics team was established in Q2 '23 with the objective of providing open, transparent, and reliable data streams encompassing all aspects of the Curve ecosystem. Our work includes the development of an analytics platform <a href="http://www.curvemonitor.com">Curve Monitor<a/>, which offers real-time dashboards featuring pool metrics, MEV monitoring, revenue tracking, and information on crvUSD rates and markets. Additionally, we are responsible for developing and maintaining back-end services which are used by Curve's official front-end, Curve sub-DAOs, and third-party protocols working within the Curve ecosystem. We also maintain bots on messaging apps that provide real-time tracking of user activity and MEV on Curve.
     - Develop and maintain user-facing analytics services through Curve Monitor and messaging app bots.
     - Develop and maintain back-end API services (such as curve-prices API, Llama Airforce API, and subgraphs), ensuring their reliability and performance while providing detailed, up-to-date documentation for each.
     - Collaborate with the Curve core team, Curve sub-DAOs, third-party protocols, and the wider Curve community to ascertain existing and upcoming data requirements and gather feedback for potential improvements to Curve Monitor and other analytics services.
     - Conduct regular reviews and audits of the data to ensure accuracy.
     - Monitor inaccurate Curve-related metrics on other platforms and liaise with said platforms to provide corrections when needed.
     - Use data to provide insights and reports on MEV, gas usage, user behavior, etc. to the core team and DAO.
     - Maintain an open codebase and onboard new analysts and contributors


### **Dev Tooling**
- Budget: 2m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- 3-of-5 multisig [TODO: multisig]
     - Fiddy - Curve Dev Tooling Lead
     - Charles - Vyper Dev
     - Fubuloubu - Vyper and ApeWorx Dev
     - Wavey - Yearn Dev
     - Pascal - Vyper (Snekmate) Dev
- [TODO: team]
- [TODO: responsibilities]

### **Curve Ventures**
- Budget: 20m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- [TODO: multisig]
- [TODO: team]
- Responsibilities: Curve Ventures will invest in Curve ecosystem projects that have a token and continue a relationship with its portfolio projects to support their success. The sub-DAO will have fixed operational costs to pay manager salaries, but otherwise, the CRV in its treasury will be used to purchase a stake in Curve ecosystem projects. It seeks to earn a return on its initial investment for the benefit of the Curve DAO. Profits from its activities belong to the Curve DAO to be directed either to the treasury or tokenholders as the DAO sees fit. 
    - funding projects that are aligned with the long-term growth of the Curve ecosystem
    - provide guidance on integration (pool parameter setting, oracle use, working with Vyper contracts, etc.) & technical support (for instance on migrating to Vyper if the team wants to do solidity)
    - feedback on protocol design 
    - recommendations on security, quality audit firms
    - access to a network within the larger curve ecosystem and community engagement
    - seal of approval from the curve brand

