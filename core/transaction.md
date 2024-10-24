## Transaction

```
dto Transaction:
    sender: Address;
    receiver: Address;
    gasLimit: uint32;
    chainID: string;

    nonce?: uint64;
    value?: Amount;
    senderUsername?: string;
    receiverUsername?: string;
    gasPrice?: uint32;

    data?: bytes;
    version?: uint32;
    options?: uint32;
    guardian?: Address;

    signature: bytes;
    guardianSignature?: bytes;
    relayer?: string;
    innerTransactions?: List[Transaction];

    // Named constructor:
    new_from_plain_object(value: any): Transaction;

    // Conversion utility method:
    to_plain_object(): any;
```
