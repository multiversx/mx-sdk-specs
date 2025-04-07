## DelegationTransactionsOutcomeParser

A class that provides methods for extracting the output of delegation transactions.
i.e. Getting the address of a newly created delgation contract.

Each method should ensure that there is no `signalError` event.

For all methods that return something, arrays of plain objects(JS) or simple DTOs(Py) can be used.
For DTOs, a naming convention should be used. Something like the name of the operation followed by the `Outcome` keyword.

e.g.
For the `parse_create_new_delegation_contract` method, we can use:

```
dto CreateNewDelegationContractOutcome:
    contract_address: Address;
```

```
class DelegationTransactionsOutcomeParser:
    parse_create_new_delegation_contract(transaction: TransactionOnNetwork): {
        contract_address: Address;
    }[];

    parse_delegate(transaction: TransactionOnNetwork): {
        amount: Amount;
    }[];

    parse_undelegate(transaction: TransactionOnNetwork): {
        amount: Amount;
    }[];

    parse_claim_rewards(transaction: TransactionOnNetwork): {
        amount: Amount;
    }[];

    parse_redelegate_rewards(transaction: TransactionOnNetwork): {
        amount: Amount;
    }[];
```
