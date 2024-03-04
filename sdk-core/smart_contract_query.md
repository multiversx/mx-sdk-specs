## SmartContractQuery

```
dto SmartContractQuery:
    contract: string;
    caller?: string;
    value?: Amount;
    function: string;
    arguments: List[bytes];
```

## SmartContractQueryResponse

```
dto SmartContractQueryResponse:
    return_code: string;
    return_message: string;
    return_data_parts: List[bytes];
```
