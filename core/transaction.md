## Transaction

```
dto Transaction:
    sender: string;
    receiver: string;
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
    guardian?: string;

    signature: bytes;
    guardianSignature?: bytes;
    relayer?: string;
    innerTransactions?: List[Transaction];
```
