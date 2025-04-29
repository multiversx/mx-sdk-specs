## Transactions Parser for Multisig Transactions

```
class MultisigTransactionsOutcomeParser:
    // constructor can look like this
    constructor(abi: Optional[Abi] = None);

    // returns the address of the deployed multisig contract
    parse_deploy(transaction_on_network: TransactionOnNetwork): {
        return_code: str
        return_message: str
        contracts: {
            address: Address
            owner_address: Address
            code_hash: bytes
        }[]
    };

    // returns the id of a proposal
    parse_execute_propose_any(transaction_on_network: TransactionOnNetwork): int;

    // returns the output of a performAction transaction
    parse_execute_perform(transaction_on_network: TransactionOnNetwork): Optional[Address];
```
