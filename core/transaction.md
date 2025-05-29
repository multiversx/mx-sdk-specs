## Transaction

```
dto Transaction:
    sender: Address;
    receiver: Address;
    gasLimit: uint64;
    chainID: string;

    nonce?: uint64;
    value?: Amount;
    senderUsername?: string;
    receiverUsername?: string;
    gasPrice?: uint64;

    data?: bytes;
    version?: uint32;
    options?: uint32;
    guardian?: Address;

    signature: bytes;
    guardianSignature?: bytes;
    relayer?: Address;
    relayerSignature?: bytes;

    // Named constructor:
    new_from_plain_object(value: any): Transaction;

    // Conversion utility method:
    to_plain_object(): any;
```
