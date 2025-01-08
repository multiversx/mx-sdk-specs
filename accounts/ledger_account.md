## LedgerAccount

```
// should implement the `IAccount` interface
class LedgerAccount(IAccount):

    // sets the working address on the device
    set_address(address_index: int = 0);

    // checks if set_address was previously called, throws error otherwise
    // serializes the transaction, computes the signature and returns it;
    // sets version and option for transaction signing by hash
    sign_transaction(transaction: Transaction): bytes;

    // checks if set_address was previously called, throws error otherwise
    // serializes the message, computes the signature and returns it;
    sign_message(message: Message): bytes;

    // Gets the nonce (the one from the object's state) and increments it.
    get_nonce_then_increment(): uint64;
```
