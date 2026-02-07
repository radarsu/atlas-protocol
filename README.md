# Atlas — Your data, your rules

## Table of contents

| Document | Description |
|----------|-------------|
| [AUTH](AUTH.md) | Authentication |
| [GOVERNANCE](GOVERNANCE.md) | Governance |
| [MANIFEST](MANIFEST.md) | Manifest |
| [NOSTR](NOSTR.md) | Nostr differences |
| [PREFERENCES](PREFERENCES.md) | Preferences |
| [REST](REST.md) | REST API |
| [SECURITY](SECURITY.md) | Security |
| [TYPES-OF-APPS](TYPES-OF-APPS.md) | Types of applications |

## Overview

Atlas is a transport-protcol-agnostic P2P decentralized publishing and discovery protocol where **users own their data** and **govern it to converge into structured, interoperable databases**, empowering users and developers.
<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/3f8201da-63b8-4c78-86c1-71e05fd77c5f" />

The system separates 5 elements:

- **Identity** - cryptographic private key.
- **Content** - identified by hash, signed by author and possibly readable only by targetted audience.
- **Storage** - distributed databases (best-effort caches) for standardized formats.
- **Semantics** - governed by open contribution processes (Schema.org).
- **Gossip** - knowledge of other Nodes.

## Atlas-compliant systems MUST be:

- **Open** - Allow configuration of Nodes to pull data from, all data MUST be public and exportable.
- **Fair** - Communicate with all Nodes equally, in accordance with their transparent local policy.
- **Resilient** — Nodes MUST reject unverified or anonymous traffic to protect the network's collective resources and maintain high-signal discovery.

## Atlas-compliant systems SHOULD be:

- **Semantic** - Built on top of structured data formats, allowing interoperable, distributed databases to emerge in the ecosystem, preserving user's ownership over his data.
- **Gossiping** - Share and aggregate knowledge about known Nodes and their metadata to improve network discoverability.

## Core Concepts

### User

Person controlling a cryptographic private key.

- Creates, signs, and publishes Envelopes (data).
- Chooses to which Nodes to publish to and from which Nodes to read from.

### Connectors

Stateless servers that allow Nodes to connect when they don't have means for direct discoverability and connection, for example:

- WebRTC offer/answer exchanges.
- WebSocket servers.

### Node

Each user may turn into a Node and start hosting data in P2P network by enabling any subset of features:

**Storage**

- Pull data from known Nodes to achieve desired state.
- Accept queries to it's database.
- Accept subscriptions to notifications about Envelopes.
- Accept Envelopes from users.

**Gossip**

- Share knowledge about own metadata.
- Share knowledge about known Nodes and their metadata (description, supported endpoints, data types, data hashes, public key to claimed name mappings).

### Envelope

Signed, immutable unit of data with a strictly defined structure.

- "Payment" Envelopes may contain proof of payment verifiable by Nodes with Lightning integration (convention).
