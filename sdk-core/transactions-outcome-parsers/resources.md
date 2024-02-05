## DTOs needed for parsing token management transactions

```
dto TransactionEvent:
    address: string;
    identifier: string;
    topics: List[string]
    data: bytes
```

```
dto TransactionLogs:
    address: string;
    events: List[TransactionEvent];
```

```
dto SmartContractResult:
    hash: string;
    timestamp: int;
    sender: string;
    receiver: string;
    data: bytes;
    logs: TransactionLogs;
```

```
dto TransactionOutcome:
    smart_contract_results: List[SmartContractResult]
    transaction_logs: TransactionLogs
```
