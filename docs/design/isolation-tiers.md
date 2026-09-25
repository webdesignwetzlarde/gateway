# Isolation tiers

Kopist parses hostile input by design. Parsers and renderers are therefore untrusted and run with the
least privilege their job allows. Policy maps each (format, profile) pair to a tier; the tier used is
recorded in the receipt.

| Tier | Mechanism | Default use |
|---|---|---|
| T0 | Memory-safe Rust in the Kopist process, with resource limits | HTML heuristic mode, JSON |
| T1 | [Wasmtime](https://wasmtime.dev/) component with fuel and memory caps, no filesystem, no network | Image decoding, PDF structure parsing, all detector plugins |
| T2 | Separate process with an operating-system sandbox (Linux: seccomp, Landlock, namespaces; macOS: Seatbelt; Windows: AppContainer, Phase 3); network only through Kopist | Chromium rendering, pdfium rendering, OCR, local quarantined model |
| T3 | [gVisor](https://gvisor.dev/) or [Firecracker](https://firecracker-microvm.github.io/) microVM (Linux servers) | Office documents, pixel reconstruction, profile `paranoid` |

## Rules

- Only validated IR crosses from a tier into the trusted core.
- No tier has its own network access. Renderers receive subresources through Kopist, under the same
  policy as the main fetch.
- Every tier has limits on CPU time, memory, output size and nesting depth.
- A tier failure produces a labeled stub, never a fallback to raw bytes.

## Platform notes

- Linux is the reference platform. T3 requires virtualization support.
- On platforms without a T2 or T3 mechanism, the corresponding formats are refused unless the user
  explicitly lowers the profile, and the receipt records that decision.

## Verification

- Every parser and the IR validator are fuzz targets from their first commit (cargo-fuzz, later
  ClusterFuzzLite and OSS-Fuzz).
- Sandbox policies are tested with deliberate escape attempts in CI.
