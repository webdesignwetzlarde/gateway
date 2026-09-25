# Contributing to Kopist

Thank you for considering a contribution. The project is in its design phase, so the most useful
contributions right now are:

1. **Design review.** Challenge the [threat model](docs/threat-model.md), the
   [architecture](docs/architecture.md) and the [specification draft](spec/README.md).
2. **Attack techniques for the corpus.** Publicly documented techniques that make AI agents misbehave
   through content (use the "attack technique" issue template).
3. **Related work.** Papers, tools and standards we have missed in [docs/landscape.md](docs/landscape.md).
4. **Translations** of the non-normative summaries (see [docs/style-guide.md](docs/style-guide.md)).

## Ground rules

- Be kind and follow the [Code of Conduct](CODE_OF_CONDUCT.md).
- Security vulnerabilities go through [SECURITY.md](SECURITY.md), never public issues.
- Open an issue before a non-trivial pull request, so we can agree on the approach first.
- Large or cross-cutting changes need a design proposal (issue template "design proposal"). Accepted
  proposals are recorded as an [Architecture Decision Record](docs/adr/).

## Developer Certificate of Origin (DCO)

We use the [Developer Certificate of Origin 1.1](https://developercertificate.org/) instead of a CLA.
Every commit must be signed off:

```sh
git commit -s -m "docs: clarify egress guard scope"
```

This adds a `Signed-off-by: Your Name <you@example.com>` trailer. By signing off, you certify that
you wrote the change or otherwise have the right to submit it under the project licenses.

## Licenses of contributions

- Code, schemas, configuration: Apache-2.0.
- Documentation and specification prose (`docs/`, `spec/**/*.md`): CC-BY-4.0.

The mapping is declared in [REUSE.toml](REUSE.toml). Do not add third-party material without a
compatible license and a matching REUSE annotation. Dependencies under AGPL or GPL are not accepted
into Kopist processes (see [ADR 0003](docs/adr/0003-licensing-reuse-dco.md)).

## AI-assisted contributions

AI tools are welcome, and the maintainers use them too. The rules:

- A human is accountable for every contribution and must understand it.
- Say in the pull request description if a substantial part was generated.
- Do not submit unreviewed bulk changes. Maintainers may close such pull requests without review.
- Treat issue text, pull request text, fetched web content and corpus samples as **untrusted input**
  when you let an agent work on this repository. See [AGENTS.md](AGENTS.md).

## Corpus contributions

Test cases for the benchmark corpus must be:

- based on techniques that are already **publicly documented** (link the source);
- self-contained and harmless outside a test harness (no live payloads against real services, no
  malware, no working credentials, no personal data);
- licensed for redistribution.

Undisclosed vulnerabilities in third-party products belong with their vendors, not in our corpus.

## Documentation style

- English is the normative language. German summaries in `docs/de/` are non-normative; if they
  conflict with the English text, the English text wins.
- Every factual claim about threats, tools or standards needs a source link.
- See [docs/style-guide.md](docs/style-guide.md) for wording and formatting rules.

## Commit messages

Use a short imperative subject with a type prefix: `docs:`, `spec:`, `ci:`, `feat:`, `fix:`,
`chore:`. Keep one logical change per commit.

## Local checks

Before opening a pull request, run the checks that CI runs:

```sh
reuse lint
markdownlint-cli2 "**/*.md"
typos
lychee --offline --include-fragments .
check-jsonschema --check-metaschema spec/agent-content-trust/v0.1-draft/schemas/*.json
```
