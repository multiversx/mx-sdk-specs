## TransactionComputer

This is an utility class created to work together with the `Transaction` class.

```
class TransactionComputer:
    compute_transaction_fee(transaction: Transaction, network_config: INetworkConfig): Amount;

    // this method should take care of "regular" transaction signing.
    // should also validate if the some of the transaction fields are set (sender, receiver, gasLimit); throws error otherwise
    // should ensure that if `options` is set, also `version` >= 2; throws error otherwise 
    compute_bytes_for_signing(transaction: Transaction): bytes;

    // serializes the transaction then computes the hash; used for hash signing transactions.
    // should also validate if the some of the transaction fields are set (sender, receiver, gasLimit); throws error otherwise
    // should ensure that if `options` is set, also `version` >= 2; throws error otherwise
    compute_hash_for_signing(transaction: Transaction): bytes;

    // should compute bytes for verifying the transaction signature
    compute_bytes_for_verifying(transaction: Transaction): bytes;

    compute_transaction_hash(transaction: Transaction): bytes;

    // returns True if the second least significant bit is set; returns False otherwise
    has_options_set_for_guarded_transaction(transaction: Transaction): bool;

    // returns True if the least significant bit is set; returns False otherwise; should also have transaction.version >= 2
    has_options_set_for_hash_signing(transaction: Transacion): bool;

    // sets guardian address, transaction.version = 2, sets transaction.options second least significant bit
    apply_guardian(
        transaction: Transaction;
        guardian: string; // bech32-encoded
    );

    // sets the least significant bit of the `options` field; also ensures that `version` >= 2 
    apply_options_for_hash_signing(transaction: Transaction);

    // should return true if transaction.innerTransactions.length > 0; returns false otherwise; 
    is_relayed_v3_transaction(transaction: Transaction): boolean;

    // should return all innerTransations; returns empty array if there are no transactions
    get_inner_transactions(transaction: Transaction): List[Transaction];
```
