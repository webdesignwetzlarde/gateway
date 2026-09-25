# ADR 0003: Licensing, REUSE and DCO

- Status: accepted
- Date: 2026-09-25

## Context

Kopist should be usable by individuals, companies and other open-source projects, and eligible for
open-source programs that require an OSI-approved license. The specification should be implementable
by anyone, including competitors.

## Decision

- Code, schemas, test vectors and configuration: **Apache-2.0** (OSI-approved, explicit patent
  grant). It is the only license file at the repository root so that license detection works.
- Documentation and specification prose (`docs/`, `spec/**/*.md`): **CC-BY-4.0**.
- Per-file licensing is declared in `REUSE.toml` and checked in CI with `reuse lint`.
- Contributions are accepted under the **Developer Certificate of Origin 1.1** (sign-off), not a CLA.
- Dependencies under AGPL or GPL are not linked into Kopist processes. Allowed dependency licenses:
  Apache-2.0, MIT, BSD, ISC, Zlib, Unicode-3.0; MPL-2.0 only as an unmodified dependency. Enforced by
  `cargo-deny` once code exists.
- No open core: the project will not maintain proprietary editions.

## Consequences

- Broad adoption is possible, including in commercial products.
- AGPL tools such as Dangerzone and Firecrawl can inspire designs but cannot be embedded.
- Relicensing would require consent of all contributors, which protects the community.
