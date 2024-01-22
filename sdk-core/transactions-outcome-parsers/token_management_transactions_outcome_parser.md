## TokenManagementTransactionsOutcomeParser

A class that provides methods for extracting the output of token management transactions.
i.e. Getting the ticker of a newly issued ESDT token.

Each method should ensure that there is no `signalError` event;

```
class TokenManagementTransactionsOutcomeParser:
    // returns the token identifier
    parse_issue_fungible(transaction_events: TransactionEvent): string;

    // returns the token identifier
    parse_issue_semi_fungible(transaction_events: TransactionEvent): string;

    // returns the token identifier
    parse_issue_non_fungible(transaction_events: TransactionEvent): string;

    // returns the token identifier
    parse_register_meta_esdt(transaction_events: TransactionEvent): string;

    parse_register_and_set_all_roles(transaction_events: TransactionEvent): {
        token_identifier: string;
        roles: List[string];
    };

    // returns nothing;
    parse_set_burn_role_globally(transaction_events: TransactionEvent): None;

    // returns nothing;
    parse_unset_burn_role_globally(transaction_events: TransactionEvent): None;

    parse_set_special_role(transaction_events: TransactionEvent): {
        user_address: string;
        token_identifier: string;
        roles: List[string];
    };

    parse_nft_create(transaction_events: TransactionEvent): {
        token_identifier: string;
        nonce: string;
        initial_quantity: string;
    };

    parse_local_mint(transaction_events: TransactionEvent): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        minted_supply: string;
    };

    parse_local_burn(transaction_events: TransactionEvent): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        burnt_supply: string;
    };

    //return nothing; looks for `ESDTPause` event;
    parse_pause(transaction_events: TransactionEvent): None;

    //return nothing; looks for `ESDTUnPause` event;
    parse_unpause(transaction_events: TransactionEvent): None;

    parse_freeze(transaction_events: TransactionEvent): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        balance: string;
    };

    parse_unfreeze(transaction_events: TransactionEvent): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        balance: string;
    };

    parse_wipe(transaction_events: TransactionEvent): {
        user_address: string;
        token_identifier: string;
        nonce: string;
        balance: string;
    };

    parse_update_attributes(transaction_events: TransactionEvent): {
        token_identifier: string;
        nonce: string;
        attributes: bytes;
    };

    parse_add_quantity(transaction_events: TransactionEvent): {
        token_identifier: string;
        nonce: string;
        added_quantity: string;
    };

    parse_burn_quantity(transaction_events: TransactionEvent): {
        token_identifier: string;
        nonce: string;
        burnt_quantity: string;
    };
```
