call destroy to withdraw eth balance in contract

```
const data = web3.eth.abi.encodeFunctionCall({
    name: 'destroy',
    type: 'function',
    inputs: [{
        type: 'address',
        name: '_to'
    }]
}, [player]);

sendTransaction({ from: player, to: '<token contract address>', data });
```
