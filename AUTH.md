# Atlas - Convenient & Interoperable Authorization

## Authorization via trusted service (recommended for most of users)

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

### Client-app authorization

1. Unauthorized user chooses his **trusted service** button and gets redirected to **trusted client app** (where he is authorized), on `/atlas/delegate?url=https://app.example.com&p=[{"@type": "CreateAction","object": {"@type": "SocialMediaPosting"}}]`.

2. In **trusted client app** user sets time scope (validFrom and validUntil) and obtains short-lived, secret url to copy from **trusted service**.

3. **Trusted service** uses authorization material from user to prepare (not yet publish) delegated private key Permit envelope:

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

5. User goes back to unauthorized **client app** and inputs short-lived url.
6. **Client app** sends GET on the url to request **trusted service** (responding only to proper Origin header, no-store, no-cache, max-age=0), in return receiving delegated private-key. Short-lived url immediately stops working.
7. Upon GET request, **trusted service** publishes prepared Permit envelope to the network, which validates the delegation across the Nodes. **Trusted service** is also responsible for storage and availability of issued Permit envelopes, as recognition of validity of delegated-claims depends on them.

Regardless of using **trusted service**, user always keeps ability to sign and publish Envelopes without it, by direct private key injection into **client app** or **browser extension**.

## Authorization for casual identity (low-friction entry; not for usage, where security matters)

1. Unauthorized user inserts login, password and birth date in **trusted client app** and generates a private/public key pair.
2. User encrypts his private key using his password as decrypt phrase and publishes following Envelope to the network:

```json
{
    "@context": "https://schema.org",
    "@type": "DigitalDocument",
    "@id": "atlas:<Argon2id(login, salt = H(\"atlas-id\" || birth-date[YYYY-MM-DD]))>",
    "dateCreated": "2026-02-03T00:00:00Z",
    "text": "<XChaCha20-Poly1305 AEAD encryption of a private-key blob using a key deterministically derived from Argon2id(password)>"
}
```

3. When the user enters login, password, and birth date in any client app, it SHOULD deterministically derive the Envelope @id from the login and birth date using Argon2id, fetch the Envelope, then derive a decryption key from password using Argon2id and decrypt/authenticate the "text" field with XChaCha20-Poly1305 to recover and store the private key locally.

## Authorization via trusted client apps (recommended for technical users)

Onboarding of technical users into Atlas does not rely on a convenient **trusted service**. The user retains full custody and responsibility. Technical users trade convenience and **trusted service** assisted recovery for a full sovereignty and minimal delegated trust.

1. User generates a private/public key-pair in an environment they control (i.e. CLI, desktop app, hardware device). No third party ever sees the raw private key.
2. User stores the private key themselves (i.e. file, vault, HSM). No default "convenience" backup on a Node.
3. User SHOULD publish a Person envelope with the metadata they wish to expose (i.e. public key, auth hints) to establish their network presence.

## Suspicious activity challenging

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

On the next request, a rejected Client (if the user is trusted by any of the listed authorities) SHOULD include:

- **Header**: `X-Atlas-PublicKey: <Public key>`
- **Header**: `X-Atlas-Signature: <Signature over challenge verifiable against X-Atlas-PublicKey>`
- **Header**: `X-Atlas-ClaimReview: <base54(Canonical envelope JSON)>`

Example:

```
GET /envelopes
Request:
  X-Atlas-PublicKey: <Public key>
  X-Atlas-Signature: <Signature over challenge>
  X-Atlas-Trusted-By: https://other-authority.example.com
  X-Atlas-ClaimReview: { Envelope with ClaimReview }
```

Then Node verifies:

- **Challenge validity**: if `X-Atlas-Expires` has not expired.
- **Authority selection**: if `X-Atlas-Trusted-By` is one of the URLs listed in `X-Atlas-Authorities` (trusted by node).
- **Signature validity**: `X-Atlas-Signature` is a valid signature by `X-Atlas-PublicKey` over the challenged bytes (`X-Atlas-Challenge` value).

If verification fails, the Node rejects the request with `401 Unauthorized` and include a new `X-Atlas-Challenge`.

If verification passes, the Node verifies the signature on the ClaimReview against the public key of the trusted authority indicated by `X-Atlas-Trusted-By`, ensure the ClaimReview is time-valid (`validUntil` in the future and `datePublished` not in the future relative to Node time), and ensure that the ClaimReview asserts trust for the provided `X-Atlas-PublicKey`.

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
