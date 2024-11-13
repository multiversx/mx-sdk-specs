## SmartContractsController

Promotes all the functionality of `SmartContractTransactionsFactory`, `SmartContractTransactionsOutcomeParser` and `SmartContractQueriesController`.

```
class SmartContractsController:
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
            address: Address;
            ownerAddress: Address;
            codeHash: bytes;
        }];
    };

    // Does "await_completed_transaction" and "parse_deploy" in one go.
    await_completed_deploy(transaction_hash: string): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: Address;
            ownerAddress: Address;
            codeHash: bytes;
        }];
    };

    create_transaction_for_upgrade({
        sender: IAccount;
        nonce: Optional[int];
        contract: Address;
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
            address: Address;
            codeHash: bytes;
        }];
    };

    // Specialized function to await and parse a contract upgrade transaction.
    // Does "await_completed_transaction" and "parse_upgrade" in one go.
    await_completed_upgrade(transaction_hash: string): {
        return_code: string;
        return_message: string;
        contracts: List[{
            address: Address;
            codeHash: bytes;
        }];
    };

    create_transaction_for_execute({
        sender: IAccount;
        nonce: Optional[int];
        contract: Address;
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

    // It does "create_query", "run_query" and "parse_query_response" in one go.
    // Can throw:
    // - ErrSmartContractQuery (e.g. when return code is not "ok")
    query({
        contract: Address;
        caller?: Address;
        value?: Amount;
        function: string;
        arguments: List[object];
    }): List[any];

    // If `Abi` or `Codec` is available, this function encodes the input arguments accordingly.
    create_query({
        contract: string;
        caller?: string;
        value?: Amount;
        function: string;
        arguments: List[any];
    }): SmartContractQuery;

    // Runs the query against the network and returns the raw (encoded) response.
    run_query(query: SmartContractQuery): SmartContractQueryResponse;
```
