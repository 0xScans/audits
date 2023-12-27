# Degenerate (DGNRT) Smart Contract Audit 🛡️

## Overview
This document outlines the findings of the audit for the Degenerate (DGNRT) Smart Contract. The evaluation covers various aspects of security and compliance with best practices.

**Contract Address**: `0xF102f0F2157D9164EFc83841Ef91D2240EC912b2`

## Audit Summary
The Degenerate Smart Contract audit resulted in a safety score of **92/100** 🛡️.

## Audit Details

- **Arbitrary Jump/Storage Write**
  - Result: Pass

- **Centralization of Control**
  - Result: Medium
  - Details: Functions such as enableTrading and excludeFromFees are controlled by the owner, introducing a centralization risk.

- **Compiler Issues**
  - Result: Pass

- **Delegate Call to Untrusted Contract**
  - Result: Pass

- **Dependence on Predictable Variables**
  - Result: Pass

- **Token/Ether Theft**
  - Result: Pass

- **Flash Loans**
  - Result: Pass

- **Front Running**
  - Result: Low
  - Details: Potential vulnerability to front-running in DEX interactions, though impact is considered low.

- **Improper Events**
  - Result: Pass

- **Improper Authorization Scheme**
  - Result: Pass

- **Integer Over/Underflow**
  - Result: Pass
  - Details: Built-in overflow/underflow checks in Solidity 0.8.x.

- **Logical Issues**
  - Result: Pass

- **Oracle Issues**
  - Result: Pass

- **Outdated Compiler Version**
  - Result: Informational
  - Details: Uses Solidity 0.8.17; updating to the latest version is recommended.

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
  - Result: Pass

## Recommendations for Code Corrections

- **Centralization of Control**: Implement a multi-signature scheme or decentralized governance for critical owner-only functions.

- **Outdated Compiler Version**: Update to the latest Solidity version.
  ```solidity
  // Current version
  pragma solidity 0.8.17;

  // Recommended update
  pragma solidity ^0.8.17;
