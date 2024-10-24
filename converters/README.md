# Converters

This package holds components that are able to convert concepts (structures, classes) between different domains. By domains, we mean, for example: `core`, `wallet`, `network-providers`.

Components in this package must not be referenced by `core`, `wallet` and `network-providers`. Ideally, they should only be referenced by client code or downstream SDKs.
