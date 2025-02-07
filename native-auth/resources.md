## Resources

```
dto NativeAuthDecoded:
    ttl: number;
    origin: string;
    address: Address;
    signature: string;
    block_hash: string;
    body: string;
    extra_info?: Any;
```

```
dto NativeAuthResult:
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

```
interface NativeAuthCache {
  getValue(key: string): number | undefined;

  setValue(key: string, value: number, ttl: number);
}
```
