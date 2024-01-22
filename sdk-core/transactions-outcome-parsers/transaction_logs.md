## TransactionEvents

A class containing the transaction events.

```
class TransactionEvents:
    address: string;
    identifier: string;
    topics: List[string];
    data: string;
```

## TransactionLogs

A class containing the logs of the transaction.

```
class TransactionLogs:
    address: string;
    events: List[TransactionEvents];
```
