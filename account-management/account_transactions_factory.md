## AccountTransactionsFactory

```
class AccountTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "chainID", "minGasLimit", "gasLimitPerByte" etc.

    create_transaction_for_saving_key_value({
        sender: Address;
        key_value_pairs: Dict[bytes, bytes];
    }): Transaction;

    create_transaction_for_setting_guardian({
        sender: Address;
        guardian_address: Address;
        service_id: string;
    }): Transaction;

    create_transaction_for_guarding_account({
        sender: Address;
    }): Transaction;

    create_transaction_for_unguarding_account({
        sender: Address;
    }): Transaction;
```
