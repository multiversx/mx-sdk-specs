## MainFacade

```
class MainFacade:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // For example, it can be parametrized with:
    // - a network provider URL and kind (proxy, API)
    // - a chain ID

    register_secret_key(secret_key: ISecretKey);
    register_wallet(wallet: Any);

    sign_transaction(transaction: Transaction);
    sign_message(message: Message): Signature;
    verify_transaction_signature(transaction: Transaction): bool;
    verify_message_signature(message: Message): bool;

    recall_account_nonce(address: IAddress);

    // Account is looked up by "transaction.sender".
    apply_nonce_on_transaction(transaction: Transaction);

    send_transaction(transaction: Transaction);
    await_completed_transaction(transaction_hash: string): TransactionOutcome;

    create_transaction_for_native_token_transfer({
        sender: IAddress;
        receiver: IAddress;
        native_amount: Amount;
        data: Optional[bytes];
    }): Transaction;

    create_transaction_for_esdt_token_transfer({
        sender: IAddress;
        receiver: IAddress;
        token_transfers: TokenTransfer[];
    }): Transaction;

    // ABI is registered by name.
    // When needed (in the contract-related utility functions), ABI is looked up by name.
    register_abi({
        contract: IAddress;
        abi: Abi;
    }): void;

    create_transaction_for_deploy({
        contract_name: string;
        sender: IAddress;
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
        gasLimit: uint32;
    }): Transaction;

    parse_contract_deploy({
        contract_name: string;
        transaction_outcome: TransactionOutcome
    }): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            ownerAddress: string;
            codeHash: bytes;
        }];
    };

    // Does "await_completed_transaction" and "parse_contract_deploy" in one go.
    await_completed_contract_deploy({
        contract_name: string;
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

    create_transaction_for_contract_execute({
        contract_name: string;
        sender: IAddress;
        contract: IAddress;
        // If "function" is a reserved word in the implementing language, it should be replaced with a different name (e.g. "func" or "functionName").
        function: string;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        token_transfers: List[TokenTransfer] = [];
        gasLimit: uint32;
    }): Transaction;

    parse_contract_execute({
        contract_name: string;
        transaction_outcome: TransactionOutcome,
        function?: string
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };

    // Does "await_completed_transaction" and "parse_contract_execute" in one go.
    await_completed_contract_execute({
        contract_name: string;
        transaction_hash: string;
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };

    query_contract({
        contract_name: string;
        contract: IAddress;
        caller?: IAddress;
        value?: Amount;
        function: string;
        arguments: List[object];
        block_nonce?: int;
    }): List[any];
```
