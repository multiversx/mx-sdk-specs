## DelegationTransactionsOutcomeParser

A class that provides methods for extracting the output of delegation transactions.
i.e. Getting the address of a newly created delgation contract.

Each method should ensure that there is no `signalError` event.

For all methods that return something, plain objects(JS) or simple DTOs(Py) can be used.
For DTOs, a naming convention should be used. Something like the name of the operation followed by the `Outcome` keyword.

e.g.
For the `parse_create_new_delegation_contract` method, we can use:
```
dto CreateNewDelegationContractOutcome:
    contract_address: string;
```

```
class DelegationTransactionsOutcomeParser:
    parse_create_new_delegation_contract(transaction_outcome: TransactionOutcome): {
        contract_address: string;
    };
```
