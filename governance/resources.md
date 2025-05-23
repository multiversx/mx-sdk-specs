## Governance Resources

```
enum VoteType:
    YES = "yes"
    NO = "no"
    VETO = "veto"
    ABSTAIN = "abstain"
```

The bellow classes can be implemented as classes or types(JS). e.g in Python, they are dataclasses.

Depending on the language these are implemented in, some properties that are of type `int` might need to be changed to type that support larger values.

```
class GovernanceConfig:
    proposal_fee: int
    min_quorum: float
    min_pass_threshold: float
    min_veto_threshold: float
    last_proposal_nonce: int
```


```
class DelegatedVoteInfo:
    used_stake: int
    used_power: int
    total_stake: int
    total_power: int
```

```
class ProposalInfo:
    cost: int
    commit_hash: string
    nonce: int
    issuer: Address
    start_vote_epoch: int
    end_vote_epoch: int
    quorum_stake: int
    num_yes_votes: int
    num_no_votes: int
    num_veto_votes: int
    num_abstain_votes: int
    is_closed: bool
    is_passed: bool
```

```
class NewProposalOutcome:
    proposal_nonce: int
    commit_hash: string
    start_vote_epoch: int
    end_vote_epoch: int
```

```
class VoteOutcome:
    proposal_nonce: int
    vote: str
    total_stake: int
    total_voting_power: int
```

```
class DelegateVoteOutcome:
    proposal_nonce: int
    vote: string
    voter: Address
    user_stake: int
    voting_power: int
```

```
class CloseProposalOutcome:
    commit_hash: string
    passed: bool
```
