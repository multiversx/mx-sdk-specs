Questions:

(1) have load_account as a single method, or separate, explicit functions (3 methods)?

(2) please confirm that the suggested constructor is all right. Should we or should we not apply heuristics to infer the kind of network provider based on its URL?

(3) what to do when the developer wants to use a non-promoted function of a transactions factory?

(4) how about having a higher-level factory for each transactions factory? Pre-construction logic, post-construction logic.

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

**(a)** All functions that create transactions receive as first parameter the `sender: Account`. And an optional `guardian: Account`.

**(b)** All functions that create transactions receive a nonce, which is optional. If not provided, the nonce is fetched from the network.

**(c)** All functions that create transactions return already-signed transactions.

**(d)** All functions that create smart-contract transactions receive an optional `Abi` as a parameter.

**(e)** All functions that parse the outcome of a transaction receive an optional `Abi` as a parameter.

**(f)** The facade should allow one to access the underlying, more lower-level components (e.g. transaction factories, network provider).

**(g)** The facade should promote the most commonly-used functions of the network provider.

**(h)** The facade should promote the most commonly-used functions of the transactions factories.

**(i)** The facade should fetch network parameters (network config) needed for construction transactions (e.g. chain ID) in a lazy manner. For example, it may fetch them when the first transaction is created. This works fine for JavaScript, as well, since transaction-creation functions are async anyway.

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
    get_account(address: IAddress): AccountOnNetwork;

    // Function of the network provider, promoted to the facade.
    get_transaction(transaction_hash: string): TransactionOnNetwork;

    // Function of the network provider, promoted to the facade.
    send_transaction(transaction: Transaction): string;

    // Function of the network provider, promoted to the facade.
    send_transactions(transaction: Transaction); Tuple[int, List[string]];

    // Generic function to await a transaction on the network.
    // This, in contrast to the await* functions specialized for contract deployment and execution, does not parse the transaction.
    await_completed_transaction(transaction_hash: string): TransactionOnNetwork;

    create_transaction_for_transfer({
        sender: Account;
        nonce: Optional[int];
        receiver: IAddress;
        native_amount: Optional[Amount];
        data: Optional[bytes];
        token_transfers: TokenTransfer[];
    }): Transaction;

    create_transaction_for_contract_deploy({
        abi: Optional[Abi];
        sender: Account;
        nonce: Optional[int];
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        is_upgradeable: bool = True;
        is_readable: bool = True;
        is_payable: bool = False;
        is_payable_by_contract_: bool = True;
        gas_limit: uint32;
    }): Transaction;

    // This method is less important (supports an exotic flow).
    parse_contract_deploy({
        abi: Optional[Abi];
        transaction_on_network: TransactionOnNetwork
    }): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            ownerAddress: string;
            codeHash: bytes;
        }];
    };

    // Specialized function to await and parse a contract deployment transaction.
    // Does "await_completed_transaction" and "parse_contract_deploy" in one go.
    await_completed_contract_deploy({
        abi: Optional[Abi];
        transaction_hash: string;
    }): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            ownerAddress: string;
            codeHash: bytes;
        }];
    };

    create_transaction_for_contract_upgrade({
        abi: Optional[Abi];
        sender: Account;
        nonce: Optional[int];
        contract: IAddress;
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        is_upgradeable: bool = True;
        is_readable: bool = True;
        is_payable: bool = False;
        is_payable_by_contract_: bool = True;
        gas_limit: uint32;
    }): Transaction;

    // This method is less important (supports an exotic flow).
    parse_contract_upgrade({
        abi: Optional[Abi];
        transaction_on_network: TransactionOnNetwork
    }): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            codeHash: bytes;
        }];
    };

    // Specialized function to await and parse a contract upgrade transaction.
    // Does "await_completed_transaction" and "parse_contract_upgrade" in one go.
    await_completed_contract_upgrade({
        abi: Optional[Abi];
        transaction_hash: string;
    }): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            codeHash: bytes;
        }];
    };

    create_transaction_for_contract_execute({
        abi: Optional[Abi];
        sender: Account;
        nonce: Optional[int];
        contract: IAddress;
        // If "function" is a reserved word in the implementing language, it should be replaced with a different name (e.g. "func" or "functionName").
        function: string;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        token_transfers: List[TokenTransfer] = [];
        gas_limit: uint32;
    }): Transaction;

    // This method is less important (supports an exotic flow).
    parse_contract_execute({
        abi: Optional[Abi];
        transaction_on_network: TransactionOnNetwork,
        function?: string
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };

    // Specialized function to await and parse a contract execution transaction.
    // Does "await_completed_transaction" and "parse_contract_execute" in one go.
    await_completed_contract_execute({
        abi: Optional[Abi];
        transaction_hash: string;
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };

    query_contract({
        abi: Optional[Abi];
        contract: IAddress;
        caller?: IAddress;
        value?: Amount;
        function: string;
        arguments: List[object];
    }): List[any];

    // Function of the relayed transactions factory, promoted to the facade.
    // Handles relayed V3 transactions.
    create_relayed_transaction({
        relayer: Account;
        nonce: Optional[int];
        inner_transactions: List[ITransaction];
    }): Transaction;

    //
    create_transaction_for_issuing_token({
        // See questions (3) and (4)
    });
    //

    // Access to the underlying components:
    get_network_provider(): INetworkProvider;
    get_transaction_computer(): TransactionComputer;
    get_token_computer(): TokenComputer;
    get_address_computer(): AddressComputer;

    get_account_transactions_factory(): AccountTransactionsFactory;
    get_delegation_transactions_factory(): DelegationTransactionsFactory;
    get_token_management_transactions_factory(): TokenManagementTransactionsFactory;
    get_transfer_transactions_factory(): TransferTransactionsFactory;
    create_smart_contract_transactions_factory(abi: Optional[Abi]): SmartContractTransactionsFactory;
```

## Examples

### Transfer native tokens

```
entrypoint = MainnetEntrypoint();
sender = entrypoint.load_account_from_pem("alice.pem");
receiver = Address.from_bech32("erd1spyavw0956vq68xj8y4tenjpq2wd5a9p2c6j8gsz7ztyrnpxrruqzu66jx");

account.nonce = entrypoint.recall_account_nonce(account.address);

transaction = entrypoint.create_transaction_for_transfer({
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    receiver: receiver,
    native_amount: 1000000000000000000,
});

transaction_hash = entrypoint.send_transaction(transaction);
transaction_on_network = entrypoint.await_completed_transaction(transaction_hash);
```

### Deploy a contract

```
entrypoint = MainnetEntrypoint();
sender = entrypoint.load_account_from_pem("alice.pem");
abi = Abi.load("adder.abi.json");

account.nonce = entrypoint.recall_account_nonce(account.address);

transaction = entrypoint.create_transaction_for_deploy({
    abi: abi,
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    bytecode: bytecode,
    arguments: [arg1, arg2],
    gas_limit: 10000000
});

transaction_hash = entrypoint.send_transaction(transaction);
parsed_outcome = entrypoint.await_completed_contract_deploy(abi, transaction_hash);
```

### Access particular transaction factories

```
entrypoint = MainnetEntrypoint();

factory = entrypoint.get_delegation_transactions_factory()
factory = entrypoint.get_token_management_transactions_factory()
```
