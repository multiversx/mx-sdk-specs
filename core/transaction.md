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
    // should serialize the transaction after checking the version and the options fields.
    // if version >= 2 and the least significant bit of the options field is set it should serialize the transaction and compute its hash
    // if the least significant bit is not set should simply serialize the transaction the "regular" way
    // should also validate if the some of the transaction fields are set (sender, receiver, gasLimit, chainId); throws error otherwise
    compute_bytes_for_signing(transaction: Transaction): bytes;

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
```
