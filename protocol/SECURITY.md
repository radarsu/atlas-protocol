# Atlas Protocol Security

## Overview
<img width="1315" height="935" alt="image" src="https://github.com/user-attachments/assets/63e0106a-ccbc-4153-a133-a749e33e364a" />

Protocol-level security governs network admission at the transport layer.

Its purpose is to:
- Enforce cryptographic integrity
- Prevent resource exhaustion
- Provide baseline Sybil resistance
- Ensure message structure correctness

Only peers that pass protocol validation may interact with the network.

---

## Entry Point

### Unknown Peer

A new peer attempting to communicate with the network.

---

## Transport Layer Rejection

Peers are immediately rejected if any of the following fail:

1. Invalid identity headers or signature  
2. Invalid identity-based time-windowed Proof-of-Work  
3. Invalid envelope/message format  

Only peers passing transport validation proceed.

---

## Hard Security Model

Protocol-level security relies strictly on cryptographic enforcement:

- Mandatory Dilithium-Crystal signatures
- Identity-bound KeyPoW
- Tiered Argon2-based Proof-of-Work
- Timestamp drift enforcement
- Deterministic signature payload validation

This layer performs no trust, identity assurance, or governance evaluation.  
It strictly validates transport authenticity and resource commitment.

---

## Protocol Headers

All network-compliant endpoints MUST use:

```
Authorization: PublicKey <publicKey>
X-Atlas-PoW: <nonce>.<powHash>
X-Atlas-Signature: t=<Method|UnixTimestampMs|PathWithQuery>; s=<Base64Signature>
```

Where:

```
powHash = Argon2(
    BLAKE3(publicKey) || timeCode,
    nonce
)
```

---

## Public Time-Derived Function (Windowed Freshness)

Atlas uses a deterministic, publicly computable time-derived function to prevent long-term PoW precomputation.

No secret is used. The function is globally predictable by design.

### Time Code Derivation

For a selected tier window:

- 1 day
- 7 days
- 30 days
- 365 days

Peers compute:

```
counter = floor(unixTime / windowSeconds)
timeCode = BLAKE3("atlas" || counter)
```

Where:

- `"atlas"` is a domain separator
- `counter` ensures deterministic rotation per window

The PoW challenge becomes:

```
challenge = BLAKE3(publicKey) || timeCode
```

Argon2 is executed using:

```
powHash = Argon2(challenge, nonce)
```

The PoW is therefore valid only within the selected time window.

---

## PoW Construction Flow

1. Peer selects a window (1d / 7d / 30d / 365d).
2. Compute:
   - `publicKeyHash = BLAKE3(publicKey)`
   - `counter = floor(unixTime / windowSeconds)`
   - `timeCode = BLAKE3("atlas" || counter)`
   - `challenge = publicKeyHash || timeCode`
3. Execute Argon2 with chosen parameters and `nonce`.
4. Submit:

```
X-Atlas-PoW: <nonce>.<powHash>
```

---

## Server Verification Flow

Upon receiving a request:

1. Extract the public key.
2. Verify signature.
3. Recompute `publicKeyHash`.
4. For each supported tier (in deterministic order):
   - Compute `counter`
   - Derive `timeCode`
   - Rebuild `challenge`
   - Recompute Argon2 using provided `nonce`
   - Validate difficulty target
5. Accept on first successful match.

Servers MUST evaluate tiers in-order from shortest to longest:

1. 1-day
2. 7-day
3. 30-day
4. 365-day

The first successful match defines effective freshness.

---

## Freshness Semantics

| Window   | Freshness | Reuse Horizon | Relative Strength |
|----------|-----------|---------------|-------------------|
| 1-day    | Very high | 24 hours      | Highest           |
| 7-day    | High      | 7 days        | Strong            |
| 30-day   | Medium    | 30 days       | Moderate          |
| 365-day  | Baseline  | 1 year        | Lowest            |

Shorter windows increase computational churn and Sybil resistance.  
Longer windows reduce cost but allow longer identity reuse.

---

## Security Properties

This mechanism provides:

- Deterministic global verification  
- Publicly computable freshness rotation  
- No shared secrets  
- Resistance to long-term PoW precomputation  
- Time-rotating computational cost  

This mechanism remains strictly protocol-level enforcement. Eventual attacks on the network must be planned in advance and aligned to a specific active time window, significantly reducing attacker flexibility and converting Sybil preparation into a recurring operational cost rather than a one-time investment.

On top of that, Peers are equipped with two robust PoW-based escalation mechanisms:

- **Requirement of stronger PoW**  
  Nodes MAY increase required `memoryCost`, `timeCost`, or `difficultyBits` under load or suspicion, directly raising the marginal cost per identity.

- **Requirement of smaller time-windowed PoW**  
  Nodes MAY require shorter freshness windows (e.g., 1-day instead of 30-day), forcing attackers to recompute PoW more frequently and increasing sustained computational burn.

Together, these controls allow adaptive economic pressure against abuse while preserving deterministic, stateless verification at the protocol layer.