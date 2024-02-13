## Message

```
dto Message:
    data: bytes;
    signature: Optional[bytes];
    address: Optional[IAddress];
    version: Optional[int]
```

The only manadatory parameter for the constructor it's `data`. The others can be `undefined` or have default empty values.
If `version` is not provided the default value should be `1`.

## MessageComputer

```
class MessageComputer:
    compute_bytes_for_signing(message: Message): bytes;

    // return the call of `compute_bytes_for_signing()`
    compute_bytes_for_verifying(messge: Message): bytes;

    pack_message(message: Message): {
        message: string; // hex encoded
        signature: string; // hex encoded
        address: string; // bech32 representation
        version: int;
    };

    // packed_message should be the one obtained from calling `pack_message()`
    // should treat both 'legacy message' and current message
    unpack_message(packed_message: {
        message: string;
        signature: string;
        address: string;
        version: int;
    }): Message;
```

Reference implementation of `compute_bytes_for_signing`:

```
compute_bytes_for_signing(message: Message):
    PREFIX = bytes.fromhex("17456c726f6e64205369676e6564204d6573736167653a0a")
    size = length of message.data, encoded as a string
    content = concat(PREFIX, size, message.data)
    content_hash = keccak(digest_bits=256)
    
    return content_hash
```
