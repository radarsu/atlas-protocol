# Atlas Security Model

## Overview
<img width="1315" height="935" alt="image" src="https://github.com/user-attachments/assets/63e0106a-ccbc-4153-a133-a749e33e364a" />

The diagram describes a staged security pipeline for peer admission and participation inside the Atlas Network.  
Peers move through progressively stricter validation layers:
1. **On protocol level** - transport and protocol compliance.
2. **On order level** - trust and identity verification.

---

## 1. Entry Point

### Unknown Peer
A new peer attempting to join the network.

---

## 2. Transport Layer Rejection

Peers are immediately rejected if any of the following fail:

1. Invalid identity headers or signature  
2. Invalid identity-based Proof-of-Work
   - Some peers may require TOTP time-windowed PoW
3. Invalid envelope/message format

Only peers passing transport validation proceed.

---

## 3. Trust Layer

### Zero-Reputation Peer

A protocol-compliant peer starts with no trust.

#### Trust Rejection Conditions

A peer may be rejected if:

1. Identity is too fresh (insufficient age)
2. Excess distrust assigned by other peers  
   - Distrust accumulates quadratically  
   - Example threshold: 100 distrust quadratic points

Peers that pass move forward.

---

## 4. P2P Verification Layer

### P2P Verified

Peer successfully passes decentralized verification checks.

---

## 5. Identity Assurance Layer

### Formally Verified

Peer identity verified via trusted identity authority.

#### Identity Rejection Conditions

1. No verification by trusted identity authority  
2. No one-person-one-account enforcement  
   - Must be guaranteed by three randomized trusted identity verifications  

---

## 6. Uniqueness Enforcement

### One-Person-One-Account Enforced

The system enforces identity uniqueness before governance access.

---

## 7. Governance Layer

### Order Governance

Only fully verified, uniqueness-enforced peers can participate in governance processes.

---

## Security Model Summary

The Atlas Protocol + Order Security architecture combines:

- Cryptographic transport validation
- Identity-bound Proof-of-Work
- Protocol compliance gating
- Quadratic peer distrust scoring
- Identity age sensitivity
- Decentralized P2P verification
- Formal identity verification
- Enforced uniqueness constraints
- Governance access control


# Atlas - Security

In a headless network, health is maintained through a **dual-layered defense strategy**. While **Hard Security** (cryptographic enforcement) ensures technical integrity, Atlas also uses **Soft Security** (social dynamics, gossip, and Proof of Service between nodes) for long-term robustness.

**Hard Security:** Mandatory Ed25519 signatures and tiered Proof-of-Work (PoW) prevent resource exhaustion and Sybil attacks at the protocol level.

**Soft Security:** The protocol is designed so security is amplified by social dynamics. When most Nodes establish trust mechanisms and behave cooperatively, built-in gossip and testing allow "common responsibility" to emerge as part of the security strategy, providing extra resilience against high-level coordination attacks that code alone cannot solve.

Nodes MAY implement any subset of endpoints, but MUST adhere to the security standards below to be part of the network.

## Protocol Headers

**Authorization (KeyPow) and X-Atlas-Signature**

Guarded API endpoints (`/envelopes/*`, `/nodes/*`) use:

- `Authorization: KeyPow <publicKeyWithPow>`
- `X-Atlas-Signature: t=<Method|UnixTimestampMs|PathWithQuery>; s=<Base64Signature>`

`publicKeyWithPow` means the composed KeyPoW identity key:
`<apub1...>.<powNonce>.<powHash>`

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
