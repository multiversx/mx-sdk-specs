## SmartContractQueriesController

```
class SmartContractQueriesController:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor can be parametrized with an `Abi` or `Codec` instance (implementation detail), necessary to decode contract return data.
    // Additionally, it can either be parametrized with a network provider, or some other facility to run contract queries against the network.

    create_query(): SmartContractQuery;

    run_query(query: SmartContractQuery): SmartContractQueryResponse;

    parse_query_response(response: SmartContractQueryResponse): List[any];
```
