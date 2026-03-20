# Atlas Protocol – FAQ

## 1. What is Atlas Protocol?
Atlas is a transport-agnostic, peer-to-peer publishing and discovery protocol where users own their data. It aims to create interoperable, structured databases governed by users rather than platforms.

---

## 2. What problem does Atlas solve?
Atlas addresses:
- **Data lock-in** – Users cannot easily move data between platforms.
- **Platform centralization** – Network effects allow dominant platforms to degrade quality and extract value.
- **Engagement-driven content decay** – Algorithms optimized for attention produce low-quality, manipulative content.

Atlas shifts control of data formats and governance from platforms to users.

---

## 3. How is Atlas different from other decentralized protocols (e.g., Nostr, ActivityPub, AT Protocol)?
Most alternatives:
- Focus on a single data type (e.g., social posts).
- Federate identity in ways that reintroduce centralization.
- Lack strong incentives and user-friendly infrastructure.

Atlas is more ambitious:
- General-purpose data layer.
- User-enforced data standards.
- Integrated governance and economic mechanisms.
- Focus on a “killer app” (e.g., browser layer) to prevent centralization.

---

## 4. What does “sovereign data layer” mean?
Each user runs a **Node** that:
- Stores their own data.
- Optionally stores (“pins”) selected network data.
- Automatically gathers data matching user-defined filters.

Data structures are defined by users, not platforms. Apps pull from user-owned data instead of locking it in proprietary silos.

---

## 5. How does Atlas reduce enshittification?

Atlas mitigates platform decay by:

### 1. Eliminating data lock-in
- Users define data formats.
- Apps must adapt to user data, not vice versa.

### 2. Enabling alternative algorithms
- Interoperable data makes it easy to build competing frontends and ranking systems.

### 3. Enforcing transparency
- SDKs, tools, and potentially a dedicated Atlas browser can:
  - Flag opaque algorithms.
  - Warn about non-compliant services.
  - Exert social pressure toward transparent behavior.

---

## 6. How does Atlas handle spam and abuse?

Atlas has two layers:

### Protocol Layer (Technical)
- Entry requires a private key containing Proof-of-Work (PoW).
- Nodes must sign communications (identity verification).
- Nodes enforce rate limiting and filtering independently.
- Topic-based trust system.

### Order Layer (Governance)
- Web-of-Trust model.
- Decentralized registries (e.g., identity verification).
- One public key = one person (formal identity registries).
- Randomized, multi-registry verification (e.g., 3 independent validators).

This enables secure voting, delegation, and identity enforcement.

---

## 7. What is the “Order” layer?
The Order layer is Atlas’ governance and economic framework. It allows:
- Trusted decentralized registries.
- Voting and delegation.
- Identity enforcement.
- Economic coordination via the FairShares system.

---

## 8. What are FairShares?
FairShares are a protocol-native unit used for:
- Access control (e.g., paywalled content).
- Signaling importance (e.g., boosting posts or emails).
- Paying for storage or API usage.
- Managing network “kindness” and anti-spam limits.

They are not a cryptocurrency and are not pegged to USD.

---

## 9. Are FairShares a cryptocurrency or stablecoin?
No.

- Not a store of value.
- Not pegged to any fiat currency.
- Designed to circulate, not be hoarded.
- Collapse toward a defined equilibrium over time (e.g., via burn mechanics).

They function as a network usage and influence metric, not speculative capital.

---

## 10. How do users obtain FairShares?
Users receive FairShares according to a shared formula (similar to a universal baseline allocation in non-scarce digital contexts).

If users consume network resources without contributing, they eventually run out and must:
- Contribute storage.
- Publish valuable content.
- Provide services.
- Earn FairShares from others.

---

## 11. Why would someone run a Node?

### Soft incentives (Protocol layer)
- Host personal data.
- Share data with family/friends.
- Use high-quality apps (e.g., email client, CMS).

### Economic incentives (Order layer)
- Earn FairShares for services (storage, content, API access).
- Monetize content via FairShares-based paywalls.
- Gain influence through recognized contributions.

Long-term success depends on strong incentive alignment and compelling applications.

---

## 12. Can FairShares gain real-world value?
Indirectly, possibly.

While not designed as a store of value:
- Assets can be registered within the system.
- Social recognition and governance may reinforce ownership claims.
- Value may emerge by convention if widely used for signaling intensity and access.

Large capital accumulation is intentionally limited to prevent centralization.

---

## 13. Does Atlas support payments like Bitcoin Lightning?
Yes. External systems (e.g., Bitcoin Lightning) can be integrated as optional components.

FairShares are for internal coordination and signaling. External monetary systems can be used for real-world payments.

---

## 14. What is the long-term vision?

Key components include:

- **Atlas Browser (or equivalent desktop layer)**  
  Enforces:
  - Transparent algorithms.
  - Respect for user-defined preferences.
  - Easy data contribution and discovery.

- Protocol-level discovery.
- Structured knowledge systems (e.g., wiki-like layers).
- Modular integrations (payments, identity, registries).

The browser layer is considered critical to preventing re-centralization.

---

## 15. How does Atlas prevent power concentration?
Through:
- Caps on resource accumulation.
- Transparent registries.
- Randomized verification processes.
- Limited hoarding of protocol resources.
- Governance based on trust percentiles.

The economic model favors contestable power and distributed ownership over rapid centralized expansion.

---

## 16. Is Atlas optimized for rapid growth?
No.

Atlas prioritizes:
- Fairness.
- Transparency.
- Interoperability.
- Decentralized governance.

It may be less efficient for rapid capital concentration but is designed to prevent power abuse and systemic centralization.

---

## 17. What could drive adoption?
- A compelling “killer app” (e.g., superior email client or browser).
- Clear UX advantage.
- Strong incentive design.
- Network effect driven by utility rather than speculation.

Without a strong application layer, decentralization efforts tend to recentralize for convenience. Atlas explicitly aims to address that.