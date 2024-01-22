# Gift (GIFT) Smart Contract Audit 🛡️

## Overview
This document presents the audit results for the Gift (GIFT) Smart Contract, focusing on security and best practices.

**Contract Address**: `0x2129Ced607135e9ce23d5f0cB6Ba926e838E2a38`

## Audit Summary
The Gift (GIFT) Smart Contract has achieved a safety score of **84/100**.

## Detailed Audit Findings

### 1. Arbitrary Jump/Storage Write
- **Result**: Pass

### 2. Centralization of Control
- **Result**: Medium
- **Details**: Owner-only functions like `enableTrading`, `removeLimits`, and others pose centralization risks.

### 3. Compiler Issues
- **Result**: Pass

### 4. Delegate Call to Untrusted Contract
- **Result**: Pass

### 5. Dependence on Predictable Variables
- **Result**: Pass

### 6 - Ether/Token Theft
- **Result**: Medium

Details: The `swapBack` function sends the entire contract's balance to the owner after taking a fee for the
`TAX_ADDRESS`. This could potentially allow the owner to drain the contract of ETH, which may not be in the best
interest of the token holders.
Code:
<i>
```solidity
(success, ) = address(owner()).call{value: address(this).balance}("");
```
</i>
Correction:
<i>
```solidity
// Instead of sending the entire balance to the owner, a predefined percentage or fixed amount should be sent.
uint256 ownerAmount = address(this).balance.mul(ownerPercentage).div(100);
(success, ) = address(owner()).call{value: ownerAmount}("");
```
</i>

### 7 - Flash Loans
- **Result**: Pass
  
### 8 - Front Running
- **Result**: Low
Details: The contract does not appear to have specific protections against front-running attacks, especially since it
interacts with a DEX (Uniswap). However, the impact is considered low because the typical mitigations for front-running
are complex and often involve significant architectural changes.

### 9 - Improper Events
- **Result**: Pass

### 10 - Improper Authorization Scheme
- **Result**: Pass
  
### 11 - Integer Over/Underflow
- **Result**: Pass
Details: The contract uses SafeMath for all arithmetic operations, which protects against overflows and underflows.

### 12 - Logical Issues
- **Result**: Medium
Details: The `TAX_FEE` is set to 100%, which means the entire amount of ETH received from the `swapBack` function is
sent to the `TAX_ADDRESS`. This is likely a logical error, as it would not leave any ETH for other purposes such as
liquidity provision or project development.
Code:
<i>
```solidity
uint256 public constant TAX_FEE = 100; // 100% tax fee
```
</i>
Correction:
<i>
```solidity
uint256 public constant TAX_FEE = 10; // 10% tax fee, or another reasonable percentage
```
</i>

### 13 - Oracle Issues
- **Result**: Pass

### 14 - Outdated Compiler Version
- **Result**:Pass

### 15 - Race Conditions
- **Result**: Pass

### 16 - Reentrancy
- **Result**: Pass

### 17 - Signature Issues
- **Result**: Pass

### 18 - Sybil Attack
- **Result**: Pass

### 19 - Unbounded Loops
- **Result**: Pass

### 20 - Unused Code
- **Result**: Informational

Details: There are commented-out imports for `ERC20Burnable` and `ERC20Pausable` which are not used in the contract.
It's good practice to remove unused code to reduce contract size and potential confusion.
Code:
<i>
```solidity
// import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
// import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Pausable.sol";
```
</i>
Correction:
<i>
```solidity
// Remove the unused imports
```
</i>
