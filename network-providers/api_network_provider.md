## ApiNetworkProvider

`ApiNetworkProvider` allows one to interact with the MultiversX (General) API.

```
class ApiNetworkProvider implements INetworkProvider;
    // The constructor should allow one to specify the base URL, custom request headers, etc.

    // Fetches a block by nonce or by hash.
    get_block(block_hash: str | bytes): BlockOnNetwork;

    // Fetches the latest block of a shard.
    get_latest_block(): BlockOnNetwork;
```
