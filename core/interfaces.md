## Interfaces

```
interface IAccount:
    address: Address;
    
    sign(data: bytes): bytes;
    sign_transaction(transaction: Transaction): bytes;
    verify_transactionSignature(transaction: Transaction, signature: Uint8Array): boolean;
    sign_message((message: Message): bytes;
    verify_messageSignature((message: Message, signature: Uint8Array): booleam;
```
