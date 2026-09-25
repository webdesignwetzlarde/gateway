# Roadmap

This roadmap spans ten years because the problem does too. Dates are targets, not promises. Each phase
ends with published exit criteria; the next phase starts when they are met.

## Phase 0: Foundation (2026 Q4)

- Concept, threat model, architecture and guarantee registry open for public review.
- Agent Content Trust specification v0.1 draft.
- First crates as a technology preview: Unicode normalizer, receipt library (sign and verify with
  test vectors), `kopist inspect` for HTML in heuristic mode. Fuzzed from the first commit.
- Corpus v0 (about 100 cases) and benchmark skeleton.

Exit criteria: OpenSSF Scorecard 6.5 or higher, OSPS Baseline level 1, OpenSSF Best Practices
"passing", at least three external reviews of the threat model, first external contributors.

## Phase 1: Local intake MVP (2027 H1)

- CLI, local daemon and MCP server.
- HTML (heuristic and rendered), images (scaling defense, metadata removal, OCR labels), PDF
  (structural path and render diff).
- Receipts v0.1 with Ed25519 and experimental ML-DSA.
- Profiles, basic egress controls (SSRF protection, URL policy, markdown image and link-preview
  neutralization).
- Ready-made configurations for common coding agents.
- Python SDK.
- Benchmark v0.1 with about 500 cases and adapters for BrowseSafe-Bench and AgentDojo.

Exit criteria: all guarantee cases pass in the `strict` profile; published end-to-end attack success
with and without Kopist; utility retention of at least 95 %; HTML p50 latency under 100 ms in
heuristic mode.

## Phase 2: Teams and protocols (2027 H2)

- Typed extraction and handles.
- MCP proxy with definition pinning and flow rules.
- Cedar policy engine, full egress guard with origin sets, taint tracking, canaries and the Rule of
  Two.
- Multi-view cloaking detection, Office documents, OpenTelemetry, container images and Helm chart.
- TypeScript SDK.
- Specification v0.2, experimental MCP extension, OpenTelemetry proposal.
- First independent security audit; application to OSS-Fuzz.

Exit criteria: at least two maintainers, public adopters, Scorecard 8 or higher, OSPS Baseline
level 2.

## Phase 3: Stable and neutral (2028)

- Version 1.0 with a stable API and plugin interface.
- Specification v1.0 with a conformance suite and two independent implementations.
- Hybrid post-quantum receipt signatures by default.
- SLSA Build Level 3, reproducible builds, second audit.
- Browser-agent adapters, Agent2Agent proxy, Windows support.
- Move to a neutral foundation; Maintainers Council replaces the project lead.

## Phase 4: Ecosystem (2029 to 2030)

- Audio and video pipelines.
- Memory guard for long-running agents, payment-action policies for agentic commerce.
- Minimization of covert channels between agents.
- Formal verification of the receipt verifier and emitters.
- Post-quantum-only profile (2030).

## Phase 5: Standard infrastructure (2031 to 2033)

- Specification 2.0 on a standards track.
- Labels supported natively by model APIs and agent runtimes.
- Kopist as reference implementation with long-term support releases.

## Phase 6: Sustain (2034 to 2036)

- Classical-only signatures removed, long-lived archives re-sealed.
- Fully community-run.

## How new threats enter the roadmap

Every newly published attack technique becomes a corpus case. If it exploits a hidden channel, it
becomes a guarantee with a CI gate. If it does not, it is tracked as an advisory detection and in the
evaluation reports.
