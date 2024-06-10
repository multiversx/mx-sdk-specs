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
    }): Transaction;

    parse_create_new_delegation_contract(transaction_on_network: TransactionOnNetwork): {
        contract_address: string;
    }[];

    await_completed_create_new_delegation_contract(transaction_on_network: TransactionOnNetwork): {
        contract_address: string;
    }[];

    // ...
```
