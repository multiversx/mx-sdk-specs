## SmartContractController

Promotes all the functionality of `SmartContractTransactionsFactory`, `SmartContractTransactionsOutcomeParser` and `SmartContractQueriesController`.

```
class SmartContractController:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Suggestions:
    constructor(network_provider: INetworkProvider, abi: Optional[Abi]);

    create_transaction_for_deploy({
        sender: IAccount;
        nonce: Optional[int];
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        is_upgradeable: bool = True;
        is_readable: bool = True;
        is_payable: bool = False;
        is_payable_by_contract: bool = True;
        gas_limit: uint32;
    }): Transaction;

    parse_deploy(transaction_on_network: TransactionOnNetwork): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            ownerAddress: string;
            codeHash: bytes;
        }];
    };

    // Does "await_completed_transaction" and "parse_deploy" in one go.
    await_completed_deploy(transaction_hash: string): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            ownerAddress: string;
            codeHash: bytes;
        }];
    };

    create_transaction_for_upgrade({
        sender: IAccount;
        nonce: Optional[int];
        contract: IAddress;
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        is_upgradeable: bool = True;
        is_readable: bool = True;
        is_payable: bool = False;
        is_payable_by_contract: bool = True;
        gas_limit: uint32;
    }): Transaction;

    // This method is less important (supports an exotic flow).
    parse_upgrade(transaction_on_network: TransactionOnNetwork): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            codeHash: bytes;
        }];
    };

    // Specialized function to await and parse a contract upgrade transaction.
    // Does "await_completed_transaction" and "parse_upgrade" in one go.
    await_completed_upgrade(transaction_hash: string): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: string;
            codeHash: bytes;
        }];
    };

    create_transaction_for_execute({
        sender: IAccount;
        nonce: Optional[int];
        contract: IAddress;
        // If "function" is a reserved word in the implementing language, it should be replaced with a different name (e.g. "func" or "functionName").
        function: string;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        token_transfers: List[TokenTransfer] = [];
        gas_limit: uint32;
    }): Transaction;

    parse_execute({
        transaction_on_network: TransactionOnNetwork,
        function?: string
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };

    // Does "await_completed_transaction" and "parse_execute" in one go.
    await_completed_execute({
        transaction_hash: string;
    }): {
        values: List[any];
        return_code: string;
        return_message: string;
    };

    query_contract({
        contract: IAddress;
        caller?: IAddress;
        value?: Amount;
        function: string;
        arguments: List[object];
    }): List[any];
```
