class NetworkStake:
    totalValidators: int
    activeValidators: int
    queueSize: int
    totalStaked: int

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): NetworkStake;