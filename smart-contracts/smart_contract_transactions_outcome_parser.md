## SmartContractTransactionsOutcomeParser

```
class SmartContractTransactionsOutcomeParser:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor can be parametrized with an `Abi` or `Codec` instance (implementation detail), necessary to decode contract return data.

    // Parses the transaction and recovers basic information about the contract(s) deployed by the transaction.
    parse_deploy({
        transaction: TransactionOnNetwork
    }): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: Address;
            ownerAddress: Address;
            codeHash: bytes;
        }];
    };

    // Parses the transaction and recovers the "return data" of the contract endpoint (function) invoked by the transaction,
    // along with the return code and message.
    //
    // The `function` parameter is optional and can be used to explicitly override the ABI definition to be used for parsing.
    // If not provided, `transaction.function` should be used.
    //
    // If the ABI is not available, the result should still return the contract return data (without decoding it).
    parse_execute({
        transaction: TransactionOnNetwork,
        function?: string
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };

      // Decodes the raw (encoded) response and returns the parsed result.
    parse_query_response(response: SmartContractQueryResponse): List[any];
```
