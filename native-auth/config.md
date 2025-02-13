## NativeAuthClientConfig

```
dto NativeAuthClientConfig:
    origin?: string;
    api_url?: string;
    expiry_time_in_seconds?: int;
    block_hash_shard?: int;
    gateway_url?: string;
    extra_request_headers?: dict[string, Any];
```

## NativeAuthServerConfig

```
dto NativeAuthServerConfig:
    // Source of current block.
    api_url: string;

    // A required list of accepted origins for validating incoming request domains.
    accepted_origins: string[];

    // The endpoint where the impersonation is validated
    // This is called if the extraInfo payload contains the `multisig` or `impersonate` attribute.
    validate_impersonate_url?: string;

    // An optional function that returns a boolean if the impersonation is accepted
    // This is called if the extraInfo payload contains the `multisig` or `impersonate` attribute.
    validate_impersonate_callback?: string;

    // An optional function that returns a boolean if the origin received as a parameter is accepted.
    // This is called only if the origin is not in the list of accepted origins defined in `acceptedOrigins`
    isOriginAccepted?: (origin: string): boolean;

    // Maximum allowed TTL from the token. Default: one day (86400 seconds)
    maxExpirySeconds: number = 86400;

    skipLegacyValidation?: boolean;

    extraRequestHeaders?: { [key: string]: string };

    An optional function that returns a boolean if the signature is valid.
    // This is called only if you want to override the signature verification method
    verifySignature?: (address: string, messageString: string, signature: bytes): boolean;
```
