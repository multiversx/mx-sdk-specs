## ProxyNetworkProvider

`ProxyNetworkProvider` allows one to interact with the MultiversX Gateway (Proxy) API.

```
class ProxyNetworkProvider implements INetworkProvider;
    // The constructor should allow one to specify the base URL, custom request headers, etc.

    // Fetches a block by nonce or by hash.
    get_block(shard: number, block_hash: str | bytes, block_nonce: number): BlockOnNetwork;

    // Fetches the latest block of a shard. Shard defaults to METACHAIN.
    get_latest_block(shard: number): BlockOnNetwork;
```
