class TransactionLogs:
    address: IAddress
    events: List[TransactionEvent]

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): TransactionLogs;

    // for reference, check out: https://github.com/multiversx/mx-sdk-js-network-providers/blob/main/src/transactionLogs.ts#L22
    find_single_or_none_event(identifier: string, Callable[[event: TransactionEvent], boolean]): TransactionEvent | None;

    find_first_or_none_event(identifier: string, Callable[[event: TransactionEvent], boolean]): TransactionEvent | None;