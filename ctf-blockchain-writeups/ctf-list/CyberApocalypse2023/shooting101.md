# **Shooting 101**

## **Description** 
```
Your metallic body might have advanced targeting systems, but hitting a target is not just about technical proficiency. 
To truly master the art of targeting, you must learn to trust your instincts and develop a keen sense of intuition. 
During this training, you will emerge as a skilled marksman who can hit the targets with deadly precision. 
It's about time to train and prove yourself in the Shooting Area, can you make it?
```

You can download the files [here](./downloadable/blockchain_shooting_101.zip)

**Setup.sol**
```solidity
pragma solidity ^0.8.18;

import {ShootingArea} from "./ShootingArea.sol";

contract Setup {
    ShootingArea public immutable TARGET;

    constructor() {
        TARGET = new ShootingArea();
    }

    function isSolved() public view returns (bool) {
        return TARGET.firstShot() && TARGET.secondShot() && TARGET.thirdShot();
    }
}
```

**ShootingArea.sol**
```solidity
pragma solidity ^0.8.18;

contract ShootingArea {
    bool public firstShot;
    bool public secondShot;
    bool public thirdShot;

    modifier firstTarget() {
        require(!firstShot && !secondShot && !thirdShot);
        _;
    }

    modifier secondTarget() {
        require(firstShot && !secondShot && !thirdShot);
        _;
    }

    modifier thirdTarget() {
        require(firstShot && secondShot && !thirdShot);
        _;
    }

    receive() external payable secondTarget {
        secondShot = true;
    }

    fallback() external payable firstTarget {
        firstShot = true;
    }

    function third() public thirdTarget {
        thirdShot = true;
    }
}
```