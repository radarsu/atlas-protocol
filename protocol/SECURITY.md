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
2. Invalid identity-based Proof-of-Work  
   - Some peers may require TOTP time-windowed PoW  
3. Invalid envelope/message format  

Only peers passing transport validation proceed.

---

## Hard Security Model

Protocol-level security relies strictly on cryptographic enforcement:

- Mandatory Ed25519 signatures
- Identity-bound KeyPoW
- Tiered Argon2-based Proof-of-Work
- Timestamp drift enforcement
- Deterministic signature payload validation

This layer performs no trust, identity assurance, or governance evaluation.  
It strictly validates transport authenticity and resource commitment.
