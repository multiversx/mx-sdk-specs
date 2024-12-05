## ValidatorPublicKey

```
classs ValidatorPublicKey:
    // checks length, can throw InvalidPublicKeyLengthError
    constructor(buffer: bytes);

    verify(data: bytes, signature: bytes): boolean;

    get_bytes(): bytes
```
