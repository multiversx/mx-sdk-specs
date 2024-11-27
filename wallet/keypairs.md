## KeyPair

```
// stores both a secret key and a public key
class KeyPair:
    constructor(secret_key: SecretKey): KeyPair;

    // Should not throw.
    generate(): KeyPair;

    sign(data: bytes): bytes;

    verify(data: bytes, signature: bytes): bool;

    // Can throw:
    // - ErrInvalidSecretKeyBytes
    static new_from_bytes(data: bytes): KeyPair;

    get_secret_key(): SecretKey;

    get_public_key(): PublicKey;
```
