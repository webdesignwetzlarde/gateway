# ADR 0002: Rust core with Python and TypeScript SDKs

- Status: accepted
- Date: 2026-09-25

## Context

Kopist parses hostile content in many formats. Parser memory-safety bugs are a classic source of
remote code execution. At the same time, most agent frameworks are written in Python or TypeScript.

## Decision

- The core, all parsers written by the project, the receipt library and the CLI are written in Rust.
  `unsafe` code is forbidden by default and allowed only in reviewed, isolated modules.
- The receipt verifier and core types also compile to WebAssembly.
- Python bindings use PyO3 and maturin; TypeScript bindings use napi-rs for Node.js and WebAssembly
  for browsers and edge runtimes.
- Framework adapters are separate packages.

## Consequences

- Memory safety for the trusted computing base and for new parsers.
- Detector plugins and some parsers can run as WebAssembly components with capability sandboxing.
- Contributors need Rust for core work; SDK and adapter work stays accessible in Python and
  TypeScript.
- Third-party parsers written in C or C++ (for example pdfium) run only in isolation tiers T2 or T3.
