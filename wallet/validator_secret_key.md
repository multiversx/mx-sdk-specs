## ValidatorSecretKey

```
class ValidatorSecretKey:
    // checks length, can throw InvalidSecretKeyLengthError
    constructor(buffer: bytes);

    static from_string(buffer_hex: str): ValidatorSecretKey;

    static generate(): SecretKey;

    sign(data: bytes): bytes;

    get_public_key(): ValidatorPublicKey;

    get_bytes(): bytes
```
