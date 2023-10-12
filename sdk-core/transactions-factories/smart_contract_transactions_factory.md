## SmartContractTransactionsFactory

```
class SmartContractTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte" etc.
    // The constructor may also be parametrized with a Codec instance, necessary to encode contract call arguments.

    create_transaction_for_deploy({
        sender: Address;
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
        gasLimit: uint32;
    }): Transaction;

    create_transaction_for_execute({
        sender: Address;
        contract: Address;
        // If "function" is a reserved word in the implementing language, it should be replaced with a different name (e.g. "func" or "functionName").
        function: string;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        token_transfers: List[TokenTransfer] = [];
        gasLimit: uint32;
    }): Transaction;

    create_transaction_for_upgrade({
        sender: Address;
        contract: Address;
        bytecode: bytes OR bytecodePath: Path;
        arguments: List[object] = [];
        native_transfer_amount: Amount = 0;
        isUpgradeable: bool = True;
        isReadable: bool = True;
        isPayable: bool = False;
        isPayableBySC: bool = True;
        gasLimit: uint32;
    }): Transaction;

    ...

```
