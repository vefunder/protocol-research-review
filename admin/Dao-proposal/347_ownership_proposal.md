# Increase crvUSD Debt Ceiling and Add wstETH Market

## **Summary**: 

DAO [vote ID 347](https://dao.curve.fi/vote/ownership/347) includes 6 actions that raise the debt ceiling of each PegKeeper (USDT/USDC/USDP/TUSD) to 25m crvUSD and adds a new crvUSD market for wstETH with a debt ceiling of 150m crvUSD. The new market also implements updated oracle, monetary policy, and stable price aggregator contracts for use with the new market. 
 

## **Motivation**:

The sfrxETH crvUSD market was deployed on May 14th ([tx](https://etherscan.io/tx/0x6d879b6e8a64478f3ecc5c2f918c4cf8c29ae0a3f93651159e4dc0278c5c49b5)) with a 10m crvUSD debt ceiling. The 4 pegkeepers (for [USDC/crvUSD](https://etherscan.io/address/0xaA346781dDD7009caa644A4980f044C50cD2ae22), [USDT/crvUSD](https://etherscan.io/address/0xE7cd2b4EB1d98CD6a4A48B6071D46401Ac7DC5C8), [USDP/crvUSD](https://etherscan.io/address/0x6B765d07cf966c745B340AdCa67749fE75B5c345) and [TUSD/crvUSD](https://etherscan.io/address/0x1ef89Ed0eDd93D1EC09E4c07373f69C49f4dcCae)) were each instantiated with a 2.5m crvUSD debt ceiling.

The crvUSD pools each received a gauge that passed a DAO vote on May 26th and received gauge weighting for the first time on May 31st. Current debt of each pegkeeper is as follows:
- TUSD: 202107 crvUSD
- USDP: 172925 crvUSD
- USDT: 0 crvUSD
- USDC: 218262 crvUSD

The sfrxETH market, deployed with a low cap of 10m crvUSD for a gradual rollout, is coming close to its cap with 9,179,000 crvUSD debt currently. 

The primary motivation of this vote is to expand the debt ceiling to avoid an upward crvUSD depeg from the addition of gauge incentives and to continue the rollout of crvUSD with an established collateral type: stETH.

The vote also implements updated contracts for use with the wstETH market- [CryptoWithStablePriceWsteth](https://etherscan.io/address/0xc1793A29609ffFF81f10139fa0A7A444c9e106Ad), [AggMonetaryPolicy](https://etherscan.io/address/0x1E7d3bf98d3f8D8CE193236c3e0eC4b00e32DaaE) and [AggregatorStablePrice](https://etherscan.io/address/0x18672b1b0c623a30089A280Ed9256379fb0E4E62)


## **Contract Updates**:

#### [CryptoWithStablePriceWsteth](https://etherscan.io/address/0xc1793A29609ffFF81f10139fa0A7A444c9e106Ad) - 0xc1793A29609ffFF81f10139fa0A7A444c9e106Ad
Description: The new oracle contract is for Tricrypto-ng and stETH-ng, additionally aggregating between [TricryptoUSDC](https://etherscan.io/address/0x7F86Bf177Dd4F3494b841a37e810A34dD56c829B) and [TricryptoUSDT](https://etherscan.io/address/0xf5f5B97624542D72A9E06f04804Bf81baA15e2B4). The contract has a bugfix that excludes some plausible manipulation (which was not caught in audit). It has additional functionality to remove the Chainlink guardrail by calling set_use_chainlink().

#### [AggMonetaryPolicy](https://etherscan.io/address/0x1E7d3bf98d3f8D8CE193236c3e0eC4b00e32DaaE) - 0x1E7d3bf98d3f8D8CE193236c3e0eC4b00e32DaaE
 Description: The new monetary policy contract adds some additional logic within calculate_rate() to raise the rate when the market is close to max. It will account for individual debt ceiling to dynamically tune rate depending on filling the market. This is in addition to accounting for price of crvUSD and amount of pegkeeper debt to calculate rate.

#### [AggregatorStablePrice](https://etherscan.io/address/0x18672b1b0c623a30089A280Ed9256379fb0E4E62) - 0x18672b1b0c623a30089A280Ed9256379fb0E4E62
The new stable price agg contract changes some functionality to aggregate stable price taking EMA of pool TVL. The contract has a bugfix that excludes some plausible manipulation (which was not caught in audit).



## **Specification**:

- Deployment logs for price oracles and monetary policies for wsteth: https://github.com/curvefi/curve-stablecoin/commit/ed1066b69c61933a711bd844a1ad39cee2c3b94a
- Script for deploying wstETH oracle, new aggregator and a new monetary policy: https://github.com/curvefi/curve-stablecoin/blob/master/scripts/ape-steth-oracle.py

#### DAO Vote Actions
**ACTION 1:** 
Description: Call crvUSD ControllerFactory, function: set_debt_ceiling, set USDC/crvUSD Pegkeeper ceiling to 25m crvUSD

```
Call via agent (0x40907540d8a6C65c637785e8f8B742ae6b0b9968):  
├─ To: 0xC9332fdCB1C491Dcc683bAe86Fe3cb70360738BC
├─ Function: set_debt_ceiling(address,uint256)
└─ Inputs: ['0xaA346781dDD7009caa644A4980f044C50cD2ae22', 25000000000000000000000000]
```

**ACTION 2:** 
 Description: Call crvUSD ControllerFactory, function: set_debt_ceiling, set USDT/crvUSD Pegkeeper ceiling to 25m crvUSD

```
 Call via agent (0x40907540d8a6C65c637785e8f8B742ae6b0b9968):
 ├─ To: 0xC9332fdCB1C491Dcc683bAe86Fe3cb70360738BC
 ├─ Function: set_debt_ceiling(address,uint256)
 └─ Inputs: ['0xE7cd2b4EB1d98CD6a4A48B6071D46401Ac7DC5C8', 25000000000000000000000000]
```

**ACTION 3:** 
Description: Call crvUSD ControllerFactory, function: set_debt_ceiling, set USDP/crvUSD Pegkeeper ceiling to 25m crvUSD

```
 Call via agent (0x40907540d8a6C65c637785e8f8B742ae6b0b9968):
 ├─ To: 0xC9332fdCB1C491Dcc683bAe86Fe3cb70360738BC
 ├─ Function: set_debt_ceiling(address,uint256)
 └─ Inputs: ['0x6B765d07cf966c745B340AdCa67749fE75B5c345', 25000000000000000000000000]
```

**ACTION 4:** 
 Description: Call crvUSD ControllerFactory, function: set_debt_ceiling, set TUSD/crvUSD Pegkeeper ceiling to 25m crvUSD

```
 Call via agent (0x40907540d8a6C65c637785e8f8B742ae6b0b9968):
 ├─ To: 0xC9332fdCB1C491Dcc683bAe86Fe3cb70360738BC
 ├─ Function: set_debt_ceiling(address,uint256)
 └─ Inputs: ['0x1ef89Ed0eDd93D1EC09E4c07373f69C49f4dcCae', 25000000000000000000000000]
```

**ACTION 5:** 
 Description: Call Tricrypto-ng/stETH-ng oracle contract, function: price_w,  update contract with most recent values

```
 Call via agent (0x40907540d8a6C65c637785e8f8B742ae6b0b9968):
 ├─ To: 0xc1793A29609ffFF81f10139fa0A7A444c9e106Ad
 ├─ Function: price_w()
 └─ Inputs: []
```

**ACTION 6:** 
 Description: Call crvUSD ControllerFactory, function: add_market, add market for wstETH, A: 100, fee: .6%, admin fee: 0, price oracle contract: new oracle contract, monetary policy: new MonetaryPolicyV2 contract, loan discount: .09, liquidation discount: .06, debt ceiling: 150m crvUSD

```
 Call via agent (0x40907540d8a6C65c637785e8f8B742ae6b0b9968):
 ├─ To: 0xC9332fdCB1C491Dcc683bAe86Fe3cb70360738BC
 ├─ Function: add_market(address,uint256,uint256,uint256,address,address,uint256,uint256,uint256)
 └─ Inputs: ['0x7f39C581F595B53c5cb19bD0b3f8dA6c935E2Ca0', 100, 6000000000000000, 0, '0xc1793A29609ffFF81f10139fa0A7A444c9e106Ad', '0x1E7d3bf98d3f8D8CE193236c3e0eC4b00e32DaaE', 90000000000000000, 60000000000000000, 150000000000000000000000000]
```
