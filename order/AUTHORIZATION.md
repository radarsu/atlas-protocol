# Atlas - Authorization

## Authorization via Trusted Service (recommended for most users)

### Onboarding

1. A new user generates a private/public key pair locally in a **trusted client app** (i.e. browser).

2. The user selects an authorization method with a **trusted service**, such as:

   - Login/password
   - OIDC provider (i.e. Google)
   - etc.

3. The private key is encrypted client-side using a secret derived from the chosen authorization method:

   - Strong KDF for passwords
   - Provider-bound derivation for OIDC

4. The encrypted private key is uploaded to the **trusted service**, which never sees the raw private key.

5. The **trusted service** MUST NOT retain the ability to use the user’s private key without fresh authorization material.

---

### Client-App Authorization

1. An unauthorized user clicks their **trusted service** button and gets redirected to the **trusted client app** (where they are authorized), to `/atlas/delegate?url=https://app.example.com&p=[{"@type": "CreateAction","object": {"@type": "SocialMediaPosting"}}]`.

2. In the **trusted client app**, the user sets the time scope (`validFrom` and `validUntil`) and obtains a short-lived secret URL from the **trusted service**.

3. The **trusted service** uses authorization material from the user to prepare (but not yet publish) a delegated private key Permit Envelope:

```json
{
    "@type": "Permit",
    "validFrom": "2026-01-01T00:00:00Z",
    "validUntil": "2026-01-31T23:59:59Z",
    "identifier": {
        "@type": "PropertyValue",
        "propertyID": "delegatedKey",
        "value": "<delegated-public-key>"
    },
    "potentialAction": [
        {
            "@type": "CreateAction",
            "object": {
                "@type": "SocialMediaPosting"
            }
        }
    ]
}
```

4. The user goes back to the unauthorized **client app** and provides the short-lived URL.
5. The **client app** sends `GET` to the URL, requesting the **trusted service** (responding only to a proper `Origin` header, with `no-store, no-cache, max-age=0`), and receives the delegated private key. The short-lived URL immediately stops working.
6. On that `GET`, the **trusted service** publishes the prepared Permit Envelope to the network, which validates delegation across Nodes. The **trusted service** is also responsible for storage and availability of issued Permit Envelopes, because delegated-claim validity depends on them.

Regardless of the **trusted service**, the user always keeps the ability to sign and publish Envelopes without it, by direct private key injection into a **client app** or **browser extension**.

## Authorization for Casual Identity (low-friction entry; not for security-critical usage)

1. An unauthorized user enters login, password, and birth date in a **trusted client app** and generates a private/public key pair.
2. The user encrypts their private key using their password as a decryption phrase and publishes the following Envelope to the network:

```json
{
    "@context": "https://schema.org",
    "@type": "DigitalDocument",
    "@id": "atlas:<Argon2id(login, salt = H(\"atlas-id\" || birth-date[YYYY-MM-DD]))>",
    "dateCreated": "2026-02-03T00:00:00Z",
    "text": "<XChaCha20-Poly1305 AEAD encryption of a private-key blob using a key deterministically derived from Argon2id(password)>"
}
```

3. When the user enters login, password, and birth date in any client app, it SHOULD deterministically derive the Envelope `@id` from login and birth date using Argon2id, fetch the Envelope, then derive a decryption key from password using Argon2id and decrypt/authenticate the `text` field with XChaCha20-Poly1305 to recover and store the private key locally.

## Authorization via Trusted Client Apps (recommended for technical users)

Onboarding of technical users into Atlas does not rely on a convenient **trusted service**. The user retains full custody and responsibility. Technical users trade convenience and **trusted service**-assisted recovery for full sovereignty and minimal delegated trust.

1. The user generates a private/public key pair in an environment they control (i.e. CLI, desktop app, hardware device). No third party ever sees the raw private key.
2. The user stores the private key themselves (i.e. file, vault, HSM). No default "convenience" backup on a Node.
3. The user SHOULD publish a Person Envelope with the metadata they wish to expose (i.e. public key, auth hints) to establish their network presence.

## Suspicious Activity Challenge

Nodes MAY require an additional trust proof when suspicious activity is detected. The challenge flow begins with the Node rejecting the request with `401 Unauthorized` and returning the following headers:

- **Header**: `X-Atlas-Challenge: <base64(random bytes)>`
- **Header**: `X-Atlas-Expires: <unit timestamp milliseconds>`
- **Header**: `X-Atlas-Authorities: <comma-separated list of authority URLs>`

Example:

```
Response:
  401 Unauthorized
  X-Atlas-Challenge: base64(random bytes)
  X-Atlas-Expires: 1770065816302
  X-Atlas-Authorities: https://authority.example.com,https://other-authority.example.com
```

On the next request, a rejected client (if the user is trusted by any of the listed authorities) SHOULD include:

- **Header**: `X-Atlas-Public-Key: <Public key>`
- **Header**: `X-Atlas-Signature: <Signature over challenge verifiable against X-Atlas-Public-Key>`
- **Header**: `X-Atlas-ClaimReview: <base64(Canonical envelope JSON)>`

Example:

```
GET /envelopes
Request:
  X-Atlas-Public-Key: <Public key>
  X-Atlas-Signature: <Signature over challenge>
  X-Atlas-Trusted-By: https://other-authority.example.com
  X-Atlas-ClaimReview: { Envelope with ClaimReview }
```

Then the Node verifies:

- **Challenge validity**: if `X-Atlas-Expires` has not expired.
- **Authority selection**: if `X-Atlas-Trusted-By` is one of the URLs listed in `X-Atlas-Authorities` (trusted by node).
- **Signature validity**: `X-Atlas-Signature` is a valid signature by `X-Atlas-Public-Key` over the challenged bytes (`X-Atlas-Challenge` value).

If verification fails, the Node rejects the request with `401 Unauthorized` and includes a new `X-Atlas-Challenge`.

If verification passes, the Node verifies the signature on the ClaimReview against the public key of the trusted authority indicated by `X-Atlas-Trusted-By`, ensures the ClaimReview is time-valid (`validUntil` in the future and `datePublished` not in the future relative to Node time), and ensures that the ClaimReview asserts trust for the provided `X-Atlas-Public-Key`.

If all checks succeed, the Node accepts the request.

Example ClaimReview (data field inside Envelope):

```json
{
    "@context": "https://schema.org",
    "@type": "ClaimReview",
    "claimReviewed": "atlas:pubkey:npub1xyz...",
    "reviewRating": {
        "@type": "Rating",
        "ratingValue": "1",
        "bestRating": "1",
        "worstRating": "0"
    },
    "author": {
        "@type": "Organization",
        "name": "Example Authority",
        "url": "https://authority.example.com"
    },
    "datePublished": "2026-02-02T12:00:00Z",
    "validUntil": "2026-02-02T13:00:00Z"
}
```
