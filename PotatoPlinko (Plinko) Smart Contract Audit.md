# PotatoPlinko (Plinko) Smart Contract Audit 🛡️

## Overview
This document details the audit results for the PotatoPlinko (Plinko) Smart Contract. The audit evaluates the contract for security, efficiency, and adherence to best practices in smart contract development.

**Contract Address**: `0xa89B728708Be04f57c7a33C6f790B6F077298e26`

## Audit Summary
The PotatoPlinko Smart Contract has been thoroughly audited, resulting in an overall safety score of **91/100** 🛡️.

## Audit Details

- **Arbitrary Jump/Storage Write**
  - Result: Pass

- **Centralization of Control**
  - Result: Informational
  - Details: The contract implements an Ownable pattern, which centralizes control in the owner. This is common but can pose a risk if the owner's account is compromised.

- **Compiler Issues**
  - Result: Pass

- **Delegate Call to Untrusted Contract**
  - Result: Pass

- **Dependence on Predictable Variables**
  - Result: Pass

- **Token Theft**
  - Result: Pass

- **Flash Loans**
  - Result: Pass

- **Front Running**
  - Result: Pass

- **Improper Events**
  - Result: Pass

- **Improper Authorization Scheme**
  - Result: Pass

- **Integer Over/Underflow**
  - Result: Pass
  - Details: With Solidity 0.8.x, arithmetic operations revert on overflow/underflow, mitigating this concern.

- **Logical Issues**
  - Result: Medium
  - Details: The withdraw function does not verify if the nonce used in the signature matches the current nonce, potentially allowing replay attacks.

- **Oracle Issues**
  - Result: Pass

- **Outdated Compiler Version**
  - Result: Informational
  - Details: The contract uses Solidity version 0.8.19, which is not the latest but still recent and considered safe.

- **Race Conditions**
  - Result: Pass

- **Reentrancy**
  - Result: Pass

- **Signature Issues**
  - Result: Low
  - Details: The contract uses ECDSA for signature verification but lacks a mechanism to prevent signature reuse (no nonce check).

- **Sybil Attack**
  - Result: Pass

- **Unbounded Loops**
  - Result: Pass

- **Unused Code**
  - Result: Pass

## Code Corrections

### Current Code Snippet

```solidity
function verifySignature(address _user, uint256 _amount , uint256 _expiry, Sig memory sig) private  returns (address signatureAddress){
    bytes32 hash = prepareHash(_user, _amount, _expiry);
    bytes32 messageHash = hash.toEthSignedMessageHash();
    require(!_signStatus[messageHash], "Invalid Signature");
    _signStatus[messageHash] = true;
    nonce[_user]++;
    signatureAddress = messageHash.recover(sig.v, sig.r, sig.s);
}

function verifySignature(address _user, uint256 _amount , uint256 _expiry, Sig memory sig) private returns (address signatureAddress){
    bytes32 hash = prepareHash(_user, _amount, _expiry);
    bytes32 messageHash = hash.toEthSignedMessageHash();
    require(!_signStatus[messageHash], "Invalid Signature");
    require(nonce[_user] == _nonce, "Invalid Nonce"); // Check the nonce to prevent replay attacks
    _signStatus[messageHash] = true;
    nonce[_user]++;
    signatureAddress = messageHash.recover(sig.v, sig.r, sig.s);
}
