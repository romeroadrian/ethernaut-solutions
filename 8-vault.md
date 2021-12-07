contract memory is present in the blockchain and can be read even if the contract variable isn't public

```
const password = await web3.eth.getStorageAt(instance, 1);
contract.unlock(password);
```
