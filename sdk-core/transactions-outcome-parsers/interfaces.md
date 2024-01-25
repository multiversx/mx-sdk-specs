## Interfaces needed for parsing token management transactions

```
dto ITransactionEvent:
    address: string;
    identifier: string;
    topics: List[string]
    data: string
```

```
dto ITransactionLogs:
    address: string;
    events: List[ITransactionEvent];
```

```
dto ITransactionResult:
    hash: string;
    timestamp: int;
    sender: string;
    receiver: string;
    data: string;
    original_tx_hash: string;
    minblock_hash: string;
    logs: ITransactionLogs;
```

```
dto ITransactionResultsAndLogsHolder:
    transaction_results: List[ITransactionResult]
    transaction_logs: ITransactionLogs
```
