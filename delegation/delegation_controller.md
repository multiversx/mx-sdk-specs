## DelegationController

Promotes all the functionality of `DelegationTransactionsFactory` and `DelegationTransactionsOutcomeParser`.

```
class DelegationController:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Suggestions:
    constructor(network_provider: INetworkProvider);

    create_transaction_for_new_delegation_contract({
        sender: IAccount;
        nonce: int;
        totalDelegationCap: Amount;
        service_fee: number;
        amount: Amount;
        guardian: Optional[Address];
        relayer: Optional[Address];
        gas_limit: Optional[uint32];
        gas_price: Optional[uint32];
    }): Transaction;

    parse_create_new_delegation_contract(transaction_on_network: TransactionOnNetwork): {
        contract_address: Address;
    }[];

    await_completed_create_new_delegation_contract(transaction_on_network: TransactionOnNetwork): {
        contract_address: Address;
    }[];

    parse_delegate(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    await_completed_delegate(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    parse_undelegate(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    await_completed_undelegate(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    parse_claim_rewards(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    await_completed_claim_rewards(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    parse_redelegate_rewards(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    await_completed_redelegate_rewards(transaction_on_network: TransactionOnNetwork): {
        amount: Amount;
    }[];

    // ...
```
