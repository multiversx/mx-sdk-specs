## LedgerWalletController

Note: though the Ledger device supports `account_index`, as well, this is not used in the context of MultiversX. For all purposes, `account_index` is `0`, while `address_index` is allowed to vary.

References (not implementation suggestions):
- https://github.com/multiversx/mx-sdk-js-hw-provider/blob/v6.4.0/src/ledgerApp.ts
- https://github.com/multiversx/mx-sdk-py-cli/blob/main/multiversx_sdk_cli/ledger/ledger_app_handler.py

```
class LedgerWalletController:
    // The constructor is not strictly captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor would take as input: a "IKeysComputer", an "IAddressComputer".

    // Returns the configuration of the MultiversX App.
    get_configuration(): LedgerAppConfiguration;

    get_address(address_index: number): Address;

    get_current_address(): Address;

    select_current_address(address_index: number);

    // Part of the Native Authentication flow.
    sign_auth_token(auth_token: bytes): bytes;

    sign_transaction(serialized_transaction: bytes): bytes;
    sign_message(serialized_message: bytes): bytes;
```

```
dto LedgerAppConfiguration:
    is_transaction_data_allowed: boolean;
    address_index: number;
    version: string;
```
