# Atlas Protocol (REST)

In a headless network, strict security is the primary guarantee of network health. By enforcing cryptographic signatures and Proof-of-Work (PoW) tiering at the edge, Atlas prevents resource exhaustion (DDoS) and mitigates sybil attacks. This "strict-by-default" architecture allows the network to remain open and performant without relying on central gatekeepers or complex consensus overhead.

Therefore, Nodes MAY implement any subset of endpoints, but MUST adhere to the security standards defined below to participate in the gossip pool.

## Protocol Headers

**X-Atlas-Public-Key and X-Atlas-Signature**

All requests MUST include the client's public key and signature. Nodes MUST reject public keys which do not start with: **cit** (citizen), **tita** (titan), **atlas** (atlas).

**Header**: `X-Atlas-Public-Key: <Public key of sender (not necessarily author)>`
**Header**: `X-Atlas-Signature: t=<Method|Current Unix timestamp in milliseconds>|Path; s=<Sender signature>` (example: GET|1707052800000|envelopes?where=...1707052800000)

Clients that omit this header on GET requests SHOULD receive `401 Unauthorized`.
Clients whose timestamp dift is larger than 5 minutes SHOULD be rejected.
For unauthorized users, there should be a hardcoded in-app private/public key used.

### X-Atlas-Knowledge-Share

Nodes MAY periodically ask Clients for help with network discovery, by responding with a header.

**Header**: `X-Atlas-Knowledge-Share: region=EU; datatypes=ClaimReview,Person`

**Example Behavior**:
- Send once per hour on any response (first request within the hour window)
- Applies to all endpoints (`/envelopes/*`, `/nodes/*`)

```
GET /envelopes?where={"authorPublicKey":"abc123"}
Response:
  X-Atlas-Knowledge-Share: region=EU; datatypes=ClaimReview,Person
  [{ "hash": "...", ... }]
```

Recipients SHOULD react to header by sharing their known nodes via POST to the advertised endpoint (e.g., `POST /nodes/announcements`).

## Storage

### Envelope

```json
{
  "hash": "<CIDv1 of canonical JSON bytes (raw + sha2-256; IPFS compliant)>",
  "signature": "<Public key matching Ed25519 signature>",
  "data": "<Structured data JSON>",
  "authorPublicKey": "<Public key of Envelope author>",
  "createdAt": "<ISO-8601 date of accepting Envelope by Node>",
  "recipientKeyId": "<BLAKE3 hash of recipient public key>"
}
```

### Get Envelopes (Prisma Filters)

`GET /envelopes`

Query params:
```
where=<JSON>
orderBy=<JSON>
take=<number>
skip=<number>
```

Example:
```
GET /envelopes?where={"authorPublicKey":"<pk>"}&orderBy={"createdAt":"desc"}&take=50
```

Response: Envelope[]

### Get Envelope By Hash

`GET /envelopes/:hash`

Response: Envelope

### Publish Envelope

`POST /envelopes`

Request body: Envelope

Response:
```json
{ "ok": true }
```

### Upload Blob
Blobs MUST be sent BEFORE Envelope.

`POST /blobs`

Content-Type: application/octet-stream (or another appropriate media type, e.g. image/png)
Request body: raw bytes

Response:
```json
{
    "cid": "<CIDv1 (raw, sha2-256)>",
    "size": 245678,
    "encodingFormat": "image/png",
    "urls": [
        "https://node.example.com/blobs/bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi",
        "ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi"
    ]
}
```

Clients MUST assume that blob availability is not guaranteed.

### Referencing Blobs from Envelopes
```json
{
    "@type": "ImageObject",
    "contentUrl": [
        "https://node.example.com/blobs/bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi",
        "ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oclgtqy55fbzdi"
    ],
    "encodingFormat": "image/png"
}
```

## Gossip

### Node Metadata

`GET /nodes/self`

Response:
```json
{
  "publicKey": "<node public key>",
  "publicUrl": "<node public url>",
  "description": "<optional>",
  "endpoints": ["https://node.example.com"],
  "dataTypes": [{ /* <schema collection rules> */ }],
  "region": "EU",
  "load": "medium",
  "availableStorageGb": "10"
}
```

### Known Nodes

`GET /nodes/known`

Response:
```json
[
    {
        "publicKey": "<node public key>",
        "endpoints": ["https://node.example.com"],
        "dataTypes": [{ /* <e2e tests results> */ }],
        "latencyMs": 90, // Milliseconds
        "uptime": 0.998,
    }
]
```

### Announce Nodes

`POST /nodes/announcements`

Request body:
```json
[
    "https://node.example.com",
    "https://another-node.example.com"
]
```

Response:
```json
{
    "ok": true,
}
```