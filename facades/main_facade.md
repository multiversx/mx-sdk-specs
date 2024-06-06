TODO: find a name (think of space / astronomy stuff).
TODO: have separate classes for most important networks: MainnetEntrypoint, DevnetEntrypoint, TestnetEntrypoint

## Account

```
class Account:
    address: IAddress;
    nonce: int;

    sign(data: bytes): bytes;
    get_nonce_then_increment(): int;
```

## MainFacade

```
// All functions that create transactions receive a sender Account.
// They also receive a nonce, which is optional. If not provided, the nonce is fetched from the network.
// Furthermore, all functions that create transactions return already-signed transactions.
class MainFacade:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, it can be parametrized with:
    // - a network provider URL and kind (proxy, API)
    // - a chain ID

    // TBD
    load_account(path: Path, password: Optional[string], address_index: Optional[int]): Account;


    verify_transaction_signature(transaction: Transaction): bool;
    verify_message_signature(message: Message): bool;

    recall_account_nonce(address: IAddress);

    send_transaction(transaction: Transaction);

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

    // Below are functions of the network providers, promoted to the facade.
    // get_account()
    // TBD

    // Below are the most useful functions of the transactions factories, promoted to the facade.

    create_relayed_transaction({
        relayer: Account;
        nonce: Optional[int];
        inner_transactions: List[ITransaction];
    }): Transaction;

    create_transaction_for_issuing_token({
        TBD or more specific functions, as in the factory.
    });

```

## Examples

### Deploying a contract

```
facade = MainFacade(...);
sender = Account(...);
abi = Abi(...);

transaction = facade.create_transaction_for_deploy({
    abi: abi,
    sender: sender,
    nonce: sender.get_nonce_then_increment(),
    bytecode: bytecode,
    arguments: [arg1, arg2],
    gasLimit: 1000000
});

transaction_hash = facade.send_transaction(transaction);
parsed_outcome = facade.await_completed_contract_deploy(abi, transaction_hash);
```

### Accessing particular transaction factories

```
facade = MainFacade(...);
sender = Account(...);

facade.get_token_management_transactions_factory()
OR
facade.transaction_factories.token_management
```
