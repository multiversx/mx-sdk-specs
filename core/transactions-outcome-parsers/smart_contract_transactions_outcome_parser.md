## SmartContractTransactionsOutcomeParser

```
class SmartContractTransactionsOutcomeParser:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor can be parametrized with an `Abi` or `Codec` instance (implementation detail), necessary to decode contract return data.

    // Parses the transaction outcome and recovers basic information about the contract(s) deployed by the transaction.
    parse_deploy({
        transaction_outcome: TransactionOutcome
    }): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            ownerAddress: string;
            codeHash: bytes;
        }];
    };

    // Parses the transaction outcome and recovers the "return data" of the contract endpoint (function) invoked by the transaction,
    // along with the return code and message.
    //
    // The `function` parameter is optional and can be used to explicitly override the ABI definition to be used for parsing.
    // If not provided, `transaction_outcome.direct_smart_contract_call_outcome.function` should be used.
    //
    // If the ABI is not available, the result should be equivalent to `transaction_outcome.direct_smart_contract_call_outcome`.
    parse_execute({
        transaction_outcome: TransactionOutcome,
        function?: string
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };
```
