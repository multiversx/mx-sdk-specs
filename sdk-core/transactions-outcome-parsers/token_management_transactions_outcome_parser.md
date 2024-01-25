## TokenManagementTransactionsOutcomeParser

A class that provides methods for extracting the output of token management transactions.
i.e. Getting the ticker of a newly issued ESDT token.

Each method should ensure that there is no `signalError` event;

For methods that return multiple things, plain object can be used (JS) or simple DTOs(Py).

For DTOs, a naming convention should be used. Something like name of the operation followed by the `Outcome` keywork.

e.g.
For the `parse_register_and_set_all_roles` method, we can use:
```
dto RegisterAndSetAllRolesOutcome:
    token_identifier: str
    roles: List[str]
```

```
class TokenManagementTransactionsOutcomeParser:
    // returns the token identifier
    parse_issue_fungible(transaction_results_and_logs: ITransactionResultsAndLogsHolder): string;

    // returns the token identifier
    parse_issue_semi_fungible(transaction_results_and_logs: ITransactionResultsAndLogsHolder): string;

    // returns the token identifier
    parse_issue_non_fungible(transaction_results_and_logs: ITransactionResultsAndLogsHolder): string;

    // returns the token identifier
    parse_register_meta_esdt(transaction_results_and_logs: ITransactionResultsAndLogsHolder): string;

    parse_register_and_set_all_roles(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        token_identifier: string;
        roles: List[string];
    };

    // returns nothing;
    parse_set_burn_role_globally(transaction_results_and_logs: ITransactionResultsAndLogsHolder): None;

    // returns nothing;
    parse_unset_burn_role_globally(transaction_results_and_logs: ITransactionResultsAndLogsHolder): None;

    parse_set_special_role(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        user_address: string;
        token_identifier: string;
        roles: List[string];
    };

    parse_nft_create(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        token_identifier: string;
        nonce: string;
        initial_quantity: string;
    };

    parse_local_mint(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        minted_supply: string;
    };

    parse_local_burn(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        burnt_supply: string;
    };

    // returns the identifier of the paused token
    parse_pause(transaction_results_and_logs: ITransactionResultsAndLogsHolder): string;

    // returns the identifier of the unpaused token
    parse_unpause(transaction_results_and_logs: ITransactionResultsAndLogsHolder): string;

    parse_freeze(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        balance: string;
    };

    parse_unfreeze(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        balance: string;
    };

    parse_wipe(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        balance: string;
    };

    parse_update_attributes(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        token_identifier: string;
        nonce: string;
        attributes: bytes;
    };

    parse_add_quantity(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        token_identifier: string;
        nonce: string;
        added_quantity: string;
    };

    parse_burn_quantity(transaction_results_and_logs: ITransactionResultsAndLogsHolder): {
        token_identifier: string;
        nonce: string;
        burnt_quantity: string;
    };
```
