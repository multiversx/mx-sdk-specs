## AccountTransactionsFactory

```
class AccountTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "chainID", "minGasLimit", "gasLimitPerByte" etc.

    // should compute the corresponding DNS address
    create_transaction_for_setting_username({
        sender: IAddress;
        username: string;
    }): Transaction;

    create_transaction_for_saving_key_value({
        sender: IAddress;
        key_value_pairs: Dict[string, string];
    }): Transaction;

    create_transaction_for_setting_guardian({
        sender: IAddress;
        guardian_address: IAddress;
        service_id: string;
    }): Transaction;

    create_transaction_for_guarding_account({
        sender: IAddress;
    }): Transaction;

    create_transaction_for_unguarding_account({
        sender: IAddress;
    })
```
