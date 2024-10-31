# Network Providers

This package holds components that are able to interact with the network (e.g. send transactions, query blockchain state), and their corresponding resources (e.g. request & responses structures).

- `ProxyNetworkProvider`, following the `INetworkProvider` interface - must be implemented by the SDK.
- `ApiNetworkProvider`, following the `INetworkProvider` interface - must be implemented by the SDK.
- `NodeNetworkProvider`, following the `INetworkProvider` interface - must be implemented by the SDK.
