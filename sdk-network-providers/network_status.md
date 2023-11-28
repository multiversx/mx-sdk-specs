class NetworkStatus:
    currentRound: int
    epochNumber: int
    highestFinalNonce: int
    nonce: int
    nonceAtEpochStart: int
    noncesPassedInCurrentEpoch: int
    roundAtEpochStart: int
    roundsPassedInCurrentEpoch: int
    roundsPerEpoch: int

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): NetworkStatus;

    // optionally, a method to convert the object to a dictionary/plain object can be implemented
    to_dictionary(): Dict[str, Any];
