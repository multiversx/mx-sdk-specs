## CodeMetadata

```
class CodeMetadata:
    upgradeable?: boolean; // true, by default
    readable?: boolean; // true, by default
    payable?: boolean; // false, by default
    payable_by_sc?: boolean; // false, by default

    // constructor
    constructor(
        upgradeable?: boolean,
        readable?: boolean,
        payable?: boolean,
        payable_by_sc?: boolean
    )

    // Named constructor
    // Should check that data has correct length (2 bytes)
    static new_from_bytes(data: bytes): Address;

    // Returns the code metadata as bytes
    to_bytes(): bytes;

    // Returns the code metadata as a hex string
    to_string(): string;
```
