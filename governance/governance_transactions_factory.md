## GovernanceTransactionsFactory

```
class GovernanceTransactionsFactory:
    // the constructor can look like this
    constructor(config: TransactionsFactoryConfig);

    create_transaction_for_new_proposal(
        sender: Address,
        commit_hash: string,
        start_vote_epoch: int,
        end_vote_epoch: int,
        native_token_amount: Amount,
    ): Transaction;

    create_transaction_for_vote(
        sender: Address,
        proposal_nonce: int,
        vote: VoteType,
    ): Transaction;

    create_transaction_for_closing_proposal(
        sender: Address,
        proposal_nonce: int,
    ): Transaction;

    create_transaction_for_clearing_ended_proposals(
        sender: Address,
        proposers: Address[],
    ): Transaction;

    create_transaction_for_claiming_accumulated_fees(sender: Address): Transaction;

    create_transaction_for_claiming_accumulated_fees(
        sender: Address,
        proposal_fee: int,
        lost_proposal_fee: int,
        min_quorum: int,
        min_veto_threshold: int,
        min_pass_threshold: int,
    ): Transaction;
```
