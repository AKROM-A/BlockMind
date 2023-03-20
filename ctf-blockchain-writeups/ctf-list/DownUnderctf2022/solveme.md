# **SolveMe**

### **Description**  
```
Aight warm up time. All you gotta do is call the solve function. You can do it!

Goal: Call the solve function!

Author: @bluealder
```

SolveMe.sol
```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @title SolveMe
 * @author BlueAlder duc.tf
 */
contract SolveMe {
    bool public isSolved = false;

    function solveChallenge() external {
        isSolved = true;
    }
   
}
```