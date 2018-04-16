- Feature Name: consensus_update
- Start Date: 2018-04-16
- RFC PR: (leave this empty)
- Sawtooth Issue: (leave this empty)

# Summary
[summary]: #summary

This RFC describes the changes to the architecture of the Sawtooth Validator
that are required to support the new Consensus Engine API. This RFC references
ideas which are more fully described in the related RFCs on the Block Manager,
Sub-state Validation, and the Consensus Engine API.

# Motivation
[motivation]: #motivation

Today, the chain controller and block publisher are at the center of the
architecture and drive most of the activity within the validator.

When new blocks are completed, they are passed to the chain controller which
immediately begins validating them. These blocks cannot be passed on to
consensus because their state deltas must be computed before fork resolution
can be performed correctly.

The block publisher constantly polls the consensus and the chain controller to
determine if it can publish a block. Most of the time the answer is yes, since
neither the chain controller nor consensus have enough information about the
state of the system to determine whether it is really a good time to publish.
The chain controller is not in a position to make consensus decisions about
publishing and the consensus module is not given enough information about the
state of the validator to determine if publishing blocks makes sense given
other priorities (Eg., Am I many blocks behind? Am I already considering many
blocks at this height?)

Finally, the existing consensus module interface is not compatible with voting
based consensus algorithms.

The changes to the architecture of the validator described in this RFC remove
the existing consensus module interface and move control out of the chain
controller and block publisher and into a new component called the "consensus
engine". The consensus engine is given enough information to perform fork
resolution prior to full, expensive block validation and to make intelligent
decisions about when to publish blocks based on the state of the validator and
its view of the network.

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

The consensus engine is responsible for driving the validator and it is
provided with a set of notifications and services by the rest of the validator
as described in the Consensus Engine API RFC. The chain controller, block
publisher, and networking layer are responsible for implementing these
notifications and services.

At a high-level, these components operate roughly as follows:

## Consensus Engine

The consensus engine listens for notifications from the validator about the
status of blocks and messages from peers. It must also determine internally
when to build and publish blocks based on its view of the network and the
consensus algorithm it implements. Often this will be some sort of timer based
decision.

Based on the notifications the engine receives and the specifics of the
algorithm being implemented, the engine utilizes the services provided by the
validator to create new blocks, communicate with its peers, request that
certain blocks be committed, and fail or ignore blocks that should not be
committed.

While the validator may take actions beyond what the engine instructs it to do
for performance optimization reasons, it is the consensus engines
responsibility to drive the progress of the validator and ensure liveness.

It is not the engine's responsibility to manage blocks or memory, other than to
ensure it responds to every new block with a commit, fail, or ignore within a
"reasonable amount of time". The validator is responsible for guaranteeing the
integrity of all blocks sent to the engine until the engine responds. After the
engine responds, the validator does not guarantee that the block and its
predecessors continue to be available unless the block was committed.

Finally, as an optimization, the consensus engine can sends prioritized lists
of blocks to the chain controller for checking instead of sending them one at a
time, which allows the chain controller to intelligently work ahead while the
consensus engine makes its decisions.

## Chain Controller

The chain controller listens for new blocks coming from the completer and for
new requests coming from the consensus engine.

When the chain controller receives a new block from the completer, it performs
an initial check of any consensus transactions contained in the block and
updates the consensus sub-state used by the consensus engine. If this check
succeeds, it notifies the consensus engine of the new block and any
predecessors of the block that haven't been committed and ensures it is not
dropped until the consensus engine responds (using the block manager and
reference counting). The chain controller may also hold onto some number of
recently used forks for a period of time for performance reasons, but this
cannot be requirement for correct operation.

The chain controller provides services to the consensus engine around
committing blocks. Specifically, it is responsible for checking whether blocks
can be committed, failing and purging blocks that the consensus engine
determines are invalid along with any descendants, committing blocks, and
storing and retrieving historical consensus payloads for the engine.

The chain controller is also responsible for notifying the consensus engine
about blocks it receives, blocks it determines can or cannot be committed, and
blocks that have been committed.

Finally, as an optimization, depending on available resources the chain
controller may optimistically validate blocks before the consensus engine
requests it to do so.

## Block Publisher

The block publisher listens for new batches and for new requests from the
consensus engine.

The block publisher provides services to the consensus engine around building
blocks. On its own, it does nothing until asked except keep track of
uncommitted batches. It supports building one block at a time and any requests
from the consensus engine that would violate this return an error. The
consensus engine can cancel or finalize an in progress block at any time.

The validator may be configured in a way that limits the "size" of blocks. If
this size limit is reached while building a block and the consensus engine
hasn't cancelled or finalized the block, the publisher will stop working and
wait for a request from the engine.

When the block publisher finishes building a new block, it is published using
the existing model.

## Networking Layer

The networking layer listens for new blocks and consensus messages from peers
and routes new consensus messages from the consensus engine. When a new block
is received, its signature and structure are verified and then it is passed to
the completer (which passes it on to the chain controller when the block is
complete). When a new consensus message is received, it is forwarded to the
consensus engine.

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

This is the technical portion of the RFC. Explain the design in sufficient
detail that:

- Its interaction with other features is clear.
- It is reasonably clear how the feature would be implemented.
- Corner cases are dissected by example.

The section should return to the examples given in the previous section, and
explain more fully how the detailed proposal makes those examples work.

# Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

# Rationale and alternatives
[alternatives]: #alternatives

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not
  choosing them?
- What is the impact of not doing this?

# Prior art
[prior-art]: #prior-art

Discuss prior art, both the good and the bad, in relation to this proposal.
A few examples of what this can include are:

- For consensus, global state, transaction processors, and smart contracts
  implementation proposals: Does this feature exists in other distributed
  ledgers and what experience have their communities had?
- For community proposals: Is this done by some other community and what were
  their experiences with it?
- For other teams: What lessons can we learn from what other communities have
  done here?
- Papers: Are there any published papers or great posts that discuss this? If
  you have some relevant papers to refer to, this can serve as a more detailed
  theoretical background.

This section is intended to encourage you as an author to think about the
lessons from other distributed ledgers, provide readers of your RFC with
a fuller picture.  If there is no prior art, that is fine - your ideas are
interesting to us whether they are brand new or if it is an adaptation.

Note that while precedent set by other distributed ledgers is some motivation,
it does not on its own motivate an RFC.  Please also take into consideration
that Sawtooth sometimes intentionally diverges from common distributed
ledger/blockchain features.

# Unresolved questions
[unresolved]: #unresolved-questions

- What parts of the design do you expect to resolve through the RFC process
  before this gets merged?
- What parts of the design do you expect to resolve through the implementation
  of this feature before stabilization?
- What related issues do you consider out of scope for this RFC that could be
  addressed in the future independently of the solution that comes out of this
  RFC?
