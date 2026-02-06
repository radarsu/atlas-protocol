# Atlas - Rest

Nodes MAY implement any subset of endpoints, but MUST adhere to the security standards defined below to participate in the gossip pool.

## Storage

### Envelope

```json
{
  "hash": "<CIDv1 of canonical JSON bytes (raw + sha2-256; IPFS compliant)>",
  "data": "<Structured data JSON>",
  "signature": "<Signature on data matching>",
  "authorPublicKey": "<Public key of Envelope author>",
  "createdAt": "<ISO-8601 date of accepting Envelope by Node>",
  // Optional
  "supersedes": "<Envelope that it intends to override>",
  "delegationProof": "<Permit Envelope>", // Check AUTH.md
  "recipientKeyId": "<BLAKE3 hash of recipient public key>",
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