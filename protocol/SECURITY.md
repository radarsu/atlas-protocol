# Atlas - Security

In a headless network, health is maintained through a **dual-layered defense strategy**. While **Hard Security** (cryptographic enforcement) ensures technical integrity, Atlas also uses **Soft Security** (social dynamics, gossip, and Proof of Service between nodes) for long-term robustness.

**Hard Security:** Mandatory Ed25519 signatures and tiered Proof-of-Work (PoW) prevent resource exhaustion and Sybil attacks at the protocol level.

**Soft Security:** The protocol is designed so security is amplified by social dynamics. When most Nodes establish trust mechanisms and behave cooperatively, built-in gossip and testing allow "common responsibility" to emerge as part of the security strategy, providing extra resilience against high-level coordination attacks that code alone cannot solve.

Nodes MAY implement any subset of endpoints, but MUST adhere to the security standards below to be part of the network.

## Protocol Headers

**Authorization (KeyPow) and X-Atlas-Signature**

Guarded API endpoints (`/envelopes/*`, `/nodes/*`) use:

- `Authorization: KeyPow <publicKey>`
- `X-Atlas-Signature: t=<Method|UnixTimestampMs|PathWithQuery>; s=<Base64Signature>`

Current guard behavior:

- On `GET`, both headers are required. Missing or invalid headers MUST be rejected (`401 Unauthorized`).
- On non-`GET`, `Authorization` is optional in the current implementation.
- If a non-`GET` request provides a Citizen-tier `Authorization` key, it MUST be rejected (`401`) because Citizen is read-only.
- Signature timestamp drift above 5 minutes MUST be rejected.
- Signature payload must exactly match `METHOD|timestamp|originalUrlWithQuery`.

KeyPoW tier recognition is threshold-based (not exact-profile based):

- Minimum accepted tier is Citizen (`memoryCost >= 2048*1024`, `timeCost >= 1`, difficulty >= 4 leading zero bits).
- Higher Argon2 settings are valid and map to the highest satisfied tier threshold.

For Envelope writes, Node identity policy can enforce stricter minimum author tier via `acceptTier` (`citizen`/`titan`/`atlas`).

### X-Atlas-Knowledge-Share

Nodes MAY periodically ask clients for help with network discovery by returning this header:

**Header**: `X-Atlas-Knowledge-Share: /nodes/announcements`

**Example Behavior**:

- Send once per hour on any response (first request within the hour window)
- Applies to all endpoints (`/envelopes/*`, `/nodes/*`)

```txt
GET /envelopes?where={"authorPublicKey":"abc123"}
Response:
  X-Atlas-Knowledge-Share: /nodes/announcements
  [{ "hash": "...", ... }]
```

Recipients SHOULD react to header by sharing their known nodes via POST to the advertised endpoint (e.g., `POST /nodes/announcements`).
