## Interfaces

```
interface IAccount:
    address: Address;
    
    sign(data: bytes): bytes;
    sign_transaction(transaction: Transaction): bytes;
    verify_transaction(transaction: Transaction): boolean;
    sign_message(transaction: Transaction): bytes;
    verify_message(transaction: Transaction): booleam;
```
