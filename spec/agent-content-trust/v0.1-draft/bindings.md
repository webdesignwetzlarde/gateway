# Bindings, v0.1 (draft)

How labels and receipts travel with content. Labels always travel **out of band**: a label found
inside content bytes is content, not a label.

## 1. HTTP

### Label field

The label is carried in the `Agent-Content-Trust` response field, a Structured Field Dictionary
([RFC 9651](https://www.rfc-editor.org/rfc/rfc9651)):

| Key | Label field | Value type |
|---|---|---|
| `v` | `v` | String |
| `i` | `integrity` | Token |
| `c` | `confidentiality` | Token |
| `f` | `form` | Token |
| `r` | `receipt.digest` | Byte sequence of the raw digest; the algorithm is given by the `ra` key |
| `ra` | digest algorithm | Token, default `sha256` |
| `n` | total number of findings | Integer |

Example:

```http
Agent-Content-Trust: v="0.1", i=suspect, c=public, f=reconstructed, n=4, r=:mADhBFbeTdLDc7Qn9GxAiSvtTJb0NDRZrMKVkp+BOhg=:
Link: <http://127.0.0.1:7878/v1/receipts/9800e10456de4dd2>; rel="agent-content-receipt"
```

The full JSON label and the receipt envelope are available at the linked URI.

### Receipt link

The receipt is referenced with a `Link` field ([RFC 8288](https://www.rfc-editor.org/rfc/rfc8288))
using the relation type `agent-content-receipt`.

## 2. Model Context Protocol

- Tool results that return content produced by a producer carry the full JSON label in the result's
  `_meta` object under the key `<reverse-spec-domain>/act.label` (placeholder until the domain is
  registered). The receipt digest and URI are part of the label.
- Implementations advertise support through the MCP extensions mechanism of the
  [2026-07-28 specification](https://blog.modelcontextprotocol.io/posts/2026-07-28/) under an
  extension identifier to be registered.
- The label is intended to serve as evidence for trust and sensitivity annotations such as those
  proposed in [SEP-1913](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/1913).

## 3. Agent2Agent

- Message parts carry the label in part metadata under the key `agent-content-trust`.
- Agents that produce labels declare it through an Agent Card extension (identifier to be defined).

## 4. OpenTelemetry

Proposed attributes (namespace `agent_content_trust`), aligned with the
[GenAI semantic conventions](https://github.com/open-telemetry/semantic-conventions-genai):

| Attribute | Type | Meaning |
|---|---|---|
| `agent_content_trust.integrity` | string | Label integrity |
| `agent_content_trust.confidentiality` | string | Label confidentiality |
| `agent_content_trust.form` | string | Label form |
| `agent_content_trust.findings.count` | int | Total findings |
| `agent_content_trust.receipt.digest` | string | Receipt digest |
| `agent_content_trust.policy.digest` | string | Policy digest |

Source URIs SHOULD NOT be recorded as attributes by default (privacy).

## 5. Dataset manifests

For crawls and training datasets, one JSON object per line
([JSON Lines](https://jsonlines.org/)):

```json
{"uri": "https://example.com/a", "retrievedAt": "2026-09-25T09:14:03Z", "rawDigest": "sha256:...", "irDigest": "sha256:...", "outputDigest": "sha256:...", "receiptDigest": "sha256:...", "usage": "train-ai=n"}
```

A manifest can be referenced from a [CycloneDX](https://cyclonedx.org/) machine-learning bill of
materials. Re-fetching a URI and comparing `rawDigest` detects content that changed after collection
(split-view poisoning, [Carlini et al., 2023](https://arxiv.org/abs/2302.10149)).
