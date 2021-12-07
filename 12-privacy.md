we can read the memory to find the key. the position is 5 since:

0. is the locked `bool`
1. `uint256` with the block timestamp
2. contains flattening, denomination and awkwardness (`uint8` + `uint8` + `uint16`)
3. data[0]
4. data[1]
5. data[2]

```
const data = await web3.eth.getStorageAt(instance, 5);
const key = data.slice(0, 34);
contract.unlock(key);
```
