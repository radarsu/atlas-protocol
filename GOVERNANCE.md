# Atlas - Coordination of Meaning

Atlas does not enforce global governance. Instead, it provides an opt-in coordination of meaning and interpretation through explicit, revocable configuration.

Nodes may choose `semanticAnchorKey` to coordinate parameters that enable shared meaning. This is similar to governance of interoperable data structures by Schema.org.

It's important to understand, that semantic anchor is not an enforcement authority. Enforcement (binary accept/reject) is defined only by protocol logic and local node configuration. There is no escalation path from semantics to enforcement.

## Core Principles

- **Local Sovereignty** - Each node decides which interpretations it adopts. No policy is mandatory.
- **Explicit Authority** - Any influence over shared interpretation is named, visible, and configurable.
- **Cheap Exit** - Nodes can change or remove semantic anchors at any time without penalty.
- **Bounded Scope** - Semantic affects interpretation and weighting, never protocol validity.
- **Forkability Over Enforcement** - Disagreement results in divergence, not coercion.
- **Transparency Over Neutrality Claims** - All coordination of meaning is acknowledged as social and contextual.
- **Scope Enforcement (Anti-Authority-Drift)** - Authority drift is prevented by hard technical boundaries, not convention.
- **Forbidden Scopes (Non-Negotiable)** - Semantic authorities can never affect aspects, which would break or cause unequal treatment of other Nodes.

## Mechanism
Atlas provides **opt-in coordination of meaning**, not enforcement. Nodes may choose a `semanticAnchorKey` to adopt shared interpretations and share understanding via **DigitalDocuments**. This choice is explicit, revocable, and local.

As a bootstrap point, the semantic anchor publishes in network **two initial DigitalDocuments**:

## Digital Documents

### Contribution Points
Defines how contribution points are earned, decay, and are interpreted for shared local resource management.  
Contribution points have **local meaning only**: each node interprets them independently based on the evidence it observes and is not required to know or agree on any global balance or history.

### Voting
Defines how votes are cast, counted, and interpreted.  
Votes have **local meaning only**: each node evaluates votes based on the signed ballots it observes and is not required to see all votes or agree on a global tally.

All later DigitalDocuments are adopted only if they pass the defined vote. Nodes remain free to adopt, ignore, or fork at any time.

## Complexity
As number of digital documents grows, the complexity of some nodes will grow. That is unavoidable. But the real question is if nodes can safely ignore most of it. That will keep complexity contained and entry barrier to the network - low.

In any growing distributed system, complexity appears in one of three places:
- In the protocol
- In clients / nodes
- In social processes and off-chain norms

Atlas deliberately pushes complexity into optional documents instead of hiding it in protocol rules or client defaults for transparency.