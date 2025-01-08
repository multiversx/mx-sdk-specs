## Account

```
class Account:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Suggestions:
    constructor(secret_key: bytes, hrp: Optional[str]);

    address: Address;

    // Local copy of the account nonce.
    // Must be explicitly managed by client code.
    nonce: uint64;

    // signs using the secret key of the account
    sign(data: bytes): bytes;

    // verifies the signature using the public key of the account
    verify(data: bytes, signature: bytes) : boolean;

    // serializes the transaction, computes the signature and returns it;
    sign_transaction(transaction: Transaction): bytes;

    // serializes the message, computes the signature and returns it;
    sign_message(message: Message): bytes;

    // Gets the nonce (the one from the object's state) and increments it.
    get_nonce_then_increment(): uint64;

    // Named constructor
    // Loads a secret key from a PEM file. PEM files may contain multiple accounts - thus, an (optional) "index" is used to select the desired secret key.
    // Returns an Account object, initialized with the secret key.
    static new_from_pem(path: Path, index: Optional[int], hrp: Optional[str]): Account;

    // Named constructor
    // Loads a secret key from an encrypted keystore file. Handles both keystores that hold a mnemonic and ones that hold a secret key (legacy).
    // For keystores that hold an encrypted mnemonic, the optional "address_index" parameter is used to derive the desired secret key.
    // Returns an Account object, initialized with the secret key.
    static new_from_keystore(path: Path, password: str, address_index: Optional[int], hrp: Optional[str]): Account;

    // Named constructor
    // Loads (derives) a secret key from a mnemonic. The optional "address_index" parameter is used to guide the derivation.
    // Returns an Account object, initialized with the secret key.
    static new_from_mnemonic(mnemonic: str, address_index: Optional[int], hrp: Optional[str]): Account;

    // Named constructor
    // Returns an Account object, initialized with the secret key.
    static new_from_keypair(keypair: KeyPair): Account;

    // saves the wallet to a pem file
    save_to_pem(path: Path);

    // saves the wallet to a keystore file
    save_to_keystore(path: Path, password: string);
```
