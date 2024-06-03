## NetworkConfig

```
dto NetworkConfig:
    raw: any;

    chain_id: string;
    gas_per_data_byte: uint64;
    gas_price_modifier: float64;
    min_gas_limit: uint64;
    min_gas_price: uint64;
    extra_gas_limit_for_guarded_transactions: uint64;
    num_shards: uint32;
    round_duration: uint32;
    num_rounds_per_epoch: uint32;
    genesis_timestamp: uint64;
```

## AccountOnNetwork

```
dto AccountOnNetwork:
    raw: any;
    block_coordinates?: BlockCoordinates;

    address: string;
    nonce: uint64;
    balance: Amount;
    username: string;

    code_hash: bytes;
    code: bytes;
    developer_reward: Amount;
    owner_address: string;

    is_upgradable: bool;
    is_readable: bool;
    is_payable: bool;
    is_payable_by_smart_contract: bool;
    is_guarded: bool;
```

## BlockInfo

```
dto BlockCoordinates:
    nonce: string;
    hash: bytes;
    root_hash: bytes;
```

## TransactionOnNetwork

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

    sender: string;
    receiver: string;
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

    is_completed?: bool;
    status?: string;

    // See: core/transactions-outcome-parsers/resources.md.
    outcome: TransactionOutcome;
```
