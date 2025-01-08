## LedgerAccount

```
class LedgerAccount:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Suggestions:
    constructor(account_index: int = 0, address_index: int = 0);

    address: Address;

    // Local copy of the account nonce.
    // Must be explicitly managed by client code.
    nonce: uint64;

    // serializes the transaction, signs the transaction, returns the signature;
    sign_transaction(transaction: Transaction, account_index: int = 0, address_index: int = 0, sign_using_hash: boolean = True): bytes;

    // serializes the message, signs the message, returns the signature;
    sign_message(message: Message, account_index: int = 0, address_index: int = 0): bytes;

    // Gets the nonce (the one from the object's state) and increments it.
    get_nonce_then_increment(): uint64;
```
