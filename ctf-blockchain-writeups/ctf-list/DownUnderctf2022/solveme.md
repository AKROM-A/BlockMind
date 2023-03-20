# **SolveMe**

## **Description**  
```
Aight warm up time. All you gotta do is call the solve function. You can do it!

Goal: Call the solve function!

Author: @bluealder
```

**SolveMe.sol**
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

## **Approach**

This Challenge is straight forward, to get the flag we just need to call the "SolveMe" Function. Since this CTF use a Private Network, the addresses for the contract, wallet and private key may differ for each player.

### **ABI**

As for the ABI you can get it from REMIX IDE online after you compile the contract.
```json
[
	{
		"inputs": [],
		"name": "isSolved",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "solveChallenge",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	}
]
```

### **Solver.py**
```python
from web3 import Web3
from web3.middleware import geth_poa_middleware
import requests

#PROVIDER                            
provider_url = "https://blockchain-solveme-d0312df76a889edb-eth.2022.ductf.dev/"
web3 = Web3(Web3.HTTPProvider(provider_url))
web3.middleware_onion.inject(geth_poa_middleware, layer=0)

#ADDRESSES
wallet_address = "YOUR_WALLET_ADDRESS_HERE"
wallet_privatekey = "0xf64792a746f00890a419e6d69525dab7b95fb5fc4889625d01fa7917e1a0447f"
contract_address = "0xfb7ACc4e787dcd2a53b5AcCF91cB868839E34cBF"

#ABI
abi = ''[{"inputs":[],"name":"isSolved","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"solveChallenge","outputs":[],"stateMutability":"nonpayable","type":"function"}]''

contract = web3.eth.contract(address=contract_address,abi=abi)
nonce = web3.eth.get_transaction_count(wallet_address)

tx = {
    'nonce' : nonce,
    'gas' : 100000,
    'gasPrice' : web3.to_wei('4','gwei'),
    'from': wallet_address
}

#Calling solveChallenge()
transaction = contract.functions.solveChallenge().build_transaction(tx)
signed_tx = web3.eth.account.sign_transaction(transaction, wallet_privatekey)
tx_hash = web3.eth.send_raw_transaction(signed_tx.rawTransaction)
transaction_hash = web3.to_hex(tx_hash)
tx_receipt = web3.eth.wait_for_transaction_receipt(tx_hash)
print(tx_receipt)
```

## FLAG
to get the flag just go to the `/challenge/solve` endpoint.
```**DUCTF{muM_1_did_a_blonkchain!}**```