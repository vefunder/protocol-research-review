# Proposal to Fund Several Curve Sub-DAOs from the Community Fund

## **Summary**: 

This proposal is to create multiple CRV vests from the [Curve Community Fund](https://etherscan.io/token/0xD533a949740bb3306d119CC777fa900bA034cd52?a=0xe3997288987e6297ad550a69b31439504f513267) to fund the operations for the following Curve sub-DAOs:
- Curve Research | 6m CRV
- Llama Risk | 2m CRV
- Curve Analytics (Curve Monitor) | 2m CRV
- Dev Tooling | 2m CRV
- Curve Ventures | 20m CRV

The vests will fund operations for each sub-DAO for a period of 2 years from Q1 '24 until Q1 '26.

## **Abstract**:

Vests can be initiated by calling `deploy_vesting_contract()` to the Vesting Escrow Factory with a minimum duration of 365 days. The DAO can disable the vest by calling `toggle_disable()` on the deployed Simple Vesting Escrow contract during the vesting period.  

Each sub-DAO will receive their vest to a multisig composed of trusted individuals working in the respective discipline. Some of these sub-DAOs already have a long history working on behalf of the DAO (Curve Research, Llama Risk, and Curve Analytics) and some proposed sub-DAOs will be newly created as part of this proposal (Dev Tooling and Curve Ventures). The responsibilities of each sub-DAO is described in the Specifications section.
 
## **Motivation**:

Over the years, we have recognized an apparent need for various organized departments to support the continued development of Curve. The current funding process for these groups involves proposing a scope of work to the Curve Grant Council to periodically secure budgets, usually on a quarterly or annual basis depending on the requested budget size. There are a couple of downsides to this approach: less direct oversight of sub-DAOs from the Curve DAO directly and less certainty about continued sub-DAO funding.

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

The Curve Research Sub-DAO currently consists of researchers Chanho, Naga, 0xStan, 0xMc, and Paco with additional support from Xenophon Labs.

### **Llama Risk**
- Budget: 2m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- 3-of-6 multisig: 0xa2482aA1376BEcCBA98B17578B17EcE82E6D9E86
     - Wormhole - Head Llama Herder
     - Knows - Asst. Head Llama Herder
     - Naga - Curve Researcher
     - Chanho - Curve Researcher
     - Fiddy - Curve Team
     - Amadeo - Grants Council Member

Llama Risk currently consists of managers Wormhole and Knows, technical lead Marin, legal counsel Svetlin and our Llama Risk analyst team. 

### **Curve Analytics**
- Budget: 2m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- 2-of-3 multisig: 0xAeF6ea60f6443BAD046E825C1d2b0C0B5eBC1f16
     - Benny - Curve Monitor Lead
     - Alunara - Curve Monitor front end
     - Phillip - Curve Monitor back end

Curve Analytics currently consists of developers Benny, Alunara, and Phillip.

### **Dev Tooling**
- Budget: 2m CRV vested over 365 days for operations from Q1 '24 to Q1 '26

### **Curve Ventures**
- Budget: 20m CRV vested over 365 days for operations from Q1 '24 to Q1 '26
- [TODO: multisig]

**Responsibilities**

Curve Ventures will invest in Curve ecosystem projects that have a token and continue a relationship with its portfolio projects to support their success. The sub-DAO will have fixed operational costs to pay manager salaries, but otherwise, the CRV in its treasury will be used to purchase a stake in Curve ecosystem projects. It seeks to earn a return on its initial investment for the benefit of the Curve DAO. Profits from its activities belong to the Curve DAO to be directed either to the treasury or tokenholders as the DAO sees fit. 
- funding projects that are aligned with the long-term growth of the Curve ecosystem
- provide guidance on integration (pool parameter setting, oracle use, working with Vyper contracts, etc.) & technical support (for instance on migrating to Vyper if the team wants to do solidity)
- feedback on protocol design 
- recommendations on security, quality audit firms
- access to a network within the larger curve ecosystem and community engagement
- seal of approval from the curve brand

