## MultisigTransactionFactory

```
class MultisigTransactionsFactory:
    // the constructor can look like this
    constructor(config: TransactionsFactoryConfig, abi: Optional[Abi] = None);

    // creates a transaction to deploy the multisig contract
    create_transaction_for_deploy(sender: Address,
        bytecode: Path | bytes,
        quorum: int,
        board: list[Address],
        gas_limit: int,
        is_upgradeable: bool = True,
        is_readable: bool = True,
        is_payable: bool = False,
        is_payable_by_sc: bool = True,
    ): Transaction;

    create_transaction_for_deposit(
        sender: Address,
        contract: Address,
        gas_limit: int,
        native_token_amount: Optional[int],
        token_transfers: Optional[list[TokenTransfer]],
    ): Transaction;

    create_transaction_for_discard_batch(
        sender: Address,
        contract: Address,
        action_ids: list[int],
        gas_limit: int,
    ): Transaction;

    // implements all the methods of the multisig contract
```
