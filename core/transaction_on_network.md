## TransactionOnNetwork (and related resources)

```
dto TransactionOnNetwork:
    raw: any;

    hash: bytes;
    nonce: uint64;
    round: uint64;
    epoch: uint32;
    timestamp: uint64;
    block_hash: bytes;
    miniblock_hash: bytes;

    sender: Address;
    receiver: Address;
    sender_shard: uint32;
    receiver_shard: uint32;

    value: Amount;
    gas_limit: uint64;
    gas_price: uint64;
    function: string;
    data: bytes;
    version: uint32;
    options: uint32;
    signature: bytes;
    status: TransactionStatus;

    // All the smart contract calls produced when processing the transaction.
    smart_contract_results: List[SmartContractResult];

    // The logs produced when processing the transaction.
    logs: TransactionLogs;
```

```
dto TransactionEvent:
    raw: any;

    address: Address;
    identifier: Address;
    topics: List[bytes]

    // Before Sirius, within the Protocol, a log entry had the field "data".
    // Since Sirius, a new field, "additionalData" (collection) has been added to the log entry.
    // "additionalData" holds all the data items that correspond to the log entry, while the old (legacy) "data" field holds the first data item:
    // https://github.com/multiversx/mx-chain-go/blob/v1.6.18/process/transactionLog/process.go#L159
    data: bytes;
    additional_data: List[bytes]
```

```
dto TransactionLogs:
    address: Address;
    events: List[TransactionEvent];
```

```
dto SmartContractResult:
    raw: any;

    sender: Address;
    receiver: Address;
    data: bytes;
    logs: TransactionLogs;
```

### Operations on resources

The implementing libraries should also provide commonly-used operations on the resources defined above. For example:

```
// Finds the events (within the transaction) that match a custom predicate (provided by the caller).
// Generally speaking, this function should search across all events. See "gather_all_events".
find_events_by_predicate(transaction: TransactionOnNetwork, predicate: (TransactionEvent) -> bool): List[TransactionEvent];

// Finds the events (within the transaction) that have a specific identifier.
// Generally speaking, this function should search across all events. See "gather_all_events".
find_events_by_identifier(transaction: TransactionOnNetwork, identifier: string): List[TransactionEvent];

// Finds the events (within the transaction) that have a specific first topic (acting like an identifier for user-defined events).
// Generally speaking, this function should search across all events. See "gather_all_events".
find_events_by_first_topic(transaction: TransactionOnNetwork, topic: string): List[TransactionEvent];

// Finds all the events within the transaction, including those corresponding to smart contract results.
gather_all_events(transaction: TransactionOnNetwork): List[TransactionEvent];
```
