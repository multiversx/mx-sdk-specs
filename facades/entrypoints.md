Questions:

(1) have load_account as a single method, or separate, explicit functions (3 methods)?

(2) please confirm that the suggested constructor is all right. Should we or should we not apply heuristics to infer the kind of network provider based on its URL?

(3) even if creation (and signing) of transactions is done by controllers, broadcasting isn't (see examples). All good?

## Account

```
class Account:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Suggestions:
    constructor(secret_key: bytes, hrp: Optional[str]);
    constructor(user_signer: IUserSigner, hrp: Optional[str]);

    address: IAddress;

    // Local copy of the account nonce.
    // Must be explicitly managed by client code.
    nonce: uint64;

    sign(data: bytes): bytes;

    // Gets the nonce (the one from the object's state) and increments it.
    get_nonce_then_increment(): uint64;

    // Named constructor
    // Loads a secret key from a PEM file. PEM files may contain multiple accounts - thus, an (optional) "index" is used to select the desired secret key.
    // Returns an Account object, initialized with the secret key.
    static new_from_pem(path: Path, index: Optional[int], hrp: Optional[str]): Account;

    // Named constructor
    // Loads a secret key from an encrypted keystore file. Handles both keystores that hold a mnemonic and ones that hold a secret key (legacy).
    // For keystores that hold an encrypted mnemonic, the optional "address_index" parameter is used to derive the desired secret key.
    // Returns an Account object, initialized with the secret key.
    static new_from_keystore(path: Path, password: str, address_index: Optional[int], hrp: Optional[str]): Account;

    // Named constructor
    // Loads (derives) a secret key from a mnemonic. The optional "address_index" parameter is used to guide the derivation.
    // Returns an Account object, initialized with the secret key.
    static new_from_mnemonic(mnemonic: str, address_index: Optional[int], hrp: Optional[str]): Account;
```

## Pre-defined entrypoints

Pre-defined entrypoints inherit from `NetworkEntrypoint` and use sensible default values.

```
class MainnetEntrypoint extends NetworkEntrypoint(ApiNetworkProvider("https://api.multiversx.com"));

class DevnetEntrypoint extends NetworkEntrypoint(ApiNetworkProvider("https://devnet-api.multiversx.com"));

class TestnetEntrypoint extends NetworkEntrypoint(ApiNetworkProvider("https://testnet-api.multiversx.com"));
```

## NetworkEntrypoint

The `NetworkEntrypoint` acts as a facade for interacting with the network. It should cover the most common use-cases "out of the box".

**(a)** The facade should allow one to access the underlying, more lower-level components (e.g. transaction controllers, network provider).

**(b)** The facade should promote the most commonly-used functions of the network provider.

**(c)** The facade should fetch network parameters (network config) needed for construction transactions (e.g. chain ID) in a lazy manner. For example, it may fetch them when the first transaction is created. This works fine for JavaScript, as well, since transaction-creation functions are async anyway.

```
class NetworkEntrypoint:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, it can be parametrized with: a network provider URL and kind (proxy, API)
    constructor(url: str, kind: Optional[str]);

    verify_transaction_signature(transaction: Transaction): bool;
    verify_message_signature(message: Message): bool;

    // Fetches the account nonce from the network.
    recall_account_nonce(address: IAddress): uint64;

    // Function of the network provider, promoted to the facade.
    send_transaction(transaction: Transaction): string;

    // Function of the network provider, promoted to the facade.
    send_transactions(transaction: Transaction); Tuple[int, List[string]];

    // Generic function to await a transaction on the network.
    await_completed_transaction(transaction_hash: string): TransactionOnNetwork;

    // Access to the underlying network provider.
    get_network_provider(): INetworkProvider;

    // Access to the individual controllers.
    create_smart_contract_controller(abi: Optional[Abi]): SmartContractController;
    get_transfers_controller(): TransfersController;
    get_token_management_controller(): TokenManagementController;
    get_delegation_controller(): DelegationController;
    get_relayed_controller(): RelayedController;
    get_account_management__controller(): AccountManagementController;
```

## Examples

### Transfer native amounts and / or custom tokens

```
entrypoint = MainnetEntrypoint();
controller = entrypoint.get_transfer_transactions_controller()

sender = entrypoint.load_account_from_pem("alice.pem");
sender.nonce = entrypoint.recall_account_nonce(sender.address);

transaction = controller.create_transaction_for_transfer({
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    receiver: Address.from_bech32("erd1..."),
    native_amount: 1000000000000000000,
    token_transfers: [];
});

// Broadcast transaction (option 1):
transaction_hash = entrypoint.send_transaction(transaction);
// Broadcast transaction (option 2):
transaction_hash = entrypoint.get_network_provider().send_transaction(transaction);

// Await transaction completion (option 1, await and specialized outcome parsing):
parsed_outcome = controller.await_completed_transfer(transaction_hash);
// Await transaction completion (option 2, simple await):
transaction_on_network = entrypoint.await_completed_transaction(transaction_hash);
```

### Deploy a contract

```
entrypoint = MainnetEntrypoint();
abi = Abi.load("adder.abi.json");
controller = entrypoint.create_smart_contract_controller(abi);

sender = entrypoint.load_account_from_pem("alice.pem");
sender.nonce = entrypoint.recall_account_nonce(sender.address);

transaction = entrypoint.create_transaction_for_deploy({
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    bytecode: bytecode,
    arguments: [arg1, arg2],
    gas_limit: 10000000
});

transaction_hash = entrypoint.send_transaction(transaction);
parsed_outcome = entrypoint.await_completed_contract_deploy(abi, transaction_hash);
```

### Call a contract

```
entrypoint = MainnetEntrypoint();
abi = Abi.load("adder.abi.json");
controller = entrypoint.create_smart_contract_controller(abi);

sender = entrypoint.load_account_from_pem("alice.pem");
sender.nonce = entrypoint.recall_account_nonce(sender.address);

transaction = controller.create_transaction_for_execute({
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    contract: Address.from_bech32("erd1..."),
    function: "add",
    arguments: [arg1, arg2],
    gas_limit: 10000000
});

transaction_hash = entrypoint.send_transaction(transaction);
parsed_outcome = controller.await_completed_execute(transaction_hash);
```

### Query a contract

```
entrypoint = MainnetEntrypoint();
abi = Abi.load("adder.abi.json");
controller = entrypoint.create_smart_contract_controller(abi);

parsed_outcome = controller.query({
    contract: Address.from_bech32("erd1..."),
    function: "get",
    arguments: [arg1, arg2],
});
```

### Create a relayed contract call

```
entrypoint = MainnetEntrypoint();
abi = Abi.load("adder.abi.json");
contract_controller = entrypoint.create_smart_contract_controller(abi);
relayed_controller = entrypoint.get_relayed_controller();

sender = entrypoint.load_account_from_pem("alice.pem");
relayer = entrypoint.load_account_from_pem("carol.pem");
sender.nonce = entrypoint.recall_account_nonce(sender.address);
relayer.nonce = entrypoint.recall_account_nonce(relayer.address);

user_transaction = controller.create_transaction_for_execute({
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    contract: Address.from_bech32("erd1..."),
    function: "add",
    arguments: [arg1, arg2],
    gas_limit: 10000000
});

transaction = relayed_controller.create_relayed_transaction({
    relayer: relayer,
    nonce: relayer.get_nonce_then_increment(),
    inner_transactions: [user_transaction]
});

transaction_hash = entrypoint.send_transaction(transaction);
outcome = entrypoint.await_completed_transaction(transaction_hash);
```

### Access underlying components

```
entrypoint = MainnetEntrypoint();

controller = entrypoint.get_delegation_controller()
controller = entrypoint.get_token_management_controller()
network_provider = entrypoint.get_network_provider()
```
