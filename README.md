# Specifications for mx-sdk libraries

This repository contains specifications for the `mx-sdk-*` libraries. The specifications are written in a language-agnostic manner, and are meant to be implemented in multiple languages (e.g. Go, TypeScript, Python, Rust, etc.).

## Structure

- `core`: core components (address, transaction, etc.).
- `wallet`: core wallet components (generation, signing).
- `network-providers`: network provider (API, Gateway) components.
- `abi`: ABI components and ABI-aware codecs.
- `adapters`: components that resolve the impedance mismatch between interfaces of different domains.
- `converters`: components that are able to convert concepts (structures, classes) between different domains.

Below, we add specific details for some of the most important packages and sub-components.

### Transactions Factories

These components are located in `core/transactions-factories` and are responsible with creating transactions for specific use cases. They are designed as _multi-factory_ classes, having methods that return a `Transaction` object constructed by following specific recipes (with respect to the Protocol).

The methods are named in correspondence with the use cases they implement, e.g. `create_transaction_for_native_transfer()` or `create_transaction_for_new_delegation_contract()`. They return a `Transaction` (data transfer object), where `sender`, `receiver`, `value`, `data` and `gasLimit` are properly set (upon eventual computation, where applicable).

### Transactions Controllers

The transaction controllers are components built upon the lower-level transaction factories and transaction outcome parsers. They are able to create signed transactions and parse the outcome of these transactions. The controllers are specialized for a "family" of transactions (e.g. transfer transactions, delegation transactions, smart contract transactions), just like the factories and the outcome parsers.

One controller is backed by **one transaction factory and one outcome parser** (paired).

- All functions that create transactions receive as first parameter the `sender: IAccount`. They also receive an optional `guardian: IAccount`.
- All functions that create transactions receive a nonce, which is optional. If not provided, the nonce is fetched from the network.
- All functions that create transactions return already-signed transactions.
- All functions that parse transactions outcomes receive a `TransactionOnNetwork`.
- All functions that parse transactions outcomes are paired with an additional function that awaits the completion of the transaction on the network (before parsing the outcome). For example, `parse_deploy` is paired with `await_completed_deploy`.

## Guidelines

### **`in-ifaces-out-concrete-types`**

Generally speaking, it's recommended to receive input parameters as abstractions (interfaces) in the public API of the SDKs. This leads to an improved decoupling, and allows for easier type substitution (e.g. easier mocking, testing).

Exception to the rule above: when receiving easily constructable objects (plain structures or DTOs) as input parameters, **it's perfectly fine to receive them as concrete types**.

Generally speaking, it's recommended to return concrete types in the public API of the SDKs. The client code is responsible with decoupling from unnecessary data and behaviour of returned objects (e.g. by using interfaces, on their side). The only notable exception to this is when working with factories (abstract or method factories) that should have the function output an interface type. For example, have a look over `(User|Validator)WalletProvider.generate_keypair()` - this method returns abstract types (interfaces).

### **`pay-attention-to-types`**

- For JavaScript / TypeScript, `bytes` should be `Uint8Array`.
- For JavaScript / TypeScript, use `bigint` when applicable (amounts, gas limit, nonces), instead of `number`.

### **`follow-language-conventions`**

- Make sure to follow the naming conventions of the language you're using, e.g. `snake_case` vs. `camelCase`.
- In the specs, interfaces are prefixed with `I`, simply to make them stand out. However, in the implementing libraries, this convention does not have to be applied.
- In `go`, the term `serialize` (whether it's part of a class name or a function name) can be replaced by `marshal`, since that is the convention.
- Errors should also follow the language convention - e.g. `ErrInvalidPublicKey` vs `InvalidPublicKeyError`. Should have the same error message in all implementing libraries, though.
- Generally speaking, named constructors should be prefixed with `new` - e.g. `Address.new_from_bech32()`. This guideline is especially recommended for Go and Python, but TypeScript code can also benefit from adopting this convention.
- Names of classes, functions, parameters, and other identifiers should adhere to the specifications as closely as possible. However, if a specified name is a reserved keyword in the implementing language, it should be replaced with an alternative name (e.g., replace `function` with `func`, `arguments` with `args`).

### **`plain-representation-of-object`**

When appropriate, classes should define methods for converting to and from plain objects. The definition of a "plain object" depends on the implementation language. For example, in Python, it could be a `dict`, while in TypeScript, it might be an `object`.

Python example:

```
class MyClass:
    // Named constructor:
    new_from_dict(value: dict[Any, str]): MyClass;

    // Conversion utility method:
    to_dict(): dict[Any, str];
```

TypeScript example:

```
class MyClass:
    // Named constructor:
    newFromJSON(value: any): MyClass;

    // Conversion utility method:
    toJSON(): any;
```

## **`any-object`**

In the specs, `object` is used as a placeholder for any type of object. In the implementing libraries, this would be replaced with the most appropriate type. For example:

- in Go:
  - before Go 1.18: `map[string]interface{}` or directly `interface{}` (depending on the context)
  - after Go 1.18: `map[string]any` or directly `any` (depending on the context)
- in Python: `dict` or `Any`
- in JavaScript / TypeScript: `object` or `any`
