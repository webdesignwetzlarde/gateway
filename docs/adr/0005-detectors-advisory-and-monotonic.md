# ADR 0005: Detectors are advisory and monotonic

- Status: accepted
- Date: 2026-09-25

## Context

Published prompt-injection detectors are bypassed at high rates by adaptive attackers
([Nasr, Carlini et al., 2025](https://arxiv.org/abs/2510.09023)). Yet detectors still add value by
catching unsophisticated attacks and by informing labels.

## Decision

- Detectors produce findings, never security decisions on their own.
- Detectors are **monotonic**: they can only lower the integrity of content, never raise it, and a
  detector can never unblock anything.
- Detectors run as sandboxed WebAssembly components with a versioned interface and fuel limits.
- Detector evasions are not vulnerabilities; they become public corpus cases.

## Consequences

- A bypassed detector cannot turn untrusted content into trusted content.
- Vulnerability reports focus on guarantees, keeping the security process manageable.
- Third parties can ship detectors without being part of the trusted computing base.
