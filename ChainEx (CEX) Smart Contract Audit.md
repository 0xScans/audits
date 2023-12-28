# ChainEx (CEX) Smart Contract Audit🛡️

## Overview
This document presents the audit results for the ChainEx (CEX) Smart Contract. The analysis focuses on identifying vulnerabilities, issues, potential threats, and problems for investors.

**Contract Address**: `0xd01D133166820557db7138963BCD9009C54E4C33`

## Audit Summary
The ChainEx Smart Contract audit resulted in a safety score of **87/100**.

## Audit Details

- **Arbitrary Jump/Storage Write**
  - Result: Pass

- **Centralization of Control**
  - Result: Medium
  - Details: The contract has an onlyOwner modifier which centralizes control over certain functions such as launchtoken, removeTxLimits, updateSandwichBlockerEnabled, and others. This could pose a risk if the owner's account is compromised.

- **Compiler Issues**
  - Result: Pass

- **Delegate Call to Untrusted Contract**
  - Result: Pass

- **Dependence on Predictable Variables**
  - Result: Low
  - Details: The contract uses block.number for anti-sandwich protection which can be predicted by miners.

- **Ether/Token Theft**
  - Result: Pass

- **Flash Loans**
  - Result: Pass

- **Front Running**
  - Result: Medium
  - Details: The contract attempts to mitigate front-running with antiSandwichEnabled, but it may not be fully effective as it relies on block.number.

- **Improper Events**
  - Result: Pass

- **Improper Authorization Scheme**
  - Result: Pass

- **Integer Over/Underflow**
  - Result: Pass
  - Details: The contract uses Solidity 0.8.x which has built-in overflow/underflow checks.

- **Logical Issues**
  - Result: Informational
  - Details: The contract has a renounceDevTax function that allows the dev address to renounce their tax, which could lead to unexpected behavior if not communicated properly to the token holders.

- **Oracle Issues**
  - Result: Pass

- **Outdated Compiler Version**
  - Result: Pass
  - Details: The contract uses a recent compiler version (0.8.20).

- **Race Conditions**
  - Result: Pass

- **Reentrancy**
  - Result: Pass

- **Signature Issues**
  - Result: Pass

- **Sybil Attack**
  - Result: Pass

- **Unbounded Loops**
  - Result: Pass

- **Unused Code**
  - Result: Informational
  - Details: There are some variables and functions that may not be used, such as tokensForTax.gasSaver which is set but never used in the contract logic.

## Recommendations for Code Corrections

- **Centralization of Control**: 
  - Implement a multi-signature scheme or decentralized governance for critical functions.
  
    ```solidity
    // Before
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    // After
    // Consider implementing a multi-signature scheme or a decentralized governance process for critical functions.
    ```

- **Dependence on Predictable Variables**: 
  - Use less predictable mechanisms for anti-sandwich protection.
  
    ```solidity
    // Before
    require(_holderLastTransferBlock[from] < block.number, "Anti MEV");

    // After
    // Consider using a more unpredictable variable or mechanism to prevent front-running.
    ```

- **Front Running**: 
  - Add additional anti-front-running techniques like commit-reveal schemes.

    ```solidity
    // Before
    if (antiSandwichEnabled){
        if(isAMMPair[to]){
            require(_holderLastTransferBlock[from] < block.number, "Anti MEV");
        } else {
            _holderLastTransferBlock[to] = block.number;
            _holderLastTransferBlock[tx.origin] = block.number;
        }
    }

    // After
    // Implement additional measures such as commit-reveal schemes or other anti-front-running techniques.
    ```

- **Unused Code**: 
  - Remove or implement logic for unused variables.

    ```solidity
    // Before
    bool gasSaver; // Set but never used

    // After
    // Remove unused variables or implement the logic where they are used.
    ```

