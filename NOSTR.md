# Atlas - Differences with Nostr

## NIPs vs Common Schema governance
Nostr optimizes for freedom and survivability by minimizing coordination. This prevents protocol-level gatekeeping, but shifts coherence, discovery, and interpretation entirely to clients. Over time, this might cause power to concentrate at the client layer, where dominant applications implicitly define data meaning, filtering logic, ranking, and social norms through proprietary conventions and defaults.

Atlas starts from the observation that client-level power concentration is unavoidable in incoherent data environments. Where structure is undefined at the protocol level, the most widely adopted client becomes the de facto semantic authority.

Atlas therefore accepts explicit, transparent semantic governance. By converging on shared, versioned schemas, Atlas allows queries and aggregation logic to be portable across clients. This shifts power from opaque client implementations to auditable, forkable, collectively governed semantic standards, enabling interoperable personal databases and more stable user experience across applications — at the cost of increased coordination pressure.

## The "NIP-Soup" Problem

In Nostr, if you want to build a new feature (e.g., a "Task Manager" or "Medical Record"), you have two choices:
1. **The Slow Way:** Propose a new NIP (Nostr Implementation Possibility), argue for months on GitHub, and hope other devs implement it.
2. **The Fast Way:** Just pick a random kind number, define your own custom tags, and ship.

In Atlas we accept Schema.org as a source of our truth, taking it as a common "skeleton" of semantic meanings. The interpretations might differ. Compatibility might be limited, but we have solid foundations to start from. And as most of coders already use AI agents - accidential convergance becomes desired and likely. This is important because non-interoperable data and custom data structures is what effectively causes data lock-ins and user loosing power.

## Private key and convenience
In Nostr, security is simple and delegated keys approach are an afterthrough. That ultimately will lead to high risk of losing a key and therefore - losing your identity.

In Atlas, mechanisms to generate scoped, time-limited delegated keys are baked into the protocol, as well as OIDC-like flow for keeping root private key safe and recoverable for casual users.

## Discoverability
In Nostr, each Relays has some random mixture of data. You somehow discover relays, then you connect and fetch. Finally, you somehow  make sense out of the data with heavy client-side logic. Extreme overfetching is natural.

In Atlas, each Node is specialized and interested only in specific kinds of data and Nodes have predictable discovery mechanisms. They talk to each other, they test each other and gossip. This allows client-side to hit exactly where it needs to get exactly type of data it needs, with minimized overfetching. Scalability and performance is greatly improved and each microservice can specialize in fast storage and delivery of specific structured data type. It's more like organized microservices instead of random caches.

## Websocket doubts
I have been using Websockets quite a lot in the past. 7 years ago I was already experimenting with systems built fully on Websockets, as I believed they're a good fit (https://github.com/radarsu/rpc-websocket-client). Eventually, I encountered numerous problems with scaling them. Numerous problem with browser compatibility. Even right now Nostr websites are not working well on browser of my choice - Brave, because Brave doesn't allow too many Websocket connections.

I got my lessons and I believe Websockets in Nostr are going to slow down it's adoption and block client-only apps, because effectively every higher quality app will need to use Websockets mostly on the backend to then provide classic HTTP feed with all the browser-level caching and other goodness.

Atlas goal is to lower the entry barrier and enable developers to make meaninful, high quality apps, without ever writing single line of backend code or messing with custom databases. Pure client-side apps, connecting to user's trusted nodes are the cleanest, privacy-first approach [[TYPES-OF-APPS.md](./TYPES-OF-APPS.md)].

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
