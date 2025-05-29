## GovernanceController

```
class GovernanceController:
    // constructor is not captured by the specs, but can look like this:
    constructor(chain_id: str, network_provider: NetworkProvider, address_hrp?: string);

    create_transaction_for_new_proposal(
        sender: IAccount,
        nonce: int,
        commit_hash: string,
        start_vote_epoch: int,
        end_vote_epoch: int,
        native_token_amount: int,
        gas_limit?: int,
        gas_price?: int,
        guardian?: Address,
        relayer?: Address,
    ) -> Transaction;

    parse_new_proposal(transaction: TransactionOnNetwork) -> NewProposalOutcome[];

    await_completed_new_proposal(transaction_hash: string | bytes) -> NewProposalOutcome[];

    create_transaction_for_voting(
        sender: IAccount,
        nonce: int,
        proposal_nonce: int,
        vote: VoteType,
        gas_limit?: int,
        gas_price?: int,
        guardian?: Address,
        relayer?: Address,
    ) -> Transaction;

    parse_vote(transaction: TransactionOnNetwork) -> VoteOutcome[];

    await_completed_vote(transaction_hash: string | bytes) -> VoteOutcome[];

    create_transaction_for_closing_proposal(
        sender: IAccount,
        nonce: int,
        proposal_nonce: int,
        gas_limit?: int,
        gas_price?: int,
        guardian?: Address,
        relayer?: Address,
    ) -> Transaction;

    parse_close_proposal(transaction: TransactionOnNetwork) -> CloseProposalOutcome[];

    await_completed_close_proposal(transaction_hash: string | bytes) -> CloseProposalOutcome[];

    create_transaction_for_clearing_ended_proposals(
        sender: IAccount,
        nonce: int,
        proposers: Address[],
        gas_limit?: int,
        gas_price?: int,
        guardian?: Address,
        relayer?: Address,
    ) -> Transaction;

    create_transaction_for_claiming_accumulated_fees(
        sender: IAccount,
        nonce: int,
        gas_limit?: int,
        gas_price?: int,
        guardian?: Address,
        relayer?: Address,
    ) -> Transaction;

    create_transaction_for_changing_config(
        sender: IAccount,
        nonce: int,
        proposal_fee: int,
        lost_proposal_fee: int,
        min_quorum: int,
        min_veto_threshold: int,
        min_pass_threshold: int,
        gas_limit?: int,
        gas_price?: int,
        guardian?: Address,
        relayer?: Address,
    ) -> Transaction;

    get_voting_power(address: Address): int;

    get_config(): GovernanceConfig;

    get_proposal(proposal_nonce): ProposalInfo;

    get_delegated_vote_info(contract: Address, user: Address): DelegatedVoteInfo;
```
