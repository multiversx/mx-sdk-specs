
## Cookbook (examples of usage)

Create a new mnemonic and derive the first secret key.

```
provider = new UserWalletProvider()
mnemonic = provider.generate_mnemonic()
print(mnemonic.toString())

sk = provider.derive_secret_key_from_mnemonic(mnemonic, address_index=0 , passphrase="")
pk = provider.compute_public_key_from_secret_key(sk)
address = new Address(public_key, "erd")
print(address.bech32())
```

Generate a secret key and export it to a PEM keystore.

```
provider = new UserWalletProvider()
sk, _ = provider.generate_keypair()
keystore = PEMKeystore.new_from_secret_key(provider, sk)
keystore.export_to_file("wallet.pem", "erd")
```

Generate a mnemonic and export it to a JSON keystore.

```
provider = new UserWalletProvider()
mnemonic = provider.generate_mnemonic()
keystore = EncryptedKeystore.new_from_mnemonic(provider, mnemonic)
keystore.export_to_file("wallet.json", "password", "erd")
```

Loading a JSON keystore which holds the encrypted mnemonic, then iterate over the first 3 accounts:

```
provider = new UserWalletProvider()
keystore = EncryptedKeystore.import_from_file(provider, "wallet.json", "password")
mnemonic = keystore.get_mnemonic()

for i in [0, 1, 2]:
    secret_key = provider.derive_secret_key_from_mnemonic(mnemonic, address_index=i, passphrase="")
    public_key = provider.compute_public_key_from_secret_key(secret_key)
    address = new Address(public_key, "erd")
    print("Address", i, address.bech32())
```

Changing the password of an existing keystore:

```
provider = new UserWalletProvider()
keystore = EncryptedKeystore.import_from_file(provider, "wallet.json", "password")
keystore.export_to_file("wallet.json", "new_password", "erd")
```

Loading a PEM keystore and sign a piece of data:

```
provider = new UserWalletProvider()
keystore = PEMKeystore.import_from_file(provider, "wallet.pem")
first_secret_key = keystore.get_secret_key(0)
signature = provider.sign(data, first_secret_key)
```

Loading a JSON keystore and sign data:

```
provider = new UserWalletProvider()
keystore = EncryptedKeystore.import_from_file(provider, "wallet.json", "password")

if keystore.get_kind() == "secretKey":
    first_secret_key = keystore.get_secret_key()
else:
    mnemonic = keystore.get_mnemonic()
    first_secret_key = provider.derive_secret_key_from_mnemonic(mnemonic, address_index=0, passphrase="")

signature = provider.sign(data, first_secret_key)
```
