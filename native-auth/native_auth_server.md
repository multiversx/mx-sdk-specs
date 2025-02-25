## NativeAuthServer

```
class NativeAuthServer:
    constructor(config: NativeAuthServerConfig, cache: NativeAuthCache);

    // decodes the access token in its components: ttl, origin, address, signature, block_hash & body
    decode(access_token: string): NativeAuthDecoded;

    // decodes and validates the access token.
    // Performs validation of the block hash, verifies its validity, as well as origin verification
    validate(access_token: string): NativeAuthValidateResult;

    is_valid(access_token: str): boolean;
```
