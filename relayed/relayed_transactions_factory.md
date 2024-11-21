## RelayedTransactionsFactory

```
class RelayedTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", gas limit for specific operations etc.

    // Each implementation is responsible to check that each inner transaction is signed.
    // Can throw InvalidInnerTransactionError

    create_relayed_v1_transaction({
        inner_transaction: Transaction;
        relayer_address: Address;
    }): Transaction;

    // can throw InvalidInnerTransactionError if inner_transaction.gas_limit != 0
    create_relayed_v2_transaction({
        inner_transaction: Transaction;
        inner_transaction_gas_limit: uint32;
        relayer_address: Address;
    }): Transaction;

    // the transaction should not be signed by the sender when the method is called
    // sets the relayer address and adds extra gasLimit for relayed V3 transactions
    create_relayed_v3_transaction({
        transaction: Transaction;
        relayer_address: Address;
    }): Transaction;
```
