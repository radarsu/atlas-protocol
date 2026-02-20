# Atlas - Differences with Nostr

## Governance

Nostr optimizes for freedom and survivability by minimizing coordination. This prevents protocol-level gatekeeping, but shifts coherence, discovery, and interpretation entirely to clients. Over time, this might cause power to concentrate at the client layer, where dominant applications implicitly define data meaning, filtering logic, ranking, and social norms through proprietary conventions and defaults.

Nostr claims to avoid authority, but that actually **hides it in relays and clients** (meaning is client-defined and power sits in defaults). Nostr intentionally refuses expressiveness to avoid coordination complexity.

- Clients decide how to interpret events
- Relays decide what to host
- Users choose relays

Atlas starts from the observation that client-level power concentration is unavoidable in incoherent data environments. Where structure is undefined at the protocol level, the most widely adopted client becomes the de facto semantic authority. In a distributed network, when it grows beyond trivial scale, it needs governance in the same sense people need it: to prevent its silent capture by money, defaults, or inertia. **The kind of governance matters more than the fact that it exists.**

Atlas therefore accepts explicit, transparent semantic governance. By converging on shared, versioned schemas, Atlas allows queries and aggregation logic to be portable across clients. This shifts power from opaque client implementations to auditable, forkable, collectively governed semantic standards, enabling interoperable personal databases and more stable user experience across applications, at the cost of increased coordination pressure.

### The Three Ways Power Appears (Whether You Want It or Not)

If you do nothing, power concentrates via:

#### Money

- Hosting Relays  
- Paying developers  
- Running infrastructure  
- Funding client defaults  

This leads to **economic governance**.

#### Defaults

- Popular clients  
- Preconfigured settings  
- UX shortcuts  

This leads to **governance by accident**.

#### Social Authority

- Influencers  
- Maintainers  
- Core contributors  

This leads to **governance by reputation without accountability**.

**None of these are fair.**
**None are neutral.**

### What Governance Is Actually For

Governance in distributed systems is **not** about:

- Control
- Commands
- Central planning

It **is** about:

- Bounding power
- Making influence visible
- Preserving exit
- Preventing capture

This is closer to **constitutional law** than to management. This is not perfect. But it makes the problem visible and contestable. We believe that is a major improvement over pretending it doesn't exist.

## The "NIP-Soup" Problem

In Nostr, if you want to build a new feature (e.g., a "Task Manager" or "Medical Record"), you have two choices:
1. **The Slow Way:** Propose a new NIP (Nostr Implementation Possibility), argue for months on GitHub, and hope other devs implement it.
2. **The Fast Way:** Just pick a random kind number, define your own custom tags, and ship.

In Atlas, we use Schema.org as a common "skeleton" of semantic meanings. Interpretations might differ, and compatibility may still be limited, but this gives us a strong foundation. As more developers use AI agents, accidental convergence becomes more likely. This matters because non-interoperable data and custom data structures cause lock-in and users lose power.

## Private key and convenience

In Nostr, security is simple and delegated-key approaches are an afterthought. That increases the risk of losing a key and, as a result, losing your identity.

In Atlas, mechanisms to generate scoped, time-limited delegated keys are baked into the protocol, as well as OIDC-like flow for keeping root private key safe and recoverable for casual users.

## Discoverability

In Nostr, each relay has a random mixture of data. You discover relays, connect, fetch, and then make sense of data with heavy client-side logic. Overfetching is natural.

In Atlas, each Node is specialized for specific data types, and nodes have predictable discovery mechanisms. They talk to each other, test each other, and gossip. This lets clients query exactly where they need to for specific data, with less overfetching. Scalability and performance improve, and each microservice can specialize in storing and delivering a specific structured data type. It behaves more like organized microservices than random caches.

## WebSocket doubts

I have used WebSockets a lot in the past. Seven years ago, I experimented with systems built fully on WebSockets because I believed they were a good fit (https://github.com/radarsu/rpc-websocket-client). Eventually, I encountered many scaling and browser-compatibility problems. Even now, some Nostr websites do not work well in Brave because it limits WebSocket connections.

Based on that experience, I believe WebSockets in Nostr may slow adoption and block client-only apps. In practice, higher-quality apps often end up using WebSockets mostly on the backend, then serving a classic HTTP feed to benefit from browser caching and related optimizations.

Atlas aims to lower the entry barrier and enable developers to build meaningful, high-quality apps without writing backend code or managing custom databases. Pure client-side apps that connect to a user's trusted nodes are the cleanest, privacy-first approach ([TYPES-OF-APPS.md](./TYPES-OF-APPS.md)).

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
| Stable document ID | `(pubkey, kind, d)`                      | IPFS-compliant hash    |
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
