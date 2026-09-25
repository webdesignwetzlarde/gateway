# Design principles

These principles decide trade-offs. When a design choice conflicts with a principle, the principle
wins unless an [ADR](adr/) explicitly records an exception.

## P1: The boundary is architecture, not classification

Guarantees come only from three mechanisms:

1. **What can reach the model:** the closed intermediate representation (IR) and typed schemas.
2. **What the agent may do:** capabilities and egress policy.
3. **Isolation:** parsers and renderers run in sandboxes with no network access of their own.

Detectors are advisory. Kopist assumes that every detector can be bypassed by an adaptive attacker
([Nasr, Carlini et al., 2025](https://arxiv.org/abs/2510.09023)).

## P2: Reconstruct, don't filter

Content is parsed into the IR and re-emitted from it in a canonical format. Anything the IR cannot
represent is dropped. Original bytes are never passed through in reconstruction profiles. This is
content disarm and reconstruction (CDR) applied to agents.

## P3: Typed extraction beats prose

When a task allows it, the agent receives enums, numbers, dates, allowlisted URLs and bounded
strings instead of free text. Free text is always labeled as untrusted prose and taints whatever it
flows into. This follows the capability and data-flow ideas of CaMeL
([Debenedetti et al., 2025](https://arxiv.org/abs/2503.18813)) and the design patterns of
[Beurer-Kellner et al., 2025](https://arxiv.org/abs/2506.08837).

## P4: Provenance everywhere

Every artifact records its source, fetch context, hashes (raw, IR, output), applied transforms,
findings and policy. Labels are sticky: when content is combined, integrity takes the minimum and
confidentiality the maximum.

## P5: Egress is denied by default

Every outbound path passes one guard: HTTP requests, tool calls, rendered markdown, link previews and
writes to agent configuration. Meta's "Rule of Two"
([Meta, 2025-10-31](https://ai.meta.com/blog/practical-ai-agent-security/)) is available as an
enforcement mode.

## P6: Local-first and sovereign

One binary, listening on `127.0.0.1` or a Unix socket by default. No telemetry. Models are optional
and run locally or at an endpoint the user chooses.

## P7: Model and protocol agnostic

Kopist speaks MCP, HTTP and, later, Agent2Agent, and offers SDKs. A thin "model binding" layer adapts
output to each model's best channel for untrusted data.

## P8: Fail closed, degrade gracefully

On any error the consumer receives a labeled stub, never raw bytes. Profiles (`relaxed`, `standard`,
`strict`, `paranoid`) make the trade-off between utility and safety explicit.

## P9: Crypto agility

Algorithm identifiers everywhere, multiple signatures per receipt, key rotation, and a published
post-quantum migration timeline ([ADR 0009](adr/0009-crypto-agility-and-pqc.md)).

## P10: Determinism

The same input, pipeline version and policy produce the same output bytes and the same receipt body.
This enables caching, auditing, dataset integrity and differential testing.

## P11: Explain every change

Every modification is traceable to a receipt entry. `kopist why` explains it, and a diff view shows
"what the agent sees versus what you see". Explainability is the main tool against false positives.

## P12: Small trusted computing base

The trusted computing base (TCB) is limited to IR types and validator, Unicode normalizer, emitters,
policy engine, egress guard and receipt code. Parsers, renderers, detectors and quarantined models are
untrusted and isolated.

## P13: Measure, don't claim

Every protection claim is backed by the public benchmark, reporting both attack success and task
utility, including adaptive attacks ([evaluation method](evaluation.md)).

## P14: Annotate, don't amputate

Only guarantee-level reconstruction is enforced by default. Advisory findings label content rather
than delete it, so legitimate content survives and users stay in control.
