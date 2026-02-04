## Differences with Nostr

Nostr optimizes for freedom and survivability by minimizing coordination. This prevents protocol-level gatekeeping, but shifts coherence, discovery, and interpretation entirely to clients. Over time, this might cause power to concentrate at the client layer, where dominant applications implicitly define data meaning, filtering logic, ranking, and social norms through proprietary conventions and defaults.

Atlas starts from the observation that client-level power concentration is unavoidable in incoherent data environments. Where structure is undefined at the protocol level, the most widely adopted client becomes the de facto semantic authority.

Atlas therefore accepts explicit, transparent semantic governance. By converging on shared, versioned schemas, Atlas allows queries and aggregation logic to be portable across clients. This shifts power from opaque client implementations to auditable, forkable, collectively governed semantic standards, enabling interoperable personal databases and more stable user experience across applications — at the cost of increased coordination pressure.

### Core Goal Comparisons

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
| Stable document ID | `(pubkey, kind, d)`                      | IPFS-compliant has     |
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
