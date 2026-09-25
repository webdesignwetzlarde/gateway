# ADR 0008: DSSE with in-toto statements, COSE as compact binding

- Status: proposed
- Date: 2026-09-25

## Context

Content receipts must be verifiable offline, support multiple signatures (classical and
post-quantum), and be compatible with existing supply-chain tooling.

## Proposed decision

- Primary format: an [in-toto Statement v1](https://github.com/in-toto/attestation) with an Agent
  Content Trust predicate, wrapped in a [DSSE](https://github.com/secure-systems-lab/dsse) envelope.
  DSSE supports multiple signatures natively, and Sigstore tooling can verify it.
- Compact binding: COSE_Sign1 for constrained environments and CBOR transport.
- JSON canonicalization for hashing follows [RFC 8785](https://www.rfc-editor.org/rfc/rfc8785).

## Open questions

- Size budget for receipts on high-volume crawls; possible batching with Merkle trees.
- Registration of a media type and of the predicate type URI once the project domain exists.
