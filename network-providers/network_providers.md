## ProxyNetworkProvider

`ProxyNetworkProvider` allows one to interact with the MultiversX Gateway (Proxy) API.

```
class ProxyNetworkProvider implements INetworkProvider;
```

## ApiNetworkProvider

`ApiNetworkProvider` allows one to interact with the MultiversX (General) API.

```
class ApiNetworkProvider implements INetworkProvider;
```

## INetworkProvider

```
interface INetworkProvider:
    get_network_config(): NetworkConfig;

    // Block coordinates are optional, for deep-history lookups.
    get_account(address: IAddress, block_coordinates?: BlockCoordinates): AccountOnNetwork;

    get_transaction(transaction_hash: bytes | string, with_status: bool): TransactionOnNetwork;
```
