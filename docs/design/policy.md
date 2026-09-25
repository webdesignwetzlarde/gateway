# Policy

## Engine

Proposed: [Cedar](https://www.cedarpolicy.com/), a policy language with a Rust implementation, a typed
schema and formal analysis tooling. See [ADR 0007](../adr/0007-policy-engine-cedar.md).

## Model

| Cedar concept | Kopist meaning |
|---|---|
| Principal | Agent and session |
| Action | `fetch`, `extract`, `egress.http`, `egress.mcp_call`, `render.markdown_image`, `write.agent_config`, and more |
| Resource | Origins and content handles, each carrying labels |
| Context | Findings, origin set, Rule-of-Two state, consumer profile |

## Profiles

Profiles are TOML files that compile to Cedar entities and policies.

| Profile | Intent |
|---|---|
| `relaxed` | Reconstruction and labels only; advisory findings never block |
| `standard` | Default. Reconstruction, labels, egress guard for covered channels, selected findings lead to `allow-labeled` or `typed-only` |
| `strict` | All guarantees enforced, hidden content always dropped, prose from suspect sources only through typed extraction |
| `paranoid` | Pixel path for documents, T3 isolation, Rule of Two enforced, human approval for any new egress origin |

## Decision outcomes

- `allow`: emit reconstructed content with its label.
- `allow-labeled`: emit with prominent labels and model-binding wrappers (the model's untrusted-data
  channel, or spotlighting-style marking
  ([Hines et al., 2024](https://arxiv.org/abs/2403.14720)) as a fallback).
- `typed-only`: content is available only through typed extraction.
- `block`: the consumer receives a labeled stub with the reason.

## Operations

- Policies are versioned (semver). Their hash is recorded in every receipt.
- Shadow mode evaluates a new policy alongside the active one and reports differences.
- `kopist policy test` runs policy unit tests.
- Per-domain overrides are supported and recorded in receipts.
