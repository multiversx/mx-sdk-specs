class TransactionReceipt:
    value: Amount
    sender: IAddress
    data: string
    hash: str

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): TransactionReceipt;
