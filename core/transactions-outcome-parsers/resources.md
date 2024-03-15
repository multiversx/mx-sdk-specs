## DTOs needed for parsing the outcome of transactions

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
    sender: string;
    receiver: string;
    data: bytes;
    logs: TransactionLogs;
```

```
dto SmartContractCallOutcome:
    function: string;
    return_data_parts: bytes[];
    return_message: string;
    return_code: string;
```

```
dto TransactionOutcome:
    direct_smart_contract_call_outcome: SmartContractCallOutcome;
    smart_contract_results: List[SmartContractResult];
    logs: TransactionLogs;
```
