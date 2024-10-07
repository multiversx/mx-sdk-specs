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

## NetworkStatus

```
dto NetworkStatus:
    raw: any;

    block_timestamp: uint64;
    block_nonce: uint64;
    highest_final_block_nonce: uint64;
    current_round: uint64;
    current_epoch: uint32;
```

## GetBlockArguments

```
dto GetBlockArguments:
    // Some implementations might require the shard (e.g. BasicProxyNetworkProvider).
    shard?: uint32;
    block_nonce?: uint64;
    block_hash?: bytes;
```

## BlockOnNetwork

```
dto BlockOnNetwork:
    raw: any;

    shard: uint32;
    nonce: uint64;
    hash: bytes;
    previous_hash: bytes;
    timestamp: uint64;
    round: uint64;
    epoch: uint32;
```

## AccountOnNetwork

```
dto AccountOnNetwork:
    raw: any;
    block_coordinates?: BlockCoordinates;

    address: string;
    nonce: uint64;
    balance: Amount;
    username?: string;

    contract_code_hash?: bytes;
    contract_code?: bytes;
    contract_developer_reward?: Amount;
    contract_owner_address?: string;

    is_contract_upgradable?: bool;
    is_contract_readable?: bool;
    is_contract_payable?: bool;
    is_contract_payable_by_contract?: bool;

    is_guarded: bool;
```

## AccountStorage

```
dto AccountStorage:
    raw: any;
    block_coordinates?: BlockCoordinates;

    entries: AccountStorageEntry[];
```

## AccountStorageEntry

```
dto AccountStorageEntry:
    raw: any;
    block_coordinates?: BlockCoordinates;

    key: string;
    value: bytes;
```

## BlockInfo

```
dto BlockCoordinates:
    nonce: string;
    hash: bytes;
    root_hash: bytes;
```
## TransactionSimulationResponse

```
dto TransactionSimulationResponse:
    raw: any;

    status: TransactionStatus;
    outcome: TransactionOutcome;
```

## TransactionCostResponse

```
dto TransactionCostResponse:
    raw: any;

    gas_limit: uint64;
    status: TransactionStatus;
    outcome: TransactionOutcome;
```

## AwaitingOptions

```
dto AwaitingOptions:
    polling_interval_in_milliseconds: int;
    timeout_in_milliseconds: int;
    patience_in_milliseconds: int;
```

## TokenAmountOnNetwork

```
dto TokenAmountOnNetwork implements TokenAmount:
    raw: any;
    block_coordinates?: BlockCoordinates;

    token: Token;
    amount: Amount;
```

## FungibleTokenMetadata

```
dto FungibleTokenMetadata:
    raw: any;

    identifier: string;
    name: string;
    ticker: string;
    owner: string;
    decimals: number;
```

## TokensCollectionMetadata

```
dto TokensCollectionMetadata:
    raw: any;

    collection: string;
    type: string;
    name: string;
    ticker: string;
    owner: string;
    decimals: number;
```
