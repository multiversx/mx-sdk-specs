## ProxyNetworkProvider

```
class ProxyNetworkProvider:
    // the config is implementation specific (Session, AxiosConfig, etc.)
    constructor(url: str, config: Optional);

    // generic methods
    do_get_generic(resource_url: str): Dict[str, Any];
    
    do_post_generic(resource_url: str, payload: any);
    
    get_network_config(): NetworkConfig;

    get_network_gas_configs(): GasConfig;

    get_network_status(shard: int = METACHAIN_ID): NetworkStatus;

    get_account(address: IAddress): AccountOnNetwork;

    get_guardian_data(address: IAddress): GuardianData;

    get_fungible_tokens_of_account(address: IAddress): List[FungibleTokenOfAccountOnNetwork];

    get_nonfungible_tokens_of_account(address: IAddress): List[NonFungibleTokenOfAccountOnNetwork];

    get_fungible_token_of_account(address: IAddress, identifier: str): FungibleTokenOfAccountOnNetwork;

    // needs both collection and nonce; can get only identifier and apply logic to get both collection name and nonce
    get_nonfungible_token_of_account(address: IAddress, collection: str, nonce: int): NonFungibleTokenOfAccountOnNetwork;

    get_transaction(tx_hash: str, with_results: bool = false): TransactionOnNetwork;

    get_account_transactions(address: IAddress): List[TransactionOnNetwork];

    get_transaction_status(tx_hash: str): TransactionStatus;

    send_transaction(transaction: ITransaction): str;

    send_transactions(transactions: Sequence[ITransaction]): Tuple[int, str];

    query_contract(query: IContractQuery): ContractQueryResponse;

    get_definition_of_fungible_token(token_identifier: str): DefinitionOfFungibleTokenOnNetwork;

    get_definition_of_token_collection(collection: str): DefinitionOfTokenCollectionOnNetwork;

    // can get hyperblock by-hash or by-nonce
    get_hyperblock(key: int | str) -> Dict[str, Any]:
```
