## Resources

```
dto NativeAuthDecoded:
    ttl: number;
    origin: string;
    address: Address;
    signature: bytes;
    block_hash: string;
    body: string;
    extra_info?: Any;
```

```
dto NativeAuthValidateResult:
    issued: number;
    expires: number;
    address: Address;
    signer_address: Address;
    origin: string;
    extra_info?: Any;
```

```
dto WilcardOrigin:
    protocol: string;
    domain: string;
```
