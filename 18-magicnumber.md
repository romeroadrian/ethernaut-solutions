build bytecode by hand using https://ethervm.io/

store 42 at memory 0x0 then return memory 0x0:

- `PUSH 0x2A` (42)
- `PUSH 0x00` (memory location 0)
- `MSTORE`
- `PUSH 0x20` (length)
- `PUSH 0x00` (memory location 0)
- `RETURN`

`602A 6000 52 6020 6000 F3`

deploy code [1]:

- `PUSH 0x0A` (length of runtime code, 10 bytes)
- `PUSH 0x0C` (offset to runtime code, deploy code is 12 bytes so offset is 13)
- `PUSH 0x00` (destination offset)
- `CODECOPY`
- `PUSH 0x0A` (length of code, 10 bytes)
- `PUSH 0x00` (offset is 0)
- `RETURN`

`600A 600C 6000 39 600A 6000 F3`

And send the transaction to create the contract:

```
sendTransaction({
  from: player,
  to: null,
  data: '600A600C600039600A6000F3602A60005260206000F3'
});
```

[1]: https://medium.com/@hayeah/diving-into-the-ethereum-vm-part-5-the-smart-contract-creation-process-cb7b6133b855
