# ADR 0007: Cedar as policy engine

- Status: proposed
- Date: 2026-09-25

## Context

Kopist needs a policy language for fetch, extraction and egress decisions that is analyzable, fast,
embeddable in Rust, and understandable by security teams.

## Options considered

- [Cedar](https://www.cedarpolicy.com/): Rust implementation, typed schemas, formal analysis tooling.
- Open Policy Agent with Rego: widely used, but a separate runtime and a less constrained language.
- A custom rule format: simple at first, but without analysis tools.

## Proposed decision

Use Cedar for all authorization-style decisions. User-facing profiles are TOML files that compile to
Cedar policies. Policy hashes are recorded in receipts.

## Open questions

- Performance with large origin sets and many findings per document.
- How to express data-flow (taint) rules ergonomically.

The decision becomes final after a prototype in Phase 1.
