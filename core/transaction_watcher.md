## TransactionWatcher

TransactionWatcher allows one to continuously watch (monitor), by means of polling, the status of a given transaction.

We need a 'TransactionFetcher` that fetches the transaction from the network.

```
interface TransactionFetcher:
    get_transaction(tx_hash: string): TransactionOnNetwork;
```

```
class TransactionWatcher:
    // The default polling interval for the `TransactionFetcher`. Can be 6000ms (6s)
    static default_polling_interval;

    // The default timeout, in milliseconds.
    static default_timeout;

    // An extra time (in milliseconds) to wait after the transaction has reached the desired status.
    static default_patience;

    // if optional values not provided, should use the default ones
    constructor (
        fetcher: TransactionFetcher,
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
