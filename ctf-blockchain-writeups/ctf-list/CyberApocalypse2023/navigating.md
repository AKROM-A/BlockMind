# **Navigating the Unknown**

## **Description**  
```
Your advanced sensory systems make it easy for you to navigate familiar environments, but you must rely on intuition to 
navigate in unknown territories. Through practice and training, you must learn to read subtle cues and become comfortable 
in unpredictable situations. Can you use your software to find your way through the blocks?
```

First let's download the [files](./downloadable/blockchain_navigating_the_unknown.zip) 

**Setup.sol**
```solidity
pragma solidity ^0.8.18;

import {Unknown} from "./Unknown.sol";

contract Setup {
    Unknown public immutable TARGET;

    constructor() {
        TARGET = new Unknown();
    }

    function isSolved() public view returns (bool) {
        return TARGET.updated();
    }
}
```

**Unknown.sol**
```solidity
pragma solidity ^0.8.18;


contract Unknown {
    
    bool public updated;

    function updateSensors(uint256 version) external {
        if (version == 10) {
            updated = true;
        }
    }

}
```

## **Approach**
