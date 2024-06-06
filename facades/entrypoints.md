## Account

```
class Account:
    address: IAddress;
    nonce: int;

    sign(data: bytes): bytes;
    get_nonce_then_increment(): int;
```

## Pre-defined entrypoints

Pre-defined entrypoints inherit from `NetworkEntrypoint` and use sensible default values.

```
class MainnetEntrypoint extends NetworkEntrypoint(ApiNetworkProvider("api.multiversx.com"), "1");

class DevnetEntrypoint extends NetworkEntrypoint(ApiNetworkProvider("devnet-api.multiversx.com"), "D");

class TestnetEntrypoint extends NetworkEntrypoint(ApiNetworkProvider("testnet-api.multiversx.com"), "T");
```

## NetworkEntrypoint

The `NetworkEntrypoint` acts as a facade for interacting with the network. It should cover the most common use-cases "out of the box".

**(a)** All functions that create transactions receive as first parameter the sender Account.

**(b)** All functions that create transactions receive a nonce, which is optional. If not provided, the nonce is fetched from the network.

**(c)** All functions that create transactions return already-signed transactions.

**(d)** All functions that create smart-contract transactions receive an optional ABI as a parameter.

**(e)** All functions that parse the outcome of a transaction receive an optional ABI as a parameter.

**(f)** The facade should allow one to access the underlying, more lower-level components (e.g. transaction factories, network provider).

```
class NetworkEntrypoint:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, it can be parametrized with:
    // - a network provider URL and kind (proxy, API)
    // - a chain ID

    // TBD
    load_account(path: Path, password: Optional[string], address_index: Optional[int]): Account;


    verify_transaction_signature(transaction: Transaction): bool;
    verify_message_signature(message: Message): bool;

    recall_account_nonce(address: IAddress);

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
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
        gasLimit: uint32;
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
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
        gasLimit: uint32;
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
        gasLimit: uint32;
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
        block_nonce?: int;
    }): List[any];

    // Below are the most useful functions of the transactions factories, promoted to the facade.

    create_relayed_transaction({
        relayer: Account;
        nonce: Optional[int];
        inner_transactions: List[ITransaction];
    }): Transaction;

    create_transaction_for_issuing_token({
        TBD or more specific functions, as in the factory.
    });

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

### Deploying a contract

```
entrypoint = MainnetEntrypoint(...);
sender = Account(...);
abi = Abi(...);

transaction = entrypoint.create_transaction_for_deploy({
    abi: abi,
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    bytecode: bytecode,
    arguments: [arg1, arg2],
    gasLimit: 1000000
});

transaction_hash = entrypoint.send_transaction(transaction);
parsed_outcome = entrypoint.await_completed_contract_deploy(abi, transaction_hash);
```

### Accessing particular transaction factories

```
entrypoint = MainnetEntrypoint(...);
sender = Account(...);

entrypoint.get_token_management_transactions_factory()
OR
entrypoint.transaction_factories.token_management
```
