## IBasicNetworkProvider

```
interface IBasicNetworkProvider:
    // Fetches the general configuration of the network.
    get_network_config(): NetworkConfig;

    // Fetches the current status of the network.
    get_network_status(shard: number): NetworkStatus;

    // Fetches a block by nonce or by hash.
    get_block(arguments: GetBlockArguments, url_parameters?: Any): BlockOnNetwork;

    // Fetches the latest block of a shard.
    get_latest_block(shard: number): BlockOnNetwork;

    // Fetches account information for a given address.
    // URL parameters can be used, for example, to provide block coordinates for deep-history lookups.
    get_account(address: IAddress, url_parameters?: Any): AccountOnNetwork;

    // Fetches the storage (key-value pairs) of an account.
    get_account_storage(address: IAddress, url_parameters?: Any): AccountStorage;

    // Fetches a specific storage entry of an account.
    get_account_storage_entry(address: IAddress, entry_key: string, url_parameters?: Any): AccountStorageEntry;

    // Waits until an account satisfies a given condition.
    // Can throw:
    // - ErrAwaitConditionNotReached
    await_account_on_condition(
        address: IAddress,
        condition: [data: AccountOnNetwork] => boolean,
        options?: AwaitingOptions
    ): AccountOnNetwork;

    // Broadcasts a transaction and returns its hash.
    send_transaction(transaction: Transaction): bytes;

    // Simulates a transaction.
    simulate_transaction(transaction: Transaction): TransactionSimulationResponse;

    // Estimates the cost of a transaction.
    estimate_transaction_cost(transaction: Transaction): TransactionCostEstimationResponse

    // Broadcasts multiple transactions and returns a tuple of (number of accepted transactions, list of transaction hashes).
    // In the returned list, the order of transaction hashes corresponds to the order of transactions in the input list.
    // If a transaction is not accepted, its hash is empty in the returned list.
    send_transactions(transactions: List[Transaction]): Tuple[int, List[bytes]];

    // Fetches a transaction that was previously broadcasted (maybe already processed by the network).
    // Transaction status and outcome should be included in the response (if available).
    get_transaction(transaction_hash: bytes | string, url_parameters?: Any): TransactionOnNetwork;

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
    get_token_of_account(address: IAddress, Token: token): TokenAmountOnNetwork;

    // Fetches the balances of an account, for all fungible tokens held by the account.
    // Pagination isn't explicitly handled by a basic network provider, but can be achieved by using the `url_parameters` argument.
    get_fungible_tokens_of_account(address: IAddress, url_parameters?: Any): List[TokenAmountOnNetwork];

    // Fetches the balances of an account, for all non-fungible tokens held by the account.
    // Pagination isn't explicitly handled by a basic network provider, but can be achieved by using the `url_parameters` argument.
    get_non_fungible_tokens_of_account(address: IAddress, url_parameters?: Any): List[TokenAmountOnNetwork];

    // Fetches the definition of a fungible token.
    get_definition_of_fungible_token(token_identifier: string, url_parameters?: Any): FungibleTokenMetadata;

    // Fetches the definition of a tokens collection.
    get_definition_of_tokens_collection(collection_name: string, url_parameters?: Any): TokensCollectionMetadata;

    // Queries a smart contract.
    // URL parameters can be used, for example, to provide block coordinates for deep-history lookups.
    query_contract(query: SmartContractQuery, url_parameters?: Any): SmartContractQueryResponse;

    // Does a generic GET request against the network (handles API enveloping).
    do_get_generic(url: string, url_parameters?: Any): any;

    // Does a generic POST request against the network (handles API enveloping).
    do_post_generic(url: string, data: any): any;
```
