## Transaction

```
dto Transaction:
    sender: string;
    receiver: string;
    gasLimit: uint32;
    chainID: string;

    nonce?: uint64;
    value?: Amount;
    senderUsername?: string;
    receiverUsername?: string;
    gasPrice?: uint32;

    data?: bytes;
    version?: uint32;
    options?: uint32;
    guardian?: string;

    signature: bytes;
    guardianSignature?: bytes;

    // Optional named constructor, if and only if the implementing library defines a `DraftTransaction`.
    new_from_draft(draft: DraftTransaction): Transaction;
```

## DraftTransaction

Optionally, if desired, the implementing library can also define an incomplete representation of the transaction, to be used as return type for the **transaction factories**. See [README](../README.md), instead of the `Transaction` type.

```
dto DraftTransaction:
    sender: string;
    receiver: string;
    value?: string;
    data?: bytes;
    gasLimit: uint32;
    chainID: string; // The chain ID from the factory's config (received in the constructor) should be used
```

## TransactionComputer

```
class TransactionComputer:
    compute_transaction_fee(transaction: Transaction, network_config: INetworkConfig): Amount;

    // this method should take care of both "regular" transaction signing and hash signing.
    // if `signByHash` is false should serialize the transaction the regular way, otherwise it should compute its hash
    // should also validate if the some of the transaction fields are set (sender, receiver, gasLimit); throws error otherwise
    // should ensure that if `options` is set, also `version` >= 2; throws error otherwise 
    compute_bytes_for_signing(transaction: Transaction, signByHash?: bool): bytes;

    // should compute bytes for verifying the transaction signature
    // if `isSignedByHash` is false, should serialize the transaction the regular way, otherwise it should compute its hash
    compute_bytes_for_verifying(transaction: Transaction, isSignedByHash?: bool): bytes;

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
```
