class PairOnNetwork:
    address: IAddress
    id: string
    symbol: string
    name: string
    price: Amount
    baseId: string
    basePrice: Amount
    baseSymbol: string
    baseName: string
    quoteId: string
    quotePrice: Amount
    quoteSymbol: string
    quoteName: string
    totalValue: Amount
    volume24h: Amount
    state: string
    type: string

    rawResponse: any = {};

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): PairOnNetwork;