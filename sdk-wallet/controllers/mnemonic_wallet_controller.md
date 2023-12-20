## MnemonicWalletController

```
class MnemonicWalletController implements IWalletController:
    // The constructor is not strictly captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor would take as input: a "Mnemonic", an "IMnemonicComputer", a "IKeysComputer", an "IAddressComputer".
    // Generally speaking, upon construction, the selected address would be the one at index 0.

    // Generally speaking, this would use the underlying mnemonic computer to derive the secret key, 
    // then compute the public key and, ultimately, the address. 
    get_address(address_index: number): Address;

    get_current_address(): Address;

    // Generally speaking, this would use the underlying mnemonic computer to derive the secret key.
    select_current_address(address_index: number);

    // Part of the Native Authentication flow.
    sign_auth_token(auth_token: bytes): bytes;
    
    // Kept separate from "sign_message", for clarity, and also because some wallet controllers require separate signing flows (e.g. Ledger).
    sign_transaction(serialized_transaction: bytes): bytes;
    // Kept separate from "sign_transaction", for clarity, and also because some wallet controllers require separate signing flows (e.g. Ledger).
    sign_message(serialized_message: bytes): bytes;
```

**Note:** mnemonics cannot be retrieved from the wallet controller - at least, not directly; this use case is not supported by the specs. Wallet-like applications are responsible to enable support for retrieving the mnemonic, if they wish to do so (e.g. for display).
