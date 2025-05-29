## GovernanceTransactionsOutcomeParser

```
class GovernanceTransactionOutcomeParser:
    // constructor is not captured by the specs, but can look like this:
    constructor(address_hrp?: string);

    parse_new_proposal(transaction: TransactionOnNetwork): NewProposalOutcome[];

    parse_vote(transaction: TransactionOnNetwork): VoteOutcome[];

    parse_delegate_vote(transaction: TransactionOnNetwork): DelegateVoteOutcome[];

    parse_close_proposal(transaction: TransactionOnNetwork): CloseProposalOutcome[];
```
