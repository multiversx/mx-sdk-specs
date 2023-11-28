class TransactionEvent:
    address: IAddress
    identifier: string
    topics: List[TransactionEventTopic]
    dataPayload: TransactionEventData
    additionalData: List[TransactionEventData]

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): TransactionEvent;

    find_first_or_none_topic(Callable[[topic: TransactionEventTopic], boolean]): TransactionEventTopic | None;

    get_last_topic(): TransactionEventTopic;

    // optionally, a method to convert the object to a dictionary/plain object can be implemented
    to_dictionary(): Dict[str, Any];


class TransactionEventData:
    raw: bytes

    constructor(data: bytes);

    static from_base64(s: string): TransactionEventData;

    to_string(): string;

    to_hex(): string;

    // optionally, implement a method to return the raw value


class TransactionEventTopic:
    raw: bytes

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be something like:
    constructor(topic: string); // topic is a base64 encoded string

    to_string(): string;

    to_hex(): string;

    // optionally, implement a method to return the raw value
