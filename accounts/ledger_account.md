## LedgerAccount

```
// should implement the `IAccount` interface
class LedgerAccount(IAccount):

    // sets the working address on the device
    set_address(address_index: int = 0);

    // serializes the transaction, computes the signature and returns it;
    // sets version and option for transaction signing by hash
    sign_transaction(transaction: Transaction, account_index: int = 0, address_index: int = 0): bytes;

    // serializes the message, computes the signature and returns it;
    sign_message(message: Message, account_index: int = 0, address_index: int = 0): bytes;

    // Gets the nonce (the one from the object's state) and increments it.
    get_nonce_then_increment(): uint64;
```
