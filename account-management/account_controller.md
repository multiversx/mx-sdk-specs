## AccountController

Promotes all the functionality of `AccountTransactionsFactory`.

```
class AccountController: 
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "chainID", "minGasLimit", "gasLimitPerByte" etc.
    // Suggestions:
    constructor(options: { chainID: string });

    create_transaction_for_saving_key_value({
        sender: IAccount;
        nonce: uint64;
        key_value_pairs: Dict[bytes, bytes];
        guardian: Optional[Address];
        relayer: Optional[Address];
        gas_limit: Optional[uint32];
        gas_price: Optional[uint32];
    }): Transaction;

    create_transaction_for_setting_guardian({
        sender: IAccount;
        nonce: uint64;
        guardian_address: Address;
        service_id: string;
        relayer: Optional[Address];
        gas_limit: Optional[uint32];
        gas_price: Optional[uint32];
    }): Transaction;

    create_transaction_for_guarding_account({
        sender: IAccount;
        nonce: uint64;
        relayer: Optional[Address];
        gas_limit: Optional[uint32];
        gas_price: Optional[uint32];
    }): Transaction;

    create_transaction_for_unguarding_account({
        sender: IAccount;
        nonce: uint64;
        guardian: Optional[Address];
        relayer: Optional[Address];
        gas_limit: Optional[uint32];
        gas_price: Optional[uint32];
    }): Transaction;
```
