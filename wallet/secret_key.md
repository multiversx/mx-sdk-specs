## SecretKey

```
class SecretKey:
    // checks length, can throw InvalidSecretKeyLengthError
    constructor(buffer: bytes);

    static new_from_string(buffer_hex: str): SecretKey;

    static generate(): SecretKey;

    sign(data: bytes): bytes;

    get_public_key(): PublicKey;

    get_bytes(): bytes
```
