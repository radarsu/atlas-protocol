# Atlas - Differences with Nostr

Nostr optimizes for freedom and survivability by minimizing coordination. This prevents protocol-level gatekeeping, but shifts coherence, discovery, and interpretation entirely to clients. Over time, this might cause power to concentrate at the client layer, where dominant applications implicitly define data meaning, filtering logic, ranking, and social norms through proprietary conventions and defaults.

Atlas starts from the observation that client-level power concentration is unavoidable in incoherent data environments. Where structure is undefined at the protocol level, the most widely adopted client becomes the de facto semantic authority.

Atlas therefore accepts explicit, transparent semantic governance. By converging on shared, versioned schemas, Atlas allows queries and aggregation logic to be portable across clients. This shifts power from opaque client implementations to auditable, forkable, collectively governed semantic standards, enabling interoperable personal databases and more stable user experience across applications — at the cost of increased coordination pressure.

Nostr solves the "Who owns my account?" problem, but it does not solve "Do these two apps actually work together?".

## The "NIP-Soup" Problem (Reinventing Wheels)

In Nostr, if you want to build a new feature (e.g., a "Task Manager" or "Medical Record"), you have two choices:
1. **The Slow Way:** Propose a new NIP (Nostr Implementation Possibility), argue for months on GitHub, and hope other devs implement it.
2. **The "Silo" Way:** Just pick a random kind number, define your own custom tags, and ship.

In Atlas we accept Schema.org as a source of our truth, taking it as a common "skeleton" of semantic meanings. The interpretations might differ. Compatibility might be limited, but we have solid foundations to start from. This is important because non-interoperable data and custom data structures is what effectively causes data lock-ins and user loosing power.

## Side-by-side Comparisons

**Atlas's View** - Meaning should be getting baked into the protocol via structured Envelopes and social governance.

**Nostr's View** - Meaning is "emergent.". If an app is successful, its kind and tag structure becomes the de facto standard (i.e., kind: 1 for short notes).

**Nostr is like email (SMTP):** It moves text from A to B. It’s great for freedom, but if you send a "spreadsheet" via email, the recipient needs the right software (the same NIP) to open it.

**Atlas is more like a Distributed Database:** It ensures that not only did the message arrive, but that the data means exactly what it was intended to mean across all Nodes.

### Core Goal

| Dimension          | **Nostr**               | **Atlas**                     |
| ------------------ | ----------------------- | ----------------------------- |
| Primary goal       | Unstoppable publication | Durable, interoperable data   |
| Guarantees         | Authenticity            | Authenticity + coherence      |
| Network assumption | Permanent partial views | Probabilistic convergence     |
| Philosophy         | Refuse responsibility   | Accept bounded responsibility |

### Identity & Data Model

| Aspect             | **Nostr**                                | **Atlas**              |
| ------------------ | ---------------------------------------- | ---------------------- |
| Identity           | Public key                               | Public key             |
| Object identity    | Event hash (version) + addressable slots | EntityId + hash        |
| Mutable state      | Replaceable / addressable events         | Supersedes + entity    |
| Stable document ID | `(pubkey, kind, d)`                      | IPFS-compliant hash     |
| History required   | No                                       | Optional / desirable   |
| Fork handling      | Deterministic (newest wins locally)      | Explicit, policy-bound |

### Storage & Synchronization

| Aspect                 | **Nostr**           | **Atlas**                         |
| ---------------------- | ------------------- | --------------------------------- |
| Storage role           | Cache               | Cooperative cache                 |
| Relay ↔ relay sync     | None                | Encouraged                        |
| Pull-based replication | Client-only         | Node-to-node                      |
| Convergence            | Explicitly rejected | Probabilistic                     |
| Missing updates        | Permanent           | Eventually repaired (best-effort) |

### Semantics & Interpretation

| Aspect                   | **Nostr**        | **Atlas**           |
| ------------------------ | ---------------- | ------------------- |
| Semantics location       | Clients          | Governed schemas    |
| Interoperability         | Convention-based | Schema-based        |
| Risk of semantic capture | Client dominance | Governance capture  |
| Schema evolution         | Ad-hoc           | Versioned, explicit |
| Query portability        | Low              | High                |

### Censorship & Power

| Aspect                 | **Nostr**         | **Atlas**            |
| ---------------------- | ----------------- | -------------------- |
| Censorship resistance  | Maximal           | High but conditional |
| Soft censorship vector | Client defaults   | Schema norms, gossip |
| Power concentration    | Clients           | Schema bodies        |
| Coordination pressure  | Minimal           | Explicit             |
| Legal pressure surface | Many small relays | Large replicators    |

### Reliability & UX

| Aspect               | **Nostr**    | **Atlas** |
| -------------------- | ------------ | --------- |
| Data durability      | Unreliable   | Decent    |
| Freshness            | Inconsistent | Decent    |
| Offline recovery     | Weak         | Strong    |
| Developer ergonomics | Low          | Higher    |

## Operational Complexity

| Aspect                | **Nostr** | **Atlas**  |
| --------------------- | --------- | ---------- |
| Relay/node complexity | Very low  | Medium     |
| Operator cost         | Low       | Low        |
| Specialization        | Rare      | Encouraged |
| Garbage collection    | Trivial   | Required   |
| Attack surface        | Minimal   | Larger     |

### Failure Modes

| Failure             | **Nostr**  | **Atlas**              |
| ------------------- | ---------- | ---------------------- |
| Data loss           | Normalized | Treated as degradation |
| Inconsistency       | Permanent  | Temporary              |
| Fork explosion      | Prevented  | Managed                |
| Governance conflict | External   | Internal               |
| Infra capture       | Clients    | Schemas                |
