# Supply-chain security plan

Kopist asks users to trust it with hostile content and with their agents' actions. Its own supply
chain must therefore be at least as rigorous as what it asks of others.

## Phase 0 (design phase, now)

- GitHub Actions pinned to full commit SHAs with version comments; Dependabot updates them.
- Workflows start from `permissions: {}` and grant per-job permissions only.
- `actions/checkout` with `persist-credentials: false`.
- [actionlint](https://github.com/rhysd/actionlint) and [zizmor](https://github.com/zizmorcore/zizmor)
  check every workflow change.
- [Harden-Runner](https://github.com/step-security/harden-runner) in audit mode.
- [OpenSSF Scorecard](https://github.com/ossf/scorecard) runs on every push to `main` and weekly.
- [REUSE](https://reuse.software/) compliance for every file.
- Machine-readable security metadata in
  [security-insights.yml](../../security-insights.yml)
  ([OpenSSF Security Insights](https://github.com/ossf/security-insights)).
- Squash merges only, signed tags, two-factor authentication for all maintainers.
- Target: [OpenSSF Best Practices](https://www.bestpractices.dev/) "passing" and
  [OSPS Baseline](https://baseline.openssf.org/) level 1.

## Phase 1 (first code)

- `cargo-deny` (licenses, advisories, sources), `cargo-audit`, `cargo-vet` with imported audits.
- `cargo-auditable` for release binaries.
- Fuzzing with cargo-fuzz and [ClusterFuzzLite](https://google.github.io/clusterfuzzlite/) in CI.
- Trusted publishing to crates.io, PyPI and npm with provenance.
- Build provenance attestations and keyless signing with
  [Sigstore](https://www.sigstore.dev/) for binaries and container images.
- Software bill of materials in [CycloneDX](https://cyclonedx.org/) format for every release.

## Phase 2

- OSPS Baseline level 2, CodeQL for Rust.
- Model signing and a machine-learning bill of materials for any detector model shipped.
- Harden-Runner in block mode: continuous integration itself runs under default-deny egress.

## Phase 3

- [SLSA](https://slsa.dev/) Build Level 3 through reusable, isolated build workflows.
- Reproducible builds, verified by an independent rebuilder.
- Vulnerability Exploitability eXchange (VEX) statements.
- OSPS Baseline level 3, OpenSSF Best Practices silver.

## Keys

- Release signing uses keyless Sigstore identities bound to the release workflow.
- Receipt signing keys are generated locally by each Kopist installation; the project never holds
  users' keys.
