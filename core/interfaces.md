## Interfaces

```
interface IAccount:
    address: Address;
    
    sign(data: bytes): bytes;
    sign_transaction(transaction: Transaction): bytes;
    verify_transactionSignature(transaction: Transaction): boolean;
    sign_message(transaction: Transaction): bytes;
    verify_messageSignature(transaction: Transaction): booleam;
```
