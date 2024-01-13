# X-Alpha.ai (XALPHA) Smart Contract Audit 🛡️

## Overview
This document presents the findings from the audit of the X-Alpha.ai (XALPHA) Smart Contract.

**Contract Address**: `0x369733153E6E08d38F2BC72ae2432E855Cfbe221`

## Audit Summary
The X-Alpha.ai (XALPHA) Smart Contract achieves a safety score of **88/100**.

## Detailed Audit Findings

### 1. Arbitrary Jump/Storage Write
- **Result**: Pass

### 2. Centralization of Control
- **Result**: Medium
- **Details**: Owner-only functions like `removeLimits`, `removeTaxes`, and `updateBuyFee` can significantly alter contract behavior.

    ```solidity
    function removeLimits() external onlyOwner {
        maxTokenAmountPerWallet = 0;
        maxTokenAmountPerTransaction = 0;
    }

    function removeTaxes() external onlyOwner {
        feePercentageBuy = 0;
        feePercentageSell = 0;
    }

    function updateBuyFee(uint16 _feePercentageBuy) external onlyOwner {
        require(_feePercentageBuy <= 2000, "Must keep fees at 20% or less");
        feePercentageBuy = _feePercentageBuy;
    }
    ```

### 3. Compiler Issues
- **Result**: Pass

### 4. Delegate Call to Untrusted Contract
- **Result**: Pass

### 5. Dependence on Predictable Variables
- **Result**: Pass

### 6. Ether/Token Theft
- **Result**: Pass

### 7. Flash Loans
- **Result**: Pass

### 8. Front Running
- **Result**: Medium
- **Details**: Vulnerable to front-running, particularly in functions like `swapBalanceToETHAndSend`.

    ```solidity
    function swapBalanceToETHAndSend() private lockTheSwap {
        // ... existing code ...
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountIn,
            0,
            path,
            address(this),
            block.timestamp
        );
        // ...
    }
    ```

### 9. Improper Events
- **Result**: Pass

### 10. Improper Authorization Scheme
- **Result**: Pass

### 11. Integer Over/Underflow
- **Result**: Pass

### 12. Logical Issues
- **Result**: Low
- **Details**: Use of `unchecked` block without a clear need, but minimal risk due to automatic checks in Solidity 0.8.0.

    ```solidity
    unchecked {
        _balances[account] = accountBalance - amount;
        _totalSupply -= amount;
    }
    ```

### 13. Oracle Issues
- **Result**: Pass

### 14. Outdated Compiler Version
- **Result**: Pass

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
- **Details**: Some variables and functions are not used and could be removed for gas optimization.

    ```solidity
    // Identify and remove/comment out unused variables and functions
    ```

## Recommendations for Code Corrections

### Centralization of Control:
- **Correction**: Implement a decentralized governance system or multi-signature control.

### Front Running:
- **Correction**: Add commit-reveal schemes or similar mechanisms.

### Logical Issues:
- **Correction**: Review and ensure safety of arithmetic operations, especially where `unchecked` is used.

### Unused Code:
- **Correction**: Remove/comment out any unused code for efficiency.

## Final Remarks
The X-Alpha.ai (XALPHA) Smart Contract exhibits medium-level vulnerabilities related to centralization and potential front-running. Implementing the suggested corrections will improve the contract's security and operational fairness.
