## DelegationTransactionsFactory

```
class DelegationTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", gas limit for specific operations etc. (e.g. "gasLimitForStaking").

    create_transaction_for_new_delegation_contract({
        sender: Address;
        totalDelegationCap: Amount;
        service_fee: number;
        amount: Amount;
    }): Transaction;

    create_transaction_for_adding_nodes({
        sender: Address;
        delegationContract: Address;
        publicKeys: List[IPublicKey];
        signedMessages: List[bytes];
    }): Transaction;

    create_transaction_for_removing_nodes({
        sender: Address,
        delegationContract: Address;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_staking_nodes({
        sender: Address,
        delegationContract: Address;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_unbonding_nodes({
        sender: Address,
        delegationContract: Address;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_unstaking_nodes({
        sender: Address,
        delegationContract: Address;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_unjailing_nodes({
        sender: Address,
        delegationContract: Address;
        publicKeys: List[IPublicKey];
    }): Transaction;

    create_transaction_for_changing_service_fee({
        sender: Address;
        delegationContract: Address;
        serviceFee: int;
    }): Transaction;

    create_transaction_for_modifying_delegation_cap({
        sender: Address;
        delegationContract: Address;
        delegationCap: int;
    }): Transaction;

    create_transaction_for_setting_automatic_activation({
        sender: Address;
        delegationContract: Address;
    }): Transaction;

    create_transaction_for_unsetting_automatic_activation({
        sender: Address;
        delegationContract: Address;
    }): Transaction;

    create_transaction_for_setting_cap_check_on_redelegate_rewards({
        sender: Address;
        delegationContract: Address;
    }): Transaction;

    create_transaction_for_unsetting_cap_check_on_redelegate_rewards({
        sender: Address;
        delegationContract: Address;
    }): Transaction;

    create_transaction_for_setting_metadata({
        sender: Address;
        delegationContract: Address;
        name: str;
        website: str;
        identifier: str;
    }): Transaction;

    ...
```
