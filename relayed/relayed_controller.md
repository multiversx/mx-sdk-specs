## RelayedController

Promotes all the functionality of `RelayedTransactionsFactory` and `RelayedTransactionsOutcomeParser`.

```
class RelayedController:
    // the transaction should not be signed by the sender when the method is called
    // sets the relayer address, adds extra gasLimit, signs and sets the `relayerSignature` field
    // after calling this method, the `transaction.sender` should also sign the transaction
    create_relayed_v3_transaction({
        sender: IAccount;
        transaction: Transaction;
    }): Transaction;
```
