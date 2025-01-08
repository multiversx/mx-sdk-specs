## LedgerApp

```
class LedgerApp:

    // sets the address to be used
    set_address(address_index: int = 0);

    // returns the bech32 representation of the address
    get_address(address_index: int = 0): string;

    // returns the config
    get_app_configuration(): LedgerAppConfiguration;

    // returns the app configuration
    get_version(): string;

    // computes transaction signature and returns it as a hex string
    sign_transaction(tx_bytes: bytes): string;

    // computes message signature and returns it as a hex string
    sign_message(message_bytes: bytes): string;

    // closes the connection with the device
    close();
```
