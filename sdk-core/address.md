## Address

Generally speaking, the `Address` type should be a `string` in all implementations.

The implementing library can define type aliases, if desired, for example:

```Python
Address = string
```

```Go
type Address = string
```

```JavaScript
type Address = string
```

Furthermore, the implementing library is free to define a wrapper class or structure. This isn't a requirement though. Example:

```
class Address:
    value: string
    to_string(): string
```

Within the specs, we use `Address` and we mean `string`.

## AddressComputer

```
class AddressComputer:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, it can be as follows:
    constructor(encoding_algorithm: IAddressEncodingAlgorithm, num_shards: number);

    is_valid_address(address: Address): boolean;

    // Can throw:
    // - ErrInvalidPublicKey
    // Under the hood, `AddressEncodingAlgorithm` is employed.
    compute_address_from_public_key(public_key: (IPublicKey|bytes)): string;

    // Can throw:
    // - ErrInvalidHexString
    // Under the hood, `AddressEncodingAlgorithm` is employed.
    compute_address_from_hex(public_key: (IPublicKey|bytes)): string;

    // Can throw:
    // - ErrInvalidAddress
    // Under the hood, `AddressEncodingAlgorithm` is employed.
    compute_public_key_from_address(address: Address): (IPublicKey|bytes);

    // Can throw:
    // - ErrInvalidAddress
    // Under the hood, `AddressEncodingAlgorithm` is employed.
    compute_contract_address(deployer: Address, deployment_nonce: number): Address;

    // The number of shards (necessary for computing the shard ID) would be received as a constructor parameter - constructor is not captured by specs.
    get_shard_of_address(address: Address): number;
```

Example of usage:

```
computer = AddressComputer(Bech32EncodingAlgorithm("erd"), num_shards=3)

address = computer.compute_address_from_hex("0139472eff6886771a982f3083da5d421f24c29181e63888228dc81ca60d69e1")
ok = computer.is_valid_address("erd1qyu5wthldzr8wx5c9ucg8kjagg0jfs53s8nr3zpz3hypefsdd8ssycr6th")
shard = computer.get_shard_of_address("erd1qyu5wthldzr8wx5c9ucg8kjagg0jfs53s8nr3zpz3hypefsdd8ssycr6th")
```

### Address encoding algorithm(s)

```
interface IAddressEncodingAlgorithm:
    // Can throw:
    // - ErrInvalidPublicKey
    encode(public_key: (IPublicKey|bytes)): string;

    // Can throw:
    // - ErrInvalidAddress
    decode(address: string): (IPublicKey|bytes);

class Bech32EncodingAlgorithm implements IAddressEncodingAlgorithm:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, it can be as follows:
    constructor(hrp: string = "erd");

    // Implementation of interface.
    ...
```
