## TransactionAwaiter

TransactionAwaiter allows one to await until a specific event (such as transaction completion) occurs on a given transaction.

We need a `TransactionFetcher` that fetches the transaction from the network.

```
interface ITransactionFetcher:
    get_transaction(tx_hash: string): TransactionOnNetwork;
```

```
class TransactionAwaiter:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally, it should receieve a reference to a `ITransactionFetcher` which ultimately can be a `NetworkProvider`. Could be similar to the one below.
    // If optional values not provided, should use some default ones
    constructor (
        fetcher: ITransactionFetcher,
        polling_interval_in_milliseconds?: int,
        timeout_in_milliseconds?: int,
        patience_in_milliseconds?: int
    );

    // Waits until the transaction is completely processed
    // Can throw ErrExpectedTransactionStatusNotReached
    await_completed(tx_hash: string): TransactionOnNetwork;

    // Waits until the condition is satisfied
    // Can throw ErrExpectedTransactionStatusNotReached
    await_on_condition(tx_hash: string, condition: [data: TransactionOnNetwork] => boolean): TransactionOnNetwork;
```
