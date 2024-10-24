## TransferTransactionsFactory

A class that provides methods for creating transactions for transfers (native or ESDT).

```
class TransferTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", "issueCost", gas limit for specific operations etc. (e.g. "gasLimitForESDTTransfer").

    // If multiple transfers are specified, or if a native amount and a token transfer are specified, then a multi-ESDT transfer transaction should be created.
    create_transaction_for_transfer({
        sender: Address;
        receiver: Address;
        native_amount: Optional[Amount];
        data: Optional[bytes];
        token_transfers: TokenTransfer[];
    }): Transaction;

    create_transaction_for_native_token_transfer({
        sender: Address;
        receiver: Address;
        native_amount: Amount;
        data: Optional[bytes];
    }): Transaction;

    // Can throw:
    // - ErrBadArguments
    //
    // If multiple transfers are specified, a multi-ESDT transfer transaction should be created.
    // Bad usage should be reported.
    create_transaction_for_esdt_token_transfer({
        sender: Address;
        receiver: Address;
        token_transfers: TokenAmount[];
    }): Transaction;
```
