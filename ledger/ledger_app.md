## LedgerApp

```
class LedgerApp:

    // sets the address to be used
    set_address(account_index: int = 0, address_index: int = 0);

    // returns the bech32 representation of the address
    get_address(account_index: int = 0, address_index: int = 0): string;

    // returns the config
    get_app_configuration(): LedgerAppConfiguration;

    // returns the app configuration
    get_version(): string;

    // signs the trasnaction and returns the signature as a hex string
    sign_transaction(tx_bytes: bytes, should_use_hash_signing: bool): string;

    // signs the message and returns the signature as a hex string
    sign_message(message_bytes: bytes): string;

    // closes the connection with the device
    close();
```
