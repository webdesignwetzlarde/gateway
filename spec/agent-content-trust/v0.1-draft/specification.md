# Agent Content Trust, version 0.1 (draft)

- Status: **draft for public review**, 2026-09-25
- Editors: The Kopist Authors (further editors invited)
- License: CC-BY-4.0 (prose); schemas and examples Apache-2.0

## 1. Introduction

AI agents act on content they did not author. Today, nothing that travels with such content tells an
agent, its framework or an auditor where the content came from, what was done to it, and how much it
should be trusted. Protocols for agents, including the Model Context Protocol, have no field for it.

This specification defines:

1. a compact **trust label** that accompanies content delivered to an agent;
2. a signed, offline-verifiable **content receipt** that records how the content was obtained and
   processed;
3. **label flow rules** for combining labels;
4. a **findings taxonomy** ([taxonomy.md](taxonomy.md));
5. **bindings** for HTTP, MCP, Agent2Agent, OpenTelemetry and dataset manifests
   ([bindings.md](bindings.md)).

## 2. Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT",
"RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as
described in BCP 14 ([RFC 2119](https://www.rfc-editor.org/rfc/rfc2119),
[RFC 8174](https://www.rfc-editor.org/rfc/rfc8174)) when, and only when, they appear in all capitals.

Terms:

- **Producer:** software that processes content and issues labels and receipts (for example Kopist).
- **Consumer:** software that receives labeled content (an agent, a framework, a pipeline).
- **Verifier:** software that checks receipts.
- **Output:** the content bytes the producer delivers to the consumer.

Draft identifiers use the placeholder domain `spec-domain.invalid`.

## 3. Trust label

### 3.1 Fields

| Field | Type | Required | Meaning |
|---|---|---|---|
| `v` | string | yes | Label version, `"0.1"` |
| `src` | string (URI) | yes | Canonical source URI without fragment. Uploads and tool results use URNs or tool URIs |
| `integrity` | enum | yes | `trusted`, `endorsed`, `untrusted`, `suspect`, `quarantined` |
| `confidentiality` | enum | yes | `public`, `internal`, `confidential`, `secret` |
| `form` | enum | yes | `raw`, `reconstructed`, `typed`, `summary` |
| `findings` | object | no | Map from finding category to count |
| `usage` | string | no | Usage preference signal observed at the source, copied verbatim (for example from an IETF AIPREF `Content-Usage` field) |
| `receipt` | object | yes | `digest` of the receipt envelope and optional `uri` |
| `issuer` | string | no | Key identifier of the producer (section 6) |
| `crit` | array of strings | no | Names of extension fields the consumer must understand |

The label schema is [schemas/label.schema.json](schemas/label.schema.json).

### 3.2 Semantics of `integrity`

Ordered from highest to lowest:

1. `trusted`: originates from the principal the agent works for (user or operator). A producer MUST
   NOT assign `trusted` to fetched or received third-party content.
2. `endorsed`: third-party content that an explicit, recorded policy decision raised above
   `untrusted` (for example a verified signature from a publisher on an allowlist).
3. `untrusted`: default for all third-party content.
4. `suspect`: findings indicate manipulation attempts.
5. `quarantined`: the content must not be shown to a privileged model; it is available only through
   typed extraction or to humans.

### 3.3 Semantics of `form`

- `raw`: original bytes. Producers SHOULD NOT deliver raw content to agents.
- `reconstructed`: re-emitted from an intermediate representation.
- `typed`: schema-validated values produced from the content (for example by a quarantined reader).
- `summary`: free text produced by a model from the content.

### 3.4 Constraints

- The serialized JSON label MUST NOT exceed 1024 bytes.
- `src` MUST NOT contain a fragment.
- A consumer that receives content without a label MUST treat it as `untrusted`, `raw`.

## 4. Content receipt

### 4.1 Structure

A receipt is an [in-toto Statement v1](https://github.com/in-toto/attestation/blob/main/spec/v1/statement.md)
with:

- `subject`: at least the output, named `output`, with its digest set; optionally the intermediate
  representation, named `ir`;
- `predicateType`: `https://spec-domain.invalid/agent-content-trust/receipt/v0.1-draft`;
- `predicate`: the fields in section 4.2.

The statement is wrapped in a [DSSE](https://github.com/secure-systems-lab/dsse) envelope with payload
type `application/vnd.in-toto+json`. The schema for the unsigned statement is
[schemas/receipt.schema.json](schemas/receipt.schema.json).

### 4.2 Predicate fields

| Field | Required | Meaning |
|---|---|---|
| `actVersion` | yes | `"0.1-draft"` |
| `label` | yes | The trust label delivered with the output, without its `receipt` member (see 4.3) |
| `sources` | yes | One entry per fetch view or input: `uri`, `retrievedAt`, `view` (`declared`, `browser`, `upload`, `mcp`, `a2a`), `status`, `contentType`, `digest`, optional `redirects`, `tlsSpkiDigest`, `identityKeyId` |
| `usageDecision` | no | `mode` (`agent` or `crawler`), `robots` (`allow`, `disallow`, `absent`), `signal` (verbatim), `outcome` (`allow` or `deny`) |
| `pipeline` | yes | `implementation` (name, version), `profile`, `policyDigest`, `isolationTier` (`T0` to `T3`), `components` (name, version) |
| `transforms` | yes | Ordered list of operations: `op`, `count`, optional `detail` |
| `findings` | yes | List of findings: `category`, `detector` (id, version), `score` from 0 to 1, optional `taxonomy` references and `spans` |
| `irDigest` | no | Digest of the canonical intermediate representation |
| `blocksMerkleRoot` | no | Merkle root over output blocks, for chunk-level proofs |
| `upstreamProvenance` | no | Results of checking provenance present in the source: `type` (`c2pa`, `sri`, `signature`), `result` (`valid`, `invalid`, `absent`) |
| `parents` | no | Digests of receipts this output was derived from |
| `crit` | no | Extension fields that verifiers must understand |

### 4.3 Digests and canonicalization

- Digests use the form `<algorithm>:<lowercase hex>` in labels and in-toto digest sets (algorithm
  name as key) in statements.
- JSON is canonicalized with the JSON Canonicalization Scheme
  ([RFC 8785](https://www.rfc-editor.org/rfc/rfc8785)) before hashing.
- The label's `receipt` member can only be computed after the envelope is signed, so the predicate
  carries the label without it. Verifiers compare the delivered label, minus `receipt`, with
  `predicate.label`, and check that `receipt.digest` matches the envelope.

### 4.4 Processing rules

Producers:

- MUST record every transformation that removes or alters content.
- MUST record the policy digest and isolation tier used.
- MUST NOT record secrets or credentials in receipts.
- SHOULD produce deterministic receipt bodies for identical inputs, versions and policies, apart from
  timestamps.

Verifiers:

- MUST verify at least one signature from a trusted key before relying on any receipt content.
- MUST verify that the output digest matches the delivered output.
- MUST ignore unknown fields, except fields named in `crit`, which cause rejection if not understood.
- MUST report unknown signature algorithms instead of treating them as valid.

## 5. Label flow rules

When outputs are combined (for example, concatenated into one context or summarized together):

1. **Integrity** of the result is the lowest integrity of the inputs.
2. **Confidentiality** of the result is the highest confidentiality of the inputs.
3. Detectors and findings MAY lower integrity. They MUST NOT raise it.
4. Raising integrity (endorsement) MUST be the result of an explicit policy rule, and the receipt MUST
   contain a transform with `op` set to `endorse` and the policy digest.
5. Typed extraction keeps the integrity of its input. A consumer MAY allow low-entropy typed values
   (enumerations, bounded numbers, dates) to influence decisions that untrusted prose may not
   influence; this is a consumer policy decision.
6. A derived output MUST list the receipts of its inputs in `parents`.

## 6. Keys and algorithms

Key identifiers have the form `<algorithm>:<lowercase hex SHA-256 of the public key encoding>`.

| Identifier | Algorithm | Status in v0.1 |
|---|---|---|
| `ed25519` | Ed25519 ([RFC 8032](https://www.rfc-editor.org/rfc/rfc8032)) | REQUIRED for verifiers |
| `ecdsa-p256-sha256` | ECDSA with P-256 and SHA-256 | OPTIONAL |
| `ml-dsa-65` | ML-DSA-65 ([FIPS 204](https://csrc.nist.gov/pubs/fips/204/final)) | RECOMMENDED, experimental |
| `slh-dsa-sha2-128s` | SLH-DSA ([FIPS 205](https://csrc.nist.gov/pubs/fips/205/final)) | OPTIONAL, for long-term roots |

Hash algorithms: `sha256` (REQUIRED), `sha384`, `sha512`, `sha3-256`, `blake3` (OPTIONAL).

A DSSE envelope MAY carry several signatures. Producers SHOULD add an `ml-dsa-65` signature alongside
a classical signature. The migration timeline is described in the Kopist
[ADR 0009](../../../docs/adr/0009-crypto-agility-and-pqc.md).

## 7. Security considerations

- **Labels are claims.** A label is only as trustworthy as its issuer's key. Consumers decide which
  issuers they trust; in the common case this is the local Kopist instance.
- **Stripping.** An attacker who removes labels gains nothing, because unlabeled content is treated
  as `untrusted`, `raw` (section 3.4).
- **Forgery.** Content cannot claim its own label: labels travel out of band (headers, metadata
  fields), and any label inside content bytes is content, not a label.
- **Replay.** A receipt binds the output digest; replaying it with other content fails verification.
- **Downgrade.** Verifiers must not accept a weaker algorithm set than their policy requires.
- **Findings are not guarantees.** An empty `findings` list does not mean content is safe.

## 8. Privacy considerations

Receipts contain source URIs, timestamps and possibly identities used for fetching. Producers SHOULD
offer a privacy mode in which `src` and `sources[].uri` are replaced by salted digests
(`urn:act:hashed:<algorithm>:<hex>`), and SHOULD NOT store content in logs.

## 9. IANA considerations (future)

Before a standards-track submission, registrations are expected for:

- the HTTP field name `Agent-Content-Trust`;
- the link relation type `agent-content-receipt`;
- a media type for receipt envelopes.

## 10. Open issues

1. Whether `form` needs a value for content produced by rendering (for example OCR of a screenshot).
2. Batching of receipts for high-volume crawls.
3. Alignment with MCP trust annotations proposed in
   [SEP-1913](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/1913).
4. A compact COSE binding and its CBOR schema.
5. The patent policy for specification contributions.

## Appendix A: Examples

- [examples/label.example.json](examples/label.example.json)
- [examples/receipt.unsigned.example.json](examples/receipt.unsigned.example.json)
