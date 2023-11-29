## ApiNetworkProvider

Sometimes the `ApiNetworkProvider` class uses an underlying `ProxyNetworkProvider`as some of the API routes are forwarded to the proxy.

```
class ApiNetworkProvider:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Should specify the url to query and have a backing proxy provider
    
    do_get_generic(resource_url: string): Dict[string, Any];
    
    do_post_generic(resource_url: string, payload: any): Dict[string, Any];

    // these methods use the `backing proxy provider`
    get_network_config(): NetworkConfig;

    get_network_status(shard: int = METACHAIN_ID): NetworkStatus;

    // returns None if transaction was not sent
    send_transactions(transactions: List[ITransaction]): List[str | None];

    // the following methods use the API
    get_guardian_data(address: IAddress): GuardianData;

    get_network_gas_configs(): Dict[string, Any];

    get_network_stake_statistics(): NetworkStake;

    get_network_general_statistics(): NetworkGeneralStatistics;

    get_account(address: IAddress): AccountOnNetwork;

    get_fungible_tokens_of_account(address: IAddress, pagination: IPagination = DefaultPagination()): List[FungibleTokenOfAccountOnNetwork];

    get_nonfungible_tokens_of_account(address: IAddress, pagination: IPagination = DefaultPagination()): List[NonFungibleTokenOfAccountOnNetwork];

    get_fungible_token_of_account(address: IAddress, token_identifier: string): FungibleTokenOfAccountOnNetwork;

    get_nonfungible_token_of_account(address: IAddress, collection: string, nonce: int): NonFungibleTokenOfAccountOnNetwork;

    get_definition_of_fungible_token(token_identifier: string): DefinitionOfFungibleTokenOnNetwork;

    get_definition_of_token_collection(collection: string): DefinitionOfTokenCollectionOnNetwork;

    get_non_fungible_token(collection: string, nonce: int): NonFungibleTokenOfAccountOnNetwork;

    query_contract(query: IContractQuery): ContractQueryResponse;

    get_transaction(tx_hash: string): TransactionOnNetwork;

    get_account_transactions(address: IAddress, pagination: IPagination = DefaultPagination()): List[TransactionOnNetwork];

    get_bunch_of_transactions(tx_hashes: List[string], with_block_info: bool = False, with_results: bool = False): List[TransactionOnNetwork];

    get_transaction_status(tx_hash: string): TransactionStatus;

    send_transaction(transaction: ITransaction): string;

    get_mex_pairs(pagination: IPagination = DefaultPagination()): PaironNetwork;
```
