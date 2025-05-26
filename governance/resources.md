## Governance Resources

```
enum VoteType:
    YES = "yes"
    NO = "no"
    VETO = "veto"
    ABSTAIN = "abstain"
```

The classes below can be implemented as either classes or types (in JavaScript). For example, in Python, they are implemented as dataclasses.

```
class GovernanceConfig:
    proposal_fee: uint64
    min_quorum: float
    min_pass_threshold: float
    min_veto_threshold: float
    last_proposal_nonce: uint32
```


```
class DelegatedVoteInfo:
    used_stake: uint64
    used_power: uint64
    total_stake: uint64
    total_power: uint64
```

```
class ProposalInfo:
    cost: uint64
    commit_hash: string
    nonce: uint32
    issuer: Address
    start_vote_epoch: uint32
    end_vote_epoch: uint32
    quorum_stake: uint64
    num_yes_votes: uint64
    num_no_votes: uint64
    num_veto_votes: uint64
    num_abstain_votes: uint64
    is_closed: bool
    is_passed: bool
```

```
class NewProposalOutcome:
    proposal_nonce: uint32
    commit_hash: string
    start_vote_epoch: uint32
    end_vote_epoch: uint32
```

```
class VoteOutcome:
    proposal_nonce: uint32
    vote: str
    total_stake: uint64
    total_voting_power: uint64
```

```
class DelegateVoteOutcome:
    proposal_nonce: uint32
    vote: string
    voter: Address
    user_stake: uint64
    voting_power: uint64
```

```
class CloseProposalOutcome:
    commit_hash: string
    passed: bool
```
