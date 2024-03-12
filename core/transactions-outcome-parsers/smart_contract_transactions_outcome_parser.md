## SmartContractTransactionsOutcomeParser

```
class SmartContractTransactionsOutcomeParser:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor can be parametrized with an `Abi` or `Codec` instance (implementation detail), necessary to decode contract return data.

    parse_execute(transaction_outcome: TransactionOutcome): {
        values: List[any];
        return_code: string;
        return_message: string;
    };
```
