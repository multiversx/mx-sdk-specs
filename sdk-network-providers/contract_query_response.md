class ContractQueryResponse:
    returnData: List[string]
    returnCode: string
    returnMessage: string
    gasUsed: Amount

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): ContractQueryResponse;

    get_return_data_parts(): List[bytes];

    to_dictionary(): Dict[string, Any]