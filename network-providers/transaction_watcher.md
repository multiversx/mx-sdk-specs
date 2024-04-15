## TransactionWatcher

TransactionWatcher allows one to continuously watch (monitor), by means of polling, the status of a given transaction.

We need a `TransactionFetcher` that fetches the transaction from the network.

```
interface ITransactionFetcher:
    get_transaction(tx_hash: string): TransactionOnNetwork;
```

```
class TransactionWatcher:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally, it should receieve a reference to a `ITransactionFetcher` which ultimately can be a `NetworkProvider`. Could be similar to the one below.
    // if optional values not provided, should use some default ones
    constructor (
        fetcher: ITransactionFetcher,
        polling_interval_in_milliseconds?: int,
        timeout_in_milliseconds?: int,
        patience_in_milliseconds?: int
    );

    // waits untill the transaction is completely processed
    // can throw ErrExpectedTransactionStatusNotReached
    await_completed(tx_hash: string): TransactionOnNetwork;

    // waits until the condition is satisfied
    // can throw ErrExpectedTransactionStatusNotReached
    await_on_condition(tx_hash: string, condition: [data: TransactionOnNetwork] => boolean): TransactionOnNetwork;
```
