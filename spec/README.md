# Agent Content Trust specification

Agent Content Trust defines **trust labels** and **content receipts** for content delivered to AI
agents: where it came from, how it was processed, what was found, and how much it can be trusted.
Any tool can produce or verify them; Kopist is one implementation.

## Versions

| Version | Status | Document |
|---|---|---|
| v0.1-draft | Draft for public review | [specification](agent-content-trust/v0.1-draft/specification.md), [taxonomy](agent-content-trust/v0.1-draft/taxonomy.md), [bindings](agent-content-trust/v0.1-draft/bindings.md) |

Machine-readable parts of each version:

- JSON Schemas in `schemas/` (JSON Schema 2020-12);
- examples in `examples/`, validated against the schemas in CI.

## Scope

In scope: labels and receipts for content that an agent receives (provenance, transformations,
findings, trust level) and the rules for combining labels.

Out of scope, referenced instead:

- media provenance at creation time ([C2PA](https://c2pa.org/));
- authorization and delegation of agents;
- attestation of whole agent runs.

## Licensing

- Specification prose (`*.md`): CC-BY-4.0.
- Schemas, examples and test vectors: Apache-2.0.

Before version 1.0, the project will adopt a patent policy for specification contributions so that
independent implementations are safe to build (see [PROCESS.md](PROCESS.md)).

## Placeholders

Until the project domain is registered, permanent identifiers use the reserved domain
`spec-domain.invalid` ([RFC 2606](https://www.rfc-editor.org/rfc/rfc2606)) as a placeholder.
These identifiers change before v0.1 is finalized ([ADR 0010](../docs/adr/0010-project-name.md)).

## Contributing

See [PROCESS.md](PROCESS.md). Implementers and reviewers from other projects are explicitly invited to
become co-editors.
