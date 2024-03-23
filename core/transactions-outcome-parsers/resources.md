## Resources needed for parsing the outcome of transactions

```
dto TransactionEvent:
    address: string;
    identifier: string;
    topics: List[bytes]
    data: bytes
    // This is available since Sirius (protocol release).
    additionalData: List[bytes]
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

    // The return data (collection) of the contract endpoint invoked by the original transaction.
    return_data_parts: bytes[];

    // If not available, then it's identical to "return_code".
    return_message: string;
    return_code: string;
```

```
dto TransactionOutcome:
    // The direct outcome of a smart contract call. That is, the one that holds the "return data" of the endpoint invoked by the original transaction.
    direct_smart_contract_call_outcome: SmartContractCallOutcome;

    // All the smart contract calls produced when processing the transaction.
    smart_contract_results: List[SmartContractResult];

    // The logs produced when processing the transaction. Generally speaking, client code would also be interested in the logs within the smart contract results.
    logs: TransactionLogs;
```

### Operations on resources

The implementing libraries should also provide commonly-used operations on the resources defined above. For example:

```
// Finds the events (within the transaction outcome) that match a custom predicate (provided by the caller).
// Generally speaking, this function should search across all events. See "gather_all_events".
find_events_by_predicate(transaction_outcome: TransactionOutcome, predicate: (TransactionEvent) -> bool): List[TransactionEvent];

// Finds the events (within the transaction outcome) that have a specific identifier.
// Generally speaking, this function should search across all events. See "gather_all_events".
find_events_by_identifier(transaction_outcome: TransactionOutcome, identifier: string): List[TransactionEvent];

// Finds all the events within the transaction outcome, including those corresponding to smart contract results.
gather_all_events(transaction_outcome: TransactionOutcome): List[TransactionEvent];
```
