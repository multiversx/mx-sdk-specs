## TokenManagementTransactionsOutcomeParser

A class that provides methods for extracting the output of token management transactions.
i.e. Getting the ticker of a newly issued ESDT token.

Each method should ensure that there is no `signalError` event;

For all methods that return something, arrays of plain objects can be used (JS) or simple DTOs(Py).

For DTOs, a naming convention should be used. Something like name of the operation followed by the `Outcome` keyword.

e.g.
For the `parse_register_and_set_all_roles` method, we can use:
```
dto RegisterAndSetAllRolesOutcome:
    token_identifier: str
    roles: List[str]
```

```
class TokenManagementTransactionsOutcomeParser:
    parse_issue_fungible(transaction: TransactionOnNetwork): {
        token_identifier: string;
    }[];

    parse_issue_semi_fungible(transaction: TransactionOnNetwork): {
        token_identifier: string;
    }[];

    parse_issue_non_fungible(transaction: TransactionOnNetwork): {
        token_identifier: string;
    }[];

    parse_register_meta_esdt(transaction: TransactionOnNetwork): {
        token_identifier: string;
    }[];

    parse_register_and_set_all_roles(transaction: TransactionOnNetwork): {
        token_identifier: string;
        roles: List[string];
    }[];

    // returns nothing;
    parse_set_burn_role_globally(transaction: TransactionOnNetwork): None;

    // returns nothing;
    parse_unset_burn_role_globally(transaction: TransactionOnNetwork): None;

    parse_set_special_role(transaction: TransactionOnNetwork): {
        user_address: string;
        token_identifier: string;
        roles: List[string];
    }[];

    parse_nft_create(transaction: TransactionOnNetwork): {
        token_identifier: string;
        nonce: uint64;
        initial_quantity: uint64;
    }[];

    parse_local_mint(transaction: TransactionOnNetwork): {
        user_address: string;
        token_identifier: string;
        nonce: uint64;
        minted_supply: uint64;
    }[];

    parse_local_burn(transaction: TransactionOnNetwork): {
        user_address: string;
        token_identifier: string;
        nonce: uint64;
        burnt_supply: uint64;
    }[];

    // returns the identifier of the paused token
    parse_pause(transaction: TransactionOnNetwork): {
        token_identifier: string;
    }[];

    // returns the identifier of the unpaused token
    parse_unpause(transaction: TransactionOnNetwork): {
        token_identifier: string;
    }[];

    parse_freeze(transaction: TransactionOnNetwork): {
        user_address: string;
        token_identifier: string;
        nonce: uint64;
        balance: uint64;
    }[];

    parse_unfreeze(transaction: TransactionOnNetwork): {
        user_address: string;
        token_identifier: string;
        nonce: uint64;
        balance: uint64;
    }[];

    parse_wipe(transaction: TransactionOnNetwork): {
        user_address: string;
        token_identifier: string;
        nonce: uint64;
        balance: uint64;
    }[];

    parse_update_attributes(transaction: TransactionOnNetwork): {
        token_identifier: string;
        nonce: uint64;
        attributes: bytes;
    }[];

    parse_add_quantity(transaction: TransactionOnNetwork): {
        token_identifier: string;
        nonce: uint64;
        added_quantity: uint64;
    }[];

    parse_burn_quantity(transaction: TransactionOnNetwork): {
        token_identifier: string;
        nonce: uint64;
        burnt_quantity: uint64;
    }[];
```
