class NetworkGeneralStatistics:
    shards: int
    blocks: int
    accounts: int
    transactions: int
    refresh_rate: int
    epoch: int
    rounds_passed: int
    rounds_per_epoch: int

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): NetworkGeneralStatistics;