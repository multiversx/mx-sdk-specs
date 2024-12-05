## PEMStore

```
class PEMKeystore:
    constructor(keypair: KeyPair);

    // Named constructor
    static new_from_secret_key(secret_key: SecretKey): PEMKeystore;

    // Named constructor
    static new_from_secret_keys(secret_keys: SecretKey[]): PEMKeystore[];

    // Importing "constructor"
    static import_from_text(text: string): PEMKeystore;

    // Importing "constructor"
    static import_from_file(path: Path, index=0): PEMKeystore;

    // Importing "constructor"
    static import_all_from_file(path: Path): PEMKeystore[];

    get_keypair(): KeyPair;

    export_to_text(address_hrp: string = "erd"): string;

    export_to_file(path: Path, address_hrp: string = "erd");
```

## Examples of usage

Creating a PEM file to hold a newly created keypair.

```
keypair = KeyPair.generate()

keystore = PEMKeystore(keypair)
keystore.export_to_file(Path("wallet.pem"))
```

Iterating over the accounts in a PEM file.

```
keystores = PEMKeystore.import_all_from_file(Path("file.pem"))

for i in range(len(keystores)):
    keypair = keystores[i].get_keypair()
    pk = keypair.get_public_key()
    address = new Address(public_key.get_bytes(), "erd")
    print("Address", i, address.to_bech32())
```
