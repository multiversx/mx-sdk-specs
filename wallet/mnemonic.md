## Mnemonic

This component allows one to load / parse an existing mnemonic.

```
class Mnemonic:
    // At least one of the following constructors should be implemented.
    // The constructor(s) should also trim whitespace.
    constructor(text: string);
    constructor(words: string[]);

    // Alternatively, named constructors can be used:
    static new_from_text(text: string): Mnemonic;
    static new_from_words(words: string[]): Mnemonic;

    static new_from_entropy(entropy: bytes): Mnemonic;

    static generate(): Mnemonic;

    static validate_mnemonic(mnemonic: Mnemonic): bool;

    // Can throw:
    // - ErrInvalidMnemonic
    // Below, "passphrase" is the optional bip39 passphrase used to derive a secret key from a mnemonic.
    // Reference: https://en.bitcoin.it/wiki/Seed_phrase#Two-factor_seed_phrases
    derive_keypair(address_index: int = 0, passphrase: string = ""): KeyPair;

    get_entropy(): bytes;

    // Gets the mnemonic words.
    get_words(): string[];

    // Returns the mnemonic as a string.
    to_string(): string;
```

## Examples of usage

Creating a new mnemonic and deriving the first secret key.

```
mnemonic = Mnemonic.generate()
print(mnemonic.toString())

mnemonic = Mnemonic.newfromText("...")
keypair = mnemonic.derive_keypair()

pk = keypair.get_public_key()
address = pk.to_address()
print(address.to_bech32())
```
