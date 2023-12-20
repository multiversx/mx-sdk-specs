## UserWalletProvider

Provides functionality for generating secret keys, derivating public keys, signing & verifying data. 

Additionally, allows one to generate and validate mnemonics, and to derive secret keys from mnemonics (i.e. BIP39).

```
class UserWalletProvider implements ISigner, IVerifier, IKeysGenerator, IMnemonicComputer:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, the constructor can be parametrized with underlying, more low-level crypto components, if applicable.

    // Should not throw.
    generate_keypair(): (ISecretKey, IPublicKey)

    // Can throw:
    // - ErrInvalidSecretKey
    sign(data: bytes, secret_key: ISecretKey): bytes

    // Can throw:
    // - ErrInvalidPublicKey
    // - ErrInvalidSignature
    verify(data: bytes, signature: bytes, public_key: IPublicKey): bool

    // Can throw:
    // - ErrInvalidSecretKeyBytes
    create_secret_key_from_bytes(data: bytes): ISecretKey

    // Can throw:
    // - ErrInvalidPublicKeyBytes
    create_public_key_from_bytes(data: bytes): IPublicKey

    // Can throw:
    // - ErrInvalidSecretKey
    compute_public_key_from_secret_key(secret_key: ISecretKey): IPublicKey

    // Should not throw.
    generate_mnemonic(): Mnemonic

    // Should not throw.
    validate_mnemonic(mnemonic: Mnemonic): bool

    // Can throw:
    // - ErrInvalidMnemonic
    // Below, "passphrase" is the optional bip39 passphrase used to derive a secret key from a mnemonic.
    // Reference: https://en.bitcoin.it/wiki/Seed_phrase#Two-factor_seed_phrases
    derive_secret_key_from_mnemonic(mnemonic: Mnemonic, address_index: int, passphrase: string = ""): ISecretKey
```
