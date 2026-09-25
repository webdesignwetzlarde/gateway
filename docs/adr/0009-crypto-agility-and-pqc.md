# ADR 0009: Crypto agility and post-quantum timeline

- Status: accepted
- Date: 2026-09-25

## Context

Receipts and dataset manifests may need to remain verifiable for a decade or longer. NIST published
the first post-quantum signature standards on 2024-08-13 (FIPS 204 ML-DSA, FIPS 205 SLH-DSA) and plans
to deprecate quantum-vulnerable public-key algorithms after 2030 and disallow them after 2035
([NIST IR 8547, initial public draft](https://nvlpubs.nist.gov/nistpubs/ir/2024/NIST.IR.8547.ipd.pdf)).

## Decision

- Every signature and hash carries an algorithm identifier. Receipts can carry several signatures.
- Initial algorithms: Ed25519 (default), ECDSA P-256 (compatibility), ML-DSA-65 (experimental in
  v0.1), SLH-DSA for long-term roots. Hashes: SHA-256 by default; SHA-384, SHA3-256 and BLAKE3
  optional.
- Timeline:
  - 2026 to 2027: hybrid Ed25519 plus ML-DSA-65 available, off by default;
  - 2028: hybrid signatures on by default;
  - 2030: post-quantum-only profile available, classical-only signatures deprecated;
  - by 2035: classical-only signatures removed.
- Long-lived archives are re-sealed with evidence records ([RFC 4998](https://www.rfc-editor.org/rfc/rfc4998)).
- TLS uses hybrid key exchange where the TLS library supports it.

## Consequences

- Algorithm changes do not require a new receipt format.
- Verifiers must handle unknown algorithms by reporting them, not by failing silently.
