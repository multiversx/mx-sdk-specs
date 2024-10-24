## SmartContractQuery

```
dto SmartContractQuery:
    contract: Address;
    caller?: string;
    value?: Amount;
    function: string;
    arguments: List[bytes];
```

## SmartContractQueryResponse

```
dto SmartContractQueryResponse:
    raw: any;

    function: string;
    return_code: string;
    return_message: string;
    return_data_parts: List[bytes];
```
