## ApiNetworkProvider

Sometimes the `ApiNetworkProvider` class uses an underlying `ProxyNetworkProvider`as some of the API routes are forwarded to the proxy.

```
class ApiNetworkProvider:
    constructor(url: str, config: Optional):
        self.backing_proxy = ProxyNetworkProvider(url)
    
    // same methods as the proxy provider
    // generic methods
    do_get_generic(resource_url: str): Dict[str, Any];
        calls do_get(self.url + resource_url)
    
    do get_generic_collection(resource_url: str): List[Dict[str, Any]];
    
    do_post_generic(resource_url: str, payload: any);

    // these methods use the `backing_proxy`
    get_network_config(): NetworkConfig;

    get_network_status(shard: int = METACHAIN_ID): NetworkStatus;

    get_guardian_data(address: IAddress): GuardianData;

    // the following methods use the API
    get_network_gas_configs(): GasConfig;

    get_network_stake_statistics(): NetworkStake;

    get_network_general_statistics(): NetworkGeneralStatistics;

    get_account(address: IAddress): AccountOnNetwork;

    get_fungible_tokens_of_account(address: IAddress, pagination: IPagination = DefaultPagination()): List[FungibleTokenOfAccountOnNetwork];

    get_nonfungible_tokens_of_account(address: IAddress, pagination: IPagination = DefaultPagination()): List[NonFungibleTokenOfAccountOnNetwork];

    get_fungible_token_of_account(address: IAddress, token_identifier: str): FungibleTokenOfAccountOnNetwork;

    get_nonfungible_token_of_account(address: IAddress, collection: str, nonce: int): NonFungibleTokenOfAccountOnNetwork;

    get_definition_of_fungible_token(token_identifier: str): DefinitionOfFungibleTokenOnNetwork;

    get_definition_of_token_collection(collection: str): DefinitionOfTokenCollectionOnNetwork;

    get_non_fungible_token(collection: str, nonce: int): NonFungibleTokenOfAccountOnNetwork;

    query_contract(query: IContractQuery): ContractQueryResponse;

    get_transaction(tx_hash: str): TransactionOnNetwork;

    get_account_transactions(address: IAddress, pagination: IPagination = DefaultPagination()): List[TransactionOnNetwork];

    get_bunch_of_transactions(tx_hashes: List[str], with_block_info: bool = False, with_results: bool = False): List[TransactionOnNetwork];

    get_transactions_in_mempool_for_account(address: IAddress): List[TransactionInMempool];

    get_transaction_status(tx_hash: str): TransactionStatus;

    send_transaction(transaction: ITransaction): str;

    send_transactions(transactions: List[ITransaction]): Tuple[int, str];
```
