## EncryptedKeystore

`EncryptedKeystore` handles encrypted JSON files.

Note: current design is suboptimal. `EncryptedKeystore` handles both legacy keystore files (that hold encrypted secret keys) and new keystore files (that hold encrypted mnemonics). `get_kind()` can be used as a differentiator. A future design might involve two separate classes.

```
class EncryptedKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Instance fields to be held (suggestion): 
    //  - keys_computer: IKeysComputer
    //  - kind: "secretKey|mnemonic"
    //  - secretKey: ISecretKey (will be nil if kind == 'mnemonic')
    //  - mnemonic: Mnemonic (will be nil if kind == 'secretKey')

    // Named constructor
    // This should have a trivial implementation.
    static new_from_secret_key(keys_computer: IKeysComputer, secret_key: ISecretKey): EncryptedKeystore

    // Named constructor
    // This should have a trivial implementation.
    static new_from_mnemonic(keys_computer: IKeysComputer, mnemonic: Mnemonic): EncryptedKeystore

    // Importing "constructor"
    // This should decrypt the encrypted data, then call the (private) constructor.
    // "password" should not be retained.
    static import_from_object(keys_computer: IKeysComputer, object: KeyfileObject, password: string): EncryptedKeystore

    // Importing "constructor"
    // This should load the file content, decrypt the encrypted data, then call the (private) constructor.
    // "password" should not be retained.
    static import_from_file(keys_computer: IKeysComputer, path: Path, password: string): EncryptedKeystore

    get_kind(): "secretKey|mnemonic"

    // Can throw:
    // - ErrSecretKeyNotAvailable
    //
    // Returns the secretKey (if available, i.e. if kind == 'secretKey').
    get_secret_key(): ISecretKey

    // Can throw:
    // - ErrMnemonicNotAvailable
    // 
    // Returns the mnemonic used to create the keystore (if available, i.e. if kind == 'mnemonic').
    // The caller of this can then derive secret keys, as needed.
    get_mnemonic(): Mnemonic

    export_to_object(password: string, address_hrp: string): KeyfileObject

    export_to_file(path: Path, password: string, address_hrp: string)
```

```
dto KeyfileObject:
    version: number;

    // "secretKey|mnemonic"
    kind: string;
    
    // a GUID
    id: string

    // hex representation of the address
    address: string 

    // bech32 representation of the address
    bech32: string

    crypto: {
        // PasswordEncryptedData.ciphertext
        ciphertext: string;

        cipherparams: {
            // PasswordEncryptedData.iv
            iv: string;
        };

        // PasswordEncryptedData.cipher
        cipher: string;

        // PasswordEncryptedData.kdf
        kdf: string;
        kdfparams: {
            // PasswordEncryptedData.kdfparams.n
            n: number;

            // PasswordEncryptedData.kdfparams.r
            r: number;

            // PasswordEncryptedData.kdfparams.p
            p: number;

            // PasswordEncryptedData.kdfparams.dklen
            dklen: number;
            
            // PasswordEncryptedData.salt
            salt: string; 
        };

        // PasswordEncryptedData.mac
        mac: string;
    };
```
