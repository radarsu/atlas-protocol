# Atlas – Coordination of Meaning

Atlas separates governance into **two layers** that both define shared meaning, but with different scope and change cost.

Atlas does not enforce global governance. It provides **opt-in coordination of meaning and interpretation** through explicit, revocable configuration at the document layer.

## Two Layers of Governance

### Protocol-Level Governance (Hard Layer)

Protocol-level governance is embedded in code and shared by all compatible nodes. It is slow to change and expensive to exit. It defines non-negotiable invariants such as cryptographic primitives or protocol validity rules.

This layer enforces what *cannot* be changed.

### Document-Level Governance (Soft Layer)

Document-level governance is expressed through signed **DigitalDocuments** and is explicitly adopted by nodes. It is cheap to change and cheap to exit.

It defines dynamic interpretation such as:
- shared convention-based systems
- recommended parameters tuning
- coordination norms

This layer governs **interpretation**, not **enforcement**.

## Semantic Anchors

Nodes may choose a `semanticAnchorKey` to coordinate on document-level governance. A semantic anchor publishes DigitalDocuments, but has no enforcement authority. Enforcement (binary accept/reject) is defined only by protocol logic and local node configuration. There is no escalation path from semantics to enforcement.

## Core Principles

- **Explicit Authority** – Influence over meaning is named, visible, and configurable.
- **Local Sovereignty** – Each node independently chooses which interpretations to adopt and can exit at any time.
- **Bounded Scope** – Documents affect interpretation and weighting only; protocol validity and safety are out of scope.
- **Forkability Over Enforcement** – Disagreement results in divergence, not coercion.

## Digital Documents
On system bootstrap, a semantic anchor publishes two initial DigitalDocuments. All later DigitalDocuments are considered adopted only if they pass the defined vote under a node’s chosen interpretation. Nodes remain free to adopt, ignore, or fork at any time.

### Contribution Points
Defines how contribution points are earned, decay, and interpreted for shared local resource management. Contribution points have **local meaning only**. Each node interprets them independently based on evidence it observes and does not require global accounting or complete history.

### Voting
Defines how votes are cast, counted, and interpreted. Votes have **local meaning only**. Each node evaluates votes from signed ballots it observes and does not require a global voter list or canonical tally.

## Complexity

As the number of DigitalDocuments grows, complexity of some Nodes will grow. This is unavoidable. Atlas is designed so nodes can safely ignore most of documents. Complexity is kept explicit and optional, rather than hidden in protocol rules or client defaults, keeping entry barriers low and power.
