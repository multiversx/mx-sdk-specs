## TransactionComputer

This is an utility class created to work together with the `Transaction` class.

```
class TransactionComputer:
    compute_transaction_fee(transaction: Transaction, network_config: INetworkConfig): Amount;

    // this method should take care of "regular" transaction signing.
    // should also validate if the some of the transaction fields are set (sender, receiver, gasLimit); throws error otherwise
    // should ensure that if `options` is set, also `version` >= 2; throws error otherwise
    // if `ignore_options == False`, the method computes the bytes for signing based on the `version` and `options` of the transaction
    // if the least significant bit of the `options` is set, will serialize transaction for hash signing
    // if `ignore_options == True`, the transaction is simply serialized
    compute_bytes_for_signing(self, transaction: Transaction, ignore_options=False) -> bytes:

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
        guardian: Address;
    );

    // sets the least significant bit of the `options` field; also ensures that `version` >= 2
    apply_options_for_hash_signing(transaction: Transaction);

    // should return true if transaction.relayer is set; returns false otherwise;
    is_relayed_v3_transaction(transaction: Transaction): boolean;
```
