class NetworkConfig:
    chainID: str
    gasPerDataByte: int
    topUpFactor: float
    startTime: int
    roundDuration: int
    roundsPerEpoch: int
    topUpRewardsGradientPoint: Amount
    minGasLimit: Amount
    mingGasPrice: Amount
    gasPriceModifier: float
    minTransactionVersion: int
    numShardsWithoutMeta: int

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): NetworkConfig;

    // optionally, a method to convert the object to a dictionary/plain object can be implemented
    to_dictionary(): Dict[str, Any];