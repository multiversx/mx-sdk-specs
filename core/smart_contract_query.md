## SmartContractQuery

```
dto SmartContractQuery:
    contract: string;
    caller?: string;
    value?: Amount;
    function: string;
    arguments: List[bytes];
    block_nonce?: int;
```

## SmartContractQueryResponse

```
dto SmartContractQueryResponse:
    function: string;
    return_code: string;
    return_message: string;
    return_data_parts: List[bytes];
```
