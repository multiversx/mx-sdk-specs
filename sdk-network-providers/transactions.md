class TransactionOnNetwork:
    is_completed: Optional[boolean]
    hash: str
    type: str
    nonce: int
    round: int
    epoch: int
    value: int
    receiver: IAddress
    sender: IAddress
    gas_limit: int
    gas_price: int
    data: str
    signature: str
    status: TransactionStatus
    timestamp: int

    block_nonce: int
    hyperblock_nonce: int
    hyperblock_hash: str

    receipt: TransactionReceipt
    contract_results: ContractResults
    logs: TransactionLogs
    raw_response: Dict[str, Any]

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; static methods can be used for initialization
    // here we need two methods, one for proxy and one for api since the response is a bit different

    static from_api_http_response(payload: Dict[str, Any]) -> TransactionOnNetwork;

    static from_proxy_http_response(payload: Dict[str, Any]) -> TransactionOnNetwork;

    // optionally, a method to convert the object to a dictionary/plain object can be implemented
    to_dictionary(): Dict[str, Any];
