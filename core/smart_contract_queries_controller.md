## SmartContractQueriesController

```
class SmartContractQueriesController:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor can be parametrized with an `Abi` or `Codec` instance (implementation detail), necessary to decode contract return data.
    // Additionally, it can either be parametrized with a network provider, or some other facility to run contract queries against the network.

    // It does "create_query", "run_query" and "parse_query_response" in one go.
    // Can throw:
    // - ErrSmartContractQuery (e.g. when return code is not "ok")
    query({
        contract: IAddress;
        caller?: IAddress;
        value?: Amount;
        function: string;
        arguments: List[object];
    }): List[any];

    // If `Abi` or `Codec` is available, this function encodes the input arguments accordingly.
    create_query({
        contract: string;
        caller?: string;
        value?: Amount;
        function: string;
        arguments: List[any];
    }): SmartContractQuery;

    // Runs the query against the network and returns the raw (encoded) response.
    run_query(query: SmartContractQuery): SmartContractQueryResponse;

    // Decodes the raw (encoded) response and returns the parsed result.
    parse_query_response(response: SmartContractQueryResponse): List[any];
```
