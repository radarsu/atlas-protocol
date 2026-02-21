# Atlas - Keys

## Algorithm

**Ed25519**, 32-byte keys. No seed phrases; keys are generated randomly and then bound to a KeyPoW proof.

## Canonical key format

Keys are encoded as **Bech32**: `<HRP> "1" <base32-data> <6-char BCH checksum>`.

| Key type    | HRP    | Canonical form |
| ----------- | ------ | -------------- |
| Public key  | `apub` | `apub1...`     |
| Private key | `asec` | `asec1...`     |

Normalization also accepts compact aliases that omit the Bech32 `1` separator:

- `apub...` for public keys
- `asec...` for private keys

## KeyPoW format

Public keys may include KeyPoW proof data:

`<apub1...>.<powNonce>.<powHash>`

- `powNonce` is 16 random bytes encoded as 32 hex chars.
- `powHash` is an Argon2id hash string.
- `powNonce` and `powHash` MUST be provided together (or both omitted).

Tier is derived from PoW parameters, not from base64/public-key prefixes.

## Tiers (KeyPoW profiles)

| Tier ID | Name      | Argon2 memoryCost | Argon2 timeCost | Difficulty target | Estimated generation time* |
| ------- | --------- | ----------------- | --------------- | ----------------- | -------------------------- |
| 1       | Citizen   | `2048 * 1024` KiB | `1`             | 4 leading zero bits | Seconds to ~1 minute |
| 2       | Titan     | `4096 * 1024` KiB | `2`             | 10 leading zero bits | Minutes to a few hours |
| 3       | Atlas     | `8192 * 1024` KiB | `4`             | 14 leading zero bits | Several hours to ~1 day |

These are minimum thresholds. A key is valid if it meets at least Citizen thresholds; it does not need to match an exact predefined profile.

Tier labeling is determined by the highest threshold satisfied (Atlas > Titan > Citizen).

Generation presets still use the table above and reject a selected preset if host RAM is below that preset memory requirement.

\* Estimates are approximate and hardware-dependent (CPU, memory bandwidth, thermal limits, and parallelism).

## Generation and verification rules

1. Generate a random 32-byte private key.
2. Derive Ed25519 public key from that private key.
3. Compute PoW over `keypow-v1|<rawPublicKey>|<powNonce>`.
4. Repeat nonce search until Argon2id output satisfies the tier difficulty.

PoW is valid only if all checks pass:

- Argon2 algorithm is `argon2id`, version `19`, parallelism is at least `1`, hash length is `32`.
- `memoryCost` and `timeCost` meet at least Citizen minimums (`>= 2048*1024`, `>= 1`).
- Hash output meets the required leading-zero-bit target for the claimed/effective tier.
- `argon2.verify(powHash, powInput)` succeeds.

## DER prefixes (internal)

| Key type    | Encoding | Hex prefix                               |
| ----------- | -------- | ---------------------------------------- |
| Public key  | SPKI     | `302a300506032b6570032100`               |
| Private key | PKCS8    | `302e020100300506032b657004220420`       |

Raw 32-byte key material is appended directly after the prefix.

## Backup encryption

Private key backups are encrypted client-side. Format: `atlas-backup-v1.<base64(salt || iv || ciphertext)>`

- KDF: PBKDF2-SHA256, 100,000 iterations, 16-byte salt
- Encryption: AES-256-GCM, 12-byte IV
