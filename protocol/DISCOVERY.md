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
