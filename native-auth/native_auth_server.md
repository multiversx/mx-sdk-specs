## NativeAuthServer

```
class NativeAuthServer:
    constructor(config: NativeAuthServerConfig);

    // decodes the accessToken in its components: ttl, origin, address, signature, block_hash & body
    decode(access_token: string): NativeAuthDecoded;

    // decodes and validates the accessToken.
    // Performs validation of the block hash, verifies its validity, as well as origin verification
    async validate(access_token: string): NativeAuthValidateResult;
```
