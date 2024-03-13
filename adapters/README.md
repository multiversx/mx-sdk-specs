# Adapters

This package holds components acting as adapters that resolve the impedance mismatch between interfaces of different domains.

Components in this package must not be referenced by `core`, `wallet` and `network-providers`. Ideally, they should only be referenced by client code or downstream SDKs.

## Example: query runners and network providers

Say, for example, that `core.SmartContractQueriesController` depends on a query runner, defined as the interface `IQueryRunner`, to perform queries against the network.

Such a query runner must have no proper implementation in `core`, since the `core` package should not directly reference components that act as mere plugins.

Instead, the package `network-providers` offers a `(Proxy|API)NetworkProvider` which, in practice, should be able to provide such a feature (running a query).

However, since `(Proxy|API)NetworkProvider` was not designed with the `IQueryRunner` interface in mind (which is a good thing), we need to adapt it to the `IQueryRunner` interface, so that it's compatible with `core.SmartContractQueriesController`.

The best place for such an adapter is the `adapters` package.

## References

- [Adapter pattern (Wikipedia)](https://en.wikipedia.org/wiki/Adapter_pattern)
