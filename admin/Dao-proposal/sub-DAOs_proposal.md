# Proposal to Fund Several Curve Sub-DAOs from the Community Fund

## **Summary**: 

This proposal is to create multiple CRV vests from the [Curve Community Fund](https://etherscan.io/token/0xD533a949740bb3306d119CC777fa900bA034cd52?a=0xe3997288987e6297ad550a69b31439504f513267) to fund the operations for the following Curve sub-DAOs:
- Curve Research | 6m CRV
- Llama Risk | 2m CRV
- Curve Analytics (Curve Monitor) | 2m CRV
- Dev Tooling | 2m CRV
- Curve Capital Management | 20m CRV

The vests will fund operations for each sub-DAO for a period of 2 years from Q1 '24 until Q1 '26.

## **Abstract**:

Vests can be initiated by calling `deploy_vesting_contract()` to the Vesting Escrow Factory with a minimum duration of 365 days. The DAO can disable the vest by calling `toggle_disable()` on the deployed Simple Vesting Escrow contract during the vesting period.  

Each sub-DAO will receive their vest to a multisig composed of trusted individuals working in the respective discipline. Some of these sub-DAOs already have a long history working on behalf of the DAO (Curve Research, Llama Risk, and Curve Analytics) and some proposed sub-DAOs will be newly created as part of this proposal (Dev Tooling and Curve Capital Management). The responsibilities of each sub-DAO is described in the Specifications section.
 
## **Motivation**:

Over the years, we have recognized an apparent need for various organized departments to support the continued development of Curve. The current funding process for these groups involves proposing a scope of work to the Curve Grant Council to periodically secure budgets, usually on a quarterly or annual basis depending on the requested budget size. There are a couple of downsides to this approach: less direct oversight of sub-DAOs from the Curve DAO directly and less certainty about continued sub-DAO funding.

This alternative approach would give sub-DAOs greater autonomy, not only to conduct their own internal operations but also to additionally fund other projects related to their focus area. They would be allocated a discretionary budget to act as a discipline-focused grant council. For instance, independent teams conducting Curve research could petition the Curve Research sub-DAO for funding their project. The expectation is that teams coordinating in this way, within a particular discipline, are better suited to recognize value adds and produce useful collaborations. Well-managed sub-DAOs should attract talent within the community and experience contributor growth.

Note that the proposed sub-DAO model is not intended to supersede the existing Grant Council. Rather sub-DAOs are meant to supplement it by making more opportunities available to potential Curve contributors.

## **Specification**:

> Technical details if applicable
