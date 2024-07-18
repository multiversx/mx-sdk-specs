# Network Providers

This package holds components that are able to interact with the network (e.g. send transactions, query blockchain state), and their corresponding resources (e.g. request & responses structures).

## Basic network providers

- `BasicProxyNetworkProvider`, following the `IBasicNetworkProvider` interface - must be implemented by the SDK.
- `BasicApiNetworkProvider`, following the `IBasicNetworkProvider` interface - must be implemented by the SDK.
- `BasicNodeNetworkProvider`, following the `IBasicNetworkProvider` interface - must be implemented by the SDK.

## Fully-featured network providers

- `ProxyNetworkProvider`, mirroring the Open API specification of https://gateway.multiversx.com - can be (optionally) implemented by the SDK.
- `ApiNetworkProvider`, mirroring the Open API specification of https://api.multiversx.com - can be (optionally) implemented by the SDK.
- `NodeNetworkProvider`, mirroring the Open API specification of the Node API - can be (optionally) implemented by the SDK.
