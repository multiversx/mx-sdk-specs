## TransactionsConverter

```
class TransactionsConverter:
    transaction_to_plain_object(transaction: Transaction): any;
    plain_object_to_transaction(object: any): Transaction;
    transaction_on_network_to_outcome(transaction: TransactionOnNetwork): TransactionOutcome;
```

Note: what a plain object means is relative to the implementation language. For example, in Python, it could be a `dict`, while in TypeScript, it could be an `object`.

Note: `TransactionOnNetwork` isn't yet captured in the specs, but it's already present in some implementations of the `network-providers` package (e.g. in Python and TypeScript).
