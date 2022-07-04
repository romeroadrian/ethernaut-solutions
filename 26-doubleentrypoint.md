LegacyToken delegates transfers to DET. If we sweep the vault using the LGT address it will end up draining the DET token. We can detect this by checking the `origSender` value

```
contract VaultBot is IDetectionBot {
    address vault;

    constructor(address _vault) {
        vault = _vault;
    }

    function handleTransaction(address user, bytes calldata msgData) external {
      (,, address origSender) = abi.decode(msgData[4:], (address,uint256,address));

        if (origSender == vault) {
            IForta(msg.sender).raiseAlert(user);
        }
    }
}
```
