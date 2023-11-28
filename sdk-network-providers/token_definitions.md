class DefintionOfFungibleTokenOnNetwork:
    identifier: str
    name: str
    ticker: str
    owner: IAddress
    decimals: int
    supply: int
    burnt_value: int
    is_paused: boolean
    can_upgrade: boolean
    can_mint: boolean
    can_burn: boolean
    can_change_owner: boolean
    can_pause: boolean
    can_freeze: boolean
    can_wipe: boolean
    can_add_special_roles: boolean
    assets: Dict[string, Any]

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; static methods can be used for initialization
    // here we need two methods, one for proxy and one for api since the response is a bit different

    static from_api_http_response(payload: Dict[str, Any]): DefintionOfFungibleTokenOnNetwork;

    static from_proxy_http_response(payload: Dict[str, Any]): DefintionOfFungibleTokenOnNetwork;


class DefinitionOfTokenCollectionOnNetwork:
    collection: str
    type: str
    name: str
    ticker: str
    owner: IAddress = EmptyAddress()
    decimals: int
    can_pause: boolean
    can_freeze: boolean
    can_wipe: boolean
    can_upgrade: boolean
    can_change_owner: boolean
    can_add_special_roles: boolean
    can_transfer_nft_create_role: boolean
    can_create_multi_shard: boolean

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; static methods can be used for initialization
    // here we need two methods, one for proxy and one for api since the response is a bit different

    static from_api_http_response(payload: Dict[str, Any]): DefintionOfFungibleTokenOnNetwork

    // used after querying the ESDT SC for the token; hrp is used for correctly computing the owner's address
    static from_response_of_get_token_properties(collection: string, data: List[bytes], address_hrp: string): DefinitionOfTokenCollectionOnNetwork;
