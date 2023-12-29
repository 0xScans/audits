# LNDRY (LNDRY) Smart Contract Audit 🛡️

## Overview
This document presents the audit results for the LNDRY Smart Contract, assessing various aspects of security and best practices.

**Contract Address**: `0x613577bFEa8Ba6571F6b7a86716D04c80a3FbEb4`

## Audit Summary
The LNDRY Smart Contract audit resulted in a safety score of **90/100** 🛡️.

## Audit Details

- **Arbitrary Jump/Storage Write**
  - Result: Pass

- **Centralization of Control**
  - Result: Medium
  - Details: The contract has an Ownable pattern, giving the owner control over key functionalities like fee changes and limit removals.

- **Compiler Issues**
  - Result: Pass

- **Delegate Call to Untrusted Contract**
  - Result: Pass

- **Dependence on Predictable Variables**
  - Result: Pass

- **Ether/Token Theft**
  - Result: Pass

- **Flash Loans**
  - Result: Pass

- **Front Running**
  - Result: Medium
  - Details: Potential vulnerability to front-running due to interactions with DEXes like Uniswap.

- **Improper Events**
  - Result: Pass

- **Improper Authorization Scheme**
  - Result: Pass

- **Integer Over/Underflow**
  - Result: Pass
  - Details: Utilizes Solidity ^0.8.0 which includes overflow/underflow protection.

- **Logical Issues**
  - Result: Pass

- **Oracle Issues**
  - Result: Pass

- **Outdated Compiler Version**
  - Result: Informational
  - Details: Uses Solidity 0.8.19; updating to the latest stable version is advisable.

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
  - Details: Unused functions like increaseAllowance and decreaseAllowance could be removed for gas optimization.

## Recommendations for Code Corrections

- **Centralization of Control**: 
  Implement a time lock or multi-signature requirement for critical functions.

    ```solidity
    // Adding a time lock mechanism
    contract TimeLockedController {
        uint256 public constant delay = 2 days;
        uint256 public lastCallTime;

        modifier timeLocked() {
            require(block.timestamp - lastCallTime >= delay, "Action is time-locked");
            _;
            lastCallTime = block.timestamp;
        }

        function performCriticalAction() external timeLocked {
            // critical action code
        }
    }
    ```

- **Front Running**: 
  Introduce a commit-reveal scheme or use decentralized oracles for price feeds.

    ```solidity
    // Commit-reveal scheme to prevent front-running
    contract CommitReveal {
        mapping(bytes32 => address) public commit;
        mapping(address => uint256) public reveal;

        function commitAction(bytes32 _hash) external {
            require(commit[_hash] == address(0), "Action already committed");
            commit[_hash] = msg.sender;
        }

        function revealAction(uint256 _value, bytes32 _secret) external {
            bytes32 _hash = keccak256(abi.encodePacked(_value, _secret));
            require(commit[_hash] == msg.sender, "Invalid commit");
            reveal[msg.sender] = _value;
            // perform action after reveal
        }
    }
    ```

- **Unused Code**: 
  Remove functions that are not in use to optimize gas usage.

    ```solidity
    // Removing unused functions
    // Before
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        // ...
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        // ...
    }

    // After
    // Remove the above functions if they are not used
    ```


