## EncryptedKeystore

`EncryptedKeystore`

```
class EncryptedKeystore:
    // The constructor is not strictly captured by the specs; it's up to the implementing library to define it. Suggestion below.
    // If kind == 'secretKey', `secret_key` must be provided, while `mnemonic` must be nil.
    // If kind == 'mnemonic', `secret_key` must be nil, while `mnemonic` must be provided.
    // In the implementation, all the parameters would be held as instance state (private fields).
    private constructor(keys_computer: IKeysComputer, kind: string, secret_key?: ISecretKey, mnemonic?: Mnemonic)

    // Named constructor
    // This should have a trivial implementation (e.g. a wrapper around the private constructor).
    static new_from_secret_key(keys_computer: IKeysComputer, secret_key: ISecretKey): EncryptedKeystore

    // Named constructor
    // This should have a trivial implementation (e.g. a wrapper around the private constructor).
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

## Examples of usage

Create a new JSON keystore using a new mnemonic:

```
provider = new UserWalletProvider()
mnemonic = provider.generate_mnemonic()
keystore = EncryptedKeystore.new_from_mnemonic(provider, mnemonic)
keystore.export_to_file("file.json", "password", "erd")
```

Iterating over the first 3 accounts:

```
provider = new UserWalletProvider()
keystore = EncryptedKeystore.import_from_file(provider, "file.json", "password")

for i in [0, 1, 2]:
    secret_key = keystore.get_secret_key(i, "")
    public_key = provider.compute_public_key_from_secret_key(secret_key)
    address = new Address(public_key, "erd")
    print("Address", i, address.bech32())
```

Changing the password of an existing keystore:

```
provider = new UserWalletProvider()
keystore = EncryptedKeystore.import_from_file(provider, "file.json", "password")
keystore.export_to_file("file.json", "new_password", "erd")
```
