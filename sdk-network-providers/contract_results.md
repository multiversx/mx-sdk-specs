class ContractResultItem:
    hash: str
    nonce: int
    value: Amount
    receiver: IAddress
    sender: IAddress
    data: str
    previous_hash: str
    original_hash: str
    gas_limit: Amount
    gas_price: Amount
    call_type: int
    return_message: str
    is_refund: boolean
    logs: TransactionLogs

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; static methods can be used for initialization
    // here we need two methods, one for proxy and one for api since the response is a bit different

    static from_api_http_response(payload: Dict[str, Any]) -> ContractResultItem;

    static from_proxy_http_response(payload: Dict[str, Any]) -> ContractResultItem;

    // optionally, a method to convert the object to a dictionary/plain object can be implemented
    to_dictionary() -> Dict[str, Any];




class ContractResults:
    // should be sorted based on nonce
    items: List[ContractResultItem]

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // We can use two static methods for initialization 

    static from_api_http_response(results: List[Any]): ContractResults;
    static from_proxy_http_response(results: List[Any]): ContractResults;
