## PublicKey

```
classs PublicKey:
    // checks length, can throw InvalidPublicKeyLengthError
    constructor(buffer: bytes);

    verify(data: bytes, signature: bytes): boolean;

    to_address(hrp = "erd"): Address;

    get_bytes(): bytes
```
