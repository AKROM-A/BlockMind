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

This challenge is pretty straight forward, we need to call the isSolved Function after we modify the Sensors to 10 using the **updateSensors** function and use '10' as the value.

Here is the addresses I needed to solve this challenge

```
Private key     :  0x0dc1e9fad2d3a457b14eae02832793477b47ba57e3874c8880dcd1926db58dba
Address         :  0xB54DA7d0f1D92bDFc58E270Ef4443d06a8A065B4
Target contract :  0x4B4cfD341FdD38Ea36795d348DE88499B34A720e
Setup contract  :  0x4465FeeDCC09Df82C477b54635aa3FEDb12c1849
```
And the final payload would look like this.

```python
from web3 import Web3
from web3.middleware import geth_poa_middleware
import requests

rpc_url = "http://46.101.80.159:31608"

contract_address = "0x4B4cfD341FdD38Ea36795d348DE88499B34A720e" #TARGET

abi = '[{"inputs": [{"internalType": "uint256","name": "version","type": "uint256"}],"name": "updateSensors","outputs": [],"stateMutability": "nonpayable","type": "function"},{"inputs": [],"name": "updated","outputs": [{"internalType": "bool","name": "","type": "bool"}],"stateMutability": "view","type": "function"}]'
wallet_address = "0xB54DA7d0f1D92bDFc58E270Ef4443d06a8A065B4"
wallet_privatekey = "0x0dc1e9fad2d3a457b14eae02832793477b47ba57e3874c8880dcd1926db58dba"
web3 = Web3(Web3.HTTPProvider(rpc_url))
web3.middleware_onion.inject(geth_poa_middleware, layer=0)

contract = web3.eth.contract(address=contract_address,abi=abi)

nonce = web3.eth.get_transaction_count(wallet_address)


tx = {
    'nonce' : nonce,
    'gas' : 100000,
    'gasPrice' : web3.to_wei('4','gwei'),
    'from': wallet_address
}

transaction = contract.functions.updateSensors(10).build_transaction(tx)
signed_tx = web3.eth.account.sign_transaction(transaction, wallet_privatekey)
tx_hash = web3.eth.send_raw_transaction(signed_tx.rawTransaction)
transaction_hash = web3.to_hex(tx_hash)
tx_receipt = web3.eth.wait_for_transaction_receipt(tx_hash)
print(tx_receipt)
```

Notice the abi needed is only the setup one, because it's already import the 'unknown.sol'.

### **Flag**

```HTB{9P5_50FtW4R3_UPd4t3D}```