# 0xEngage (ENGAGE) Smart Contract Audit🛡️


## Overview
This document presents the findings from the audit of the 0xEngage (ENGAGE) Smart Contract, focusing on security aspects and best practices.

**Contract Address**: `0x3632dc4d741bfa40Dbf3B23b63Dd3A25a3061DE3`

## Audit Summary
The 0xEngage (ENGAGE) Smart Contract achieved a safety score of **86/100**.

## Detailed Audit Findings

### 1. Arbitrary Jump/Storage Write
- **Result**: Pass

### 2. Centralization of Control
- **Result**: Medium
- **Details**: The contract utilizes the Ownable pattern, granting the owner exclusive control over various critical functions. These include:
  - `enableTrading()`
  - `removeLimits()`
  - `updateMaxTxnAmount(uint256 newNum)`
  - `updateMaxWalletAmount(uint256 newNum)`
  - `excludeFromMaxTransaction(address updAds, bool isEx)`
  - `updateSwapEnabled(bool enabled)`
  - `updateBuyFees(uint256 _marketingFee, uint256 _liquidityFee, uint256 _treasuryFee)`
  - `updateSellFees(uint256 _marketingFee, uint256 _liquidityFee, uint256 _treasuryFee)`
  - `excludeFromFees(address account, bool excluded)`
  - `setAutomatedMarketMakerPair(address pair, bool value)`
  - `updateMarketingWallet(address newMarketingWallet)`
  - `updateTreasuryWallet(address newTreasuryWallet)`
  - `setAutoLPBurnSettings(uint256 _frequencyInSeconds, uint256 _percent, bool _Enabled)`
  - `manualBurnLiquidityPairTokens(uint256 percent)`

### 3. Compiler Issues
- **Result**: Pass

### 4. Delegate Call to Untrusted Contract
- **Result**: Pass

### 5. Dependence on Predictable Variables
- **Result**: Medium
- **Details**: The contract's functions `autoBurnLiquidityPairTokens()` and `manualBurnLiquidityPairTokens(uint256 percent)` rely on `block.timestamp`, which might be manipulated by miners.

### 6. Ether/Token Theft
- **Result**: Pass

### 7. Flash Loans
- **Result**: Pass

### 8. Front Running
- **Result**: Medium
- **Details**: Potential for front-running in functions involving token transfers or liquidity operations.

### 9. Improper Events
- **Result**: Pass

### 10. Improper Authorization Scheme
- **Result**: Pass

### 11. Integer Over/Underflow
- **Result**: Pass
- **Details**: Uses SafeMath for arithmetic operations.

### 12. Logical Issues
- **Result**: Pass

### 13. Oracle Issues
- **Result**: Pass

### 14. Outdated Compiler Version
- **Result**: Informational
- **Details**: Solidity version 0.8.10 is used, which is not the latest but still recent.

### 15. Race Conditions
- **Result**: Pass

### 16. Reentrancy
- **Result**: Pass

### 17. Signature Issues
- **Result**: Pass

### 18. Sybil Attack
- **Result**: Pass

### 19. Unbounded Loops
- **Result**: Pass

### 20. Unused Code
- **Result**: Informational
- **Details**: Presence of unused functions such as `renounceOwnership()`.

## Recommendations for Code Corrections

- **Centralization of Control**: Implement a multi-signature scheme or decentralized governance model.
- **Dependence on Predictable Variables**: Use decentralized oracles or additional checks.
- **Front Running**: Add commit-reveal schemes or private transaction mechanisms.
- **Unused Code**: Remove functions like `renounceOwnership()` if not needed.

## Final Thoughts
The audit identifies areas for improvement in centralization, predictable variables, and front-running vulnerabilities. Addressing these will enhance the security and functionality of the 0xEngage (ENGAGE) Smart Contract.
