## INetworkProvider

```
interface INetworkProvider:
    // Fetches the general configuration of the network.
    get_network_config(): NetworkConfig;

    // Fetches the current status of the network.
    get_network_status(shard: number): NetworkStatus;

    // Fetches account information for a given address.
    get_account(address: Address): AccountOnNetwork;

    // Fetches the storage (key-value pairs) of an account.
    get_account_storage(address: Address): AccountStorage;

    // Fetches a specific storage entry of an account.
    get_account_storage_entry(address: Address, entry_key: string): AccountStorageEntry;

    // Waits until an account satisfies a given condition.
    // Can throw:
    // - ErrAwaitConditionNotReached
    await_account_on_condition(
        address: Address,
        condition: [data: AccountOnNetwork] => boolean,
        options?: AwaitingOptions
    ): AccountOnNetwork;

    // Broadcasts a transaction and returns its hash.
    send_transaction(transaction: Transaction): bytes;

    // Simulates a transaction.
    simulate_transaction(transaction: Transaction): TransactionOnNetwork;

    // Estimates the cost of a transaction.
    estimate_transaction_cost(transaction: Transaction): TransactionCostEstimationResponse

    // Broadcasts multiple transactions and returns a tuple of (number of accepted transactions, list of transaction hashes).
    // In the returned list, the order of transaction hashes corresponds to the order of transactions in the input list.
    // If a transaction is not accepted, its hash is empty in the returned list.
    send_transactions(transactions: List[Transaction]): Tuple[int, List[bytes]];

    // Fetches a transaction that was previously broadcasted (maybe already processed by the network).
    // Transaction status and outcome should be included in the response (if available).
    get_transaction(transaction_hash: bytes | string): TransactionOnNetwork;

    // Fetches the status of a transaction that was previously broadcasted (maybe already processed by the network).
    get_transaction_status(transaction_hash: bytes | string): TransactionStatus;

    // Waits until the transaction is completely processed.
    // Can throw:
    // - ErrAwaitConditionNotReached
    await_transaction_completed(
        transaction_hash: bytes | string,
        options?: AwaitingOptions
    ): TransactionOnNetwork;

    // Waits until a transaction satisfies a given condition.
    // Can throw:
    // - ErrAwaitConditionNotReached
    await_transaction_on_condition(
        transaction_hash: bytes | string,
        condition: [data: TransactionOnNetwork] => boolean,
        options?: AwaitingOptions
    ): TransactionOnNetwork;

    // Fetches the balance of an account, for a given token.
    // Able to handle both fungible and non-fungible tokens (NFTs, SFTs, MetaESDTs).
    get_token_of_account(address: Address, Token: token): TokenAmountOnNetwork;

    // Fetches the balances of an account, for all fungible tokens held by the account.
    // Pagination isn't explicitly handled by a basic network provider, but can be achieved by using `do_get_generic`.
    get_fungible_tokens_of_account(address: Address): List[TokenAmountOnNetwork];

    // Fetches the balances of an account, for all non-fungible tokens held by the account.
    // Pagination isn't explicitly handled by a basic network provider, but can be achieved by using `do_get_generic`.
    get_non_fungible_tokens_of_account(address: Address): List[TokenAmountOnNetwork];

    // Fetches the definition of a fungible token.
    get_definition_of_fungible_token(token_identifier: string): FungibleTokenMetadata;

    // Fetches the definition of a tokens collection.
    get_definition_of_tokens_collection(collection_name: string): TokensCollectionMetadata;

    // Queries a smart contract.
    query_contract(query: SmartContractQuery): SmartContractQueryResponse;

    // Calls the /cost endpoint
    estimate_gas_limit(transaction: Transaction): uint64;

    // Does a generic GET request against the network (handles API enveloping).
    do_get_generic(url: string, url_parameters?: Any): any;

    // Does a generic POST request against the network (handles API enveloping).
    do_post_generic(url: string, data: any, url_parameters?: Any): any;
```
