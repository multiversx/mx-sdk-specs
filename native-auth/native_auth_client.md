## NativeAuthClient

```
class NativeAuthClient:
    // config defaults to a NativeAuthClientConfig object with default values
    constructor(config?: NativeAuthClientConfig);

    // returns an initial token, that is then used for the final token
    initialize(extra_info?: dict[Any, Any]): string;

    // returns the token that will be signed; 'init_token' is the token returned after calling 'initialize()'.
    get_token_for_signing(address: Address, init_token: string): bytes;

    // returns the final native auth token
    // signature is passed as a hex-string
    get_token(address: Address, token: string, signature: string): string;
```
