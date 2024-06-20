class ContractQueryRequest:
    query: IContractQuery

    constructor(query: IContractQuery);

    to_http_request(): Dict[str, Any];