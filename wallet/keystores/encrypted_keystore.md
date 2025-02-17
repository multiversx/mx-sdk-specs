## EncryptedKeystore

```
class EncryptedKeystore:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.

    // Named constructor
    static new_from_secret_key(secret_key: SecretKey, password: string, randomness: Optional[Randomnness]): EncryptedKeystore;

    // Named constructor
    // Advice: in the implementation all the parameters will be held as instance state (private fields).
    static new_from_mnemonic(mnemonic: Mnemonic, password: string, randomness: Optional[Randomnness]): EncryptedKeystore;

    // Importing "constructor"
    static import_from_object(object: KeyfileObject): EncryptedKeystore;

    // Importing "constructor"
    static import_from_file(path: Path): EncryptedKeystore;

    // When kind == 'secretKey', only index == 0 and passphrase == "" is supported.
    // When kind == 'mnemonic', secret key derivation happens under the hood.
    // Below, "passphrase" is the bip39 passphrase required to derive a secret key from a mnemonic (by default, it should be an empty string).
    decrypt_secret_key(index: int = 0, passphrase: string = ""): SecretKey;

    // decrypts the secrect keys for a range of indexes; only for kind == 'mnemonic'
    decrypt_secret_keys_in_range(start: int, end: int, passphrase: str = ""): SecretKey[];

    // Can throw:
    // - ErrMnemonicNotAvailable
    //
    // Returns the mnemonic used to create the keystore (if available, i.e. if kind == 'mnemonic').
    // This function is useful for UX flows where the application has to display the mnemonic etc.
    decrypt_mnemonic(password: string = ""): Mnemonic;

    export_to_object(address_hrp: string = "erd"): KeyfileObject;

    export_to_file(path: Path, address_hrp: string = "erd");
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
mnemonic = Mnemonic.generate_mnemonic()
keystore = EncryptedKeystore.new_from_mnemonic(mnemonic, "password")
keystore.export_to_file("file.json", "erd")
```

Iterating over the first 3 accounts:

```
keystore = EncryptedKeystore.import_from_file("file.json")

for i in [0, 1, 2]:
    secret_key = keystore.decrypt_secret_key(i)
    public_key = secret_key.get_public_key()
    address = new Address(public_key.get_bytes(), "erd")
    print("Address", i, address.to_bech32())
```

Changing the password of an existing keystore:

```
keystore = EncryptedKeystore.import_from_file("file.json")
secret_key = keystore.decrypt_secret_key() // when keystore.kind == "secretKey"
new_keystore = EncryptedKeystore.new_from_secret_key(secret_key, "newPassword")
```
