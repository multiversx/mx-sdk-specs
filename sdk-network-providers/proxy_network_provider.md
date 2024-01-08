## ProxyNetworkProvider

```
class ProxyNetworkProvider:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Should at least specify the url to query

    // generic methods used for actually querying the gateway
    do_get_generic(resource_url: str): Dict[str, Any];
    
    do_post_generic(resource_url: str, payload: any): Dict[str, Any];
    
    get_network_config(): NetworkConfig;

    get_network_gas_configs(): Dict[str, Dict[str,int]];

    get_network_status(shard: int = METACHAIN_ID): NetworkStatus;

    get_account(address: IAddress): AccountOnNetwork;

    get_guardian_data(address: IAddress): GuardianData;

    get_fungible_tokens_of_account(address: IAddress): List[FungibleTokenOfAccountOnNetwork];

    get_nonfungible_tokens_of_account(address: IAddress): List[NonFungibleTokenOfAccountOnNetwork];

    get_fungible_token_of_account(address: IAddress, identifier: str): FungibleTokenOfAccountOnNetwork;

    get_nonfungible_token_of_account(address: IAddress, collection: str, nonce: int): NonFungibleTokenOfAccountOnNetwork;

    get_transaction(tx_hash: str, with_process_status: boolean = false): TransactionOnNetwork;

    get_transaction_status(tx_hash: str): TransactionStatus;

    send_transaction(transaction: ITransaction): str;

    // returns None if transaction was not sent
    send_transactions(transactions: List[ITransaction]): List[str | None];

    query_contract(query: IContractQuery): ContractQueryResponse;

    get_definition_of_fungible_token(token_identifier: str): DefinitionOfFungibleTokenOnNetwork;

    get_definition_of_token_collection(collection: str): DefinitionOfTokenCollectionOnNetwork;

    // can get hyperblock by-hash or by-nonce
    get_hyperblock(key: int | str) -> Dict[str, Any]:
```
