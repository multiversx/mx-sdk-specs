## TransactionEvents

A dto containing the transaction events.

```
dto TransactionEvent:
    address: string;
    identifier: string;
    topics: List[string];
    data: string;
```

## TransactionLogs

A dto containing the logs of the transaction.

```
dto TransactionLogs:
    address: string;
    events: List[TransactionEvents];
```
