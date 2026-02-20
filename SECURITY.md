# Atlas - Security

In a headless network, health is maintained through a **dual-layered defense strategy**. While **Hard Security** (cryptographic enforcement) ensures technical integrity, Atlas also uses **Soft Security** (social dynamics, gossip, and Proof of Service between nodes) for long-term robustness.

**Hard Security:** Mandatory Ed25519 signatures and tiered Proof-of-Work (PoW) prevent resource exhaustion and Sybil attacks at the protocol level.

**Soft Security:** The protocol is designed so security is amplified by social dynamics. When most Nodes establish trust mechanisms and behave cooperatively, built-in gossip and testing allow "common responsibility" to emerge as part of the security strategy, providing extra resilience against high-level coordination attacks that code alone cannot solve.

Nodes MAY implement any subset of endpoints, but MUST adhere to the security standards below to be part of the network.

## Protocol Headers

**X-Atlas-Public-Key and X-Atlas-Signature**

All requests MUST include the client's public key and signature. Nodes MUST reject public keys that do not start with a conventional prefix:

- **cit** (citizen; weak PoW; READ-only access limited to `GET` requests)
- **tita** (titan; medium PoW)
- **atlas** (atlas; strong PoW)

**Header**: `X-Atlas-Public-Key: <Public key of sender (not necessarily author)>`

**Header**: `X-Atlas-Signature: t=<Method|Current Unix timestamp in milliseconds>|Path; s=<Sender signature>` (example: GET|1707052800000|envelopes?where=...1707052800000)

Clients that omit any of these headers MUST receive `401 Unauthorized`.
Clients whose timestamp drift is larger than 5 minutes SHOULD be rejected.

### X-Atlas-Knowledge-Share

Nodes MAY periodically ask clients for help with network discovery by returning this header:

**Header**: `X-Atlas-Knowledge-Share: region=EU; datatypes=ClaimReview,Person`

**Example Behavior**:

- Send once per hour on any response (first request within the hour window)
- Applies to all endpoints (`/envelopes/*`, `/nodes/*`)

```txt
GET /envelopes?where={"authorPublicKey":"abc123"}
Response:
  X-Atlas-Knowledge-Share: region=EU; datatypes=ClaimReview,Person
  [{ "hash": "...", ... }]
```

Recipients SHOULD react to header by sharing their known nodes via POST to the advertised endpoint (e.g., `POST /nodes/announcements`).
