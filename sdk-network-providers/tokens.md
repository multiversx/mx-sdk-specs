class FungibleTokenOfAccountOnNetwork:
    identifier: str
    balance: Amount
    rawResponse: Any

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): FungibleTokenOfAccountOnNetwork;


class NonFungibleTokenOfAccountOnNetwork:
    identifier: str
    collection: str
    timestamp: int
    attributes: str
    nonce: int
    type: str
    name: str
    creator: IAddress
    supply: Amount
    decimals: int
    royalties: float
    assets: List[str]
    balance: Amount