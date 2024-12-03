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
    recall_account_nonce(address: Address): uint64;

    // Signs the message and sets the signature field of the message
    def sign_message(self, message: Message, account: Account);

    // Signs the transaction and sets the signature field of the transaction
    sign_transaction(self, transaction: Transaction, account: Account);

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
    create_transfers_controller(): TransfersController;
    create_token_management_controller(): TokenManagementController;
    create_delegation_controller(): DelegationController;
    create_relayed_controller(): RelayedController;
    create_account_management__controller(): AccountManagementController;

    // Access to the individual factories.
    create_smart_contract_factory(abi: Optional[Abi]): SmartContractTransactionsFactory;
    create_transfers_factory(): TransfersTransactionsFactory;
    create_token_management_factory(): TokenManagementTransactionsFactory;
    create_delegation_factory(): DelegationTransactionsFactory;
    create_relayed_factory(): RelayedTransactionsFactory;
    create_account_management__factory(): AccountManagementTransactionsFactory;
```

## Examples

### Transfer native amounts and / or custom tokens

```
entrypoint = MainnetEntrypoint();
controller = entrypoint.create_transfers_controller()

sender = Account.new_from_pem("alice.pem");
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

sender = Account.new_from_pem("alice.pem");
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

sender = Account.new_from_pem("alice.pem");
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
relayed_controller = entrypoint.create_relayed_controller();

sender = Account.new_from_pem("alice.pem");
relayer = Account.new_from_pem("carol.pem");
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
    inner_transaction: user_transaction
});

transaction_hash = entrypoint.send_transaction(transaction);
outcome = entrypoint.await_completed_transaction(transaction_hash);
```

### Issue a fungible token, do a quick airdrop

```
entrypoint = MainnetEntrypoint();
tokens_controller = entrypoint.create_token_management_controller();
transfers_controller = entrypoint.create_transfers_controller();

sender = Account.new_from_pem("alice.pem");
sender.nonce = entrypoint.recall_account_nonce(sender.address);

transaction = tokens_controller.create_transaction_for_issuing_fungible({
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    token_name: "EXAMPLE",
    token_ticker: "EXAMPLE",
    initial_supply: 1000000000000000000000,
    num_decimals: 18,
    can_freeze: true,
    can_wipe: true,
    can_pause: true,
    can_transfer_nft_create_role: true,
    can_change_owner: true,
    can_upgrade: true,
    can_add_special_roles: true
});

transaction_hash = entrypoint.send_transaction(transaction);
parsed_outcome = tokens_controller.await_completed_issue_fungible(transaction_hash);
token_identifier = parsed_outcome.token_identifier;

receivers = [
    Address.from_bech32("erd1..."),
    Address.from_bech32("erd1..."),
    Address.from_bech32("erd1...")
];

transactions = [];

for receiver in receivers:
    transaction = transfers_controller.create_transaction_for_transfer({
        sender: sender,
        nonce: sender.get_nonce_then_increment(),
        receiver: receiver,
        token_transfers: [
            token: Token(token_identifier),
            amount: 1000000000000000000
        ]
    });

    transactions.append(transaction);

entrypoint.send_transactions(transactions);
```

### Access underlying components

```
entrypoint = MainnetEntrypoint();

controller = entrypoint.create_delegation_controller()
controller = entrypoint.create_token_management_controller()
network_provider = entrypoint.get_network_provider()
```
