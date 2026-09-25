# Guarantee registry

This registry defines what Kopist promises. A violation of a listed guarantee in a released version is
a vulnerability ([SECURITY.md](../../SECURITY.md)). Anything not listed is best effort.

Status values: **planned** (designed, not implemented), **implemented** (code exists, tests pass),
**verified** (covered by corpus gates and fuzzing, reviewed externally).

All guarantees apply to reconstruction profiles (`standard`, `strict`, `paranoid`) unless stated
otherwise. As of 2026-09, no code exists, so every guarantee is **planned**.

| ID | Guarantee | Profiles | Attack classes | Status |
|---|---|---|---|---|
| G-01 | No passthrough: output is emitted only from validated IR. Original bytes never reach the consumer | all reconstruction profiles | all | planned |
| G-02 | Unicode: output contains no code points from the removal set (Tags block, bidirectional controls, private use, noncharacters, C0 and C1 controls except tab and newline, zero-width characters outside allowed script contexts) | all | AC-02 | planned |
| G-03 | HTML: no content from scripts, styles, comments, forms, iframes or embedded objects appears as text | all | AC-01 | planned |
| G-04 | HTML: text classified as hidden by the recorded visibility method is not emitted as ordinary text | `strict`, `paranoid` | AC-01 | planned |
| G-05 | Output syntax: Clean Markdown output contains no raw HTML, no inline image syntax and no autolinks; links appear only as references with normalized URLs | all | AC-12 | planned |
| G-06 | URLs: fragments are removed before content or URLs reach the consumer; only allowlisted schemes are emitted | all | AC-03 | planned |
| G-07 | Images: emitted images are re-encoded from decoded pixels at the consumer's resolution and carry no metadata | all | AC-06, AC-11 | planned |
| G-08 | PDF: no JavaScript, embedded files, XFA or annotations are carried over; text in invisible render mode is not emitted as ordinary text | all | AC-09 | planned |
| G-09 | Receipts: every output has a receipt; verification fails if output, label or receipt was modified | all | AC-21 | planned |
| G-10 | Fetcher: no requests to loopback, private, link-local or cloud metadata addresses unless explicitly allowed; resolved addresses are pinned | all | AC-20 | planned |
| G-11 | Limits: size, time, depth and decompression-ratio limits are enforced for every input | all | AC-22 | planned |
| G-12 | Isolation: every parser and renderer runs in its declared tier without network access of its own | all | AC-19 | planned |
| G-13 | MCP definitions: a change to a pinned tool definition is never accepted silently | `standard` and above, MCP proxy | AC-15 | planned (Phase 2) |
| G-14 | Egress: under default-deny, no request leaves for an origin outside the session's origin set without an explicit decision | `standard` and above, covered channels | AC-12, AC-13, AC-14 | planned (Phase 2) |
| G-15 | Determinism: identical input, version and policy produce identical output bytes and receipt body | all | integrity | planned |
| G-16 | Dataset integrity: a manifest entry verifies only against content with the recorded digest | crawler mode | AC-18 | planned |

## Explicit non-guarantees

- Detection of instruction-like text, cloaking, steganography or malicious skills. These are advisory.
- Safety of visible content. Visible instructions remain in the output, labeled.
- Protection of channels that do not pass through Kopist.
- Correctness of the consumer model's decisions.

## Changing this registry

Adding a guarantee requires corpus cases that exercise it and a CI gate. Removing or weakening a
guarantee requires an ADR and a changelog entry.
