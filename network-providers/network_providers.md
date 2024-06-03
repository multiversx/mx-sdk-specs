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
    // Fetches the general configuration of the network.
    get_network_config(): NetworkConfig;

    // Fetches account information for a given address.
    // Block coordinates are optional, for deep-history lookups.
    get_account(address: IAddress, block_coordinates?: BlockCoordinates): AccountOnNetwork;

    // Fetches a transaction that was previously broadcasted (maybe already processed by the network).
    get_transaction(transaction_hash: bytes | string, with_status: bool): TransactionOnNetwork;

    // Broadcasts a transaction and returns its hash.
    send_transaction(transaction: Transaction): bytes;

    // Broadcasts multiple transactions and returns a tuple of (number of accepted transactions, list of transaction hashes).
    // In the returned list, the order of transaction hashes corresponds to the order of transactions in the input list.
    // If a transaction is not accepted, its hash is empty in the returned list.
    send_transactions(transactions: List[Transaction]): Tuple[int, List[bytes]];

    // Queries a smart contract.
    // Block coordinates are optional, for deep-history lookups.
    query_contract(query: SmartContractQuery, block_coordinates?: BlockCoordinates): SmartContractQueryResponse;
```
