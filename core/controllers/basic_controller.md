## BasicController

```
class BasicController:
    // Verifies the signature of a transaction.
    verify_transaction_signature(transaction: Transaction): bool;

    // Verifies the signature of a message.
    verify_message_signature(message: Message): bool;

    // Fetches the account nonce from the network.
    recall_account_nonce(address: IAddress): uint64;

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
```
