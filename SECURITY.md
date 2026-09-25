# Security Policy

Kopist is security software that parses hostile content by design. We take reports seriously and
we define clearly what counts as a vulnerability.

## Supported versions

The project is in the design phase. There are no releases yet. Once releases exist, this table
lists the supported lines.

| Version | Supported |
|---|---|
| none yet | n/a |

## Reporting a vulnerability

- **Preferred:** GitHub private vulnerability reporting on this repository
  (Security tab, "Report a vulnerability").
- **Fallback:** a dedicated `security@` address will be listed in [MAINTAINERS.md](MAINTAINERS.md)
  once the project domain exists.

Please do not open public issues for vulnerabilities. Include affected component, version or commit,
reproduction steps, impact, and whether the issue is already public.

## What counts as a vulnerability

Kopist separates **guarantees** (enforced by architecture) from **advisory detection** (best effort).
Scope is defined by the [guarantee registry](docs/security/guarantees.md).

In scope, for example:

- Original bytes reach the consumer although a reconstruction profile was active.
- A hidden-content class listed as covered in the guarantee registry survives the `strict` profile.
- The egress guard can be bypassed for a covered channel.
- A parser or renderer escapes its isolation tier.
- A receipt can be forged, or the verifier accepts an invalid receipt.
- Server-side request forgery (SSRF) or access to local resources through the fetcher.
- Supply-chain issues in our build or release process.

Out of scope, handled publicly instead:

- **Detector evasions.** Detectors are advisory by design and are assumed to be bypassable by adaptive
  attackers. Please contribute evasions as corpus cases via the
  "attack technique" issue template so they become regression tests.
- Visible instructions in otherwise legitimate content, unless a documented guarantee is violated.
- Issues in third-party dependencies that do not affect Kopist; please report them upstream.

## Response targets

These targets reflect a small maintainer team and will be tightened as the team grows.

| Step | Target |
|---|---|
| Acknowledgement | within 5 business days |
| Triage and severity assessment | within 14 days |
| Fix or public advisory | within 90 days |
| Actively exploited issues | best effort, within 7 days |

We coordinate disclosure with the reporter, credit reporters who want to be credited, and publish
advisories through GitHub Security Advisories. GitHub acts as CVE Numbering Authority.

## Safe harbor

We consider security research conducted in good faith and in line with this policy to be authorized.
We will not pursue legal action against researchers who:

- test only against their own installations, not against third-party systems;
- avoid privacy violations, data destruction and service disruption;
- give us reasonable time to fix the issue before public disclosure.

This safe-harbor statement follows the spirit of [disclose.io](https://disclose.io/).
It cannot authorize testing against systems the project does not own.

## Regulatory note

Kopist is non-commercial open-source software. The maintainers follow the practices expected of an
open-source software steward under the EU Cyber Resilience Act (Regulation (EU) 2024/2847, Art. 24),
see [docs/security/cvd-runbook.md](docs/security/cvd-runbook.md). This is not legal advice.

There is no bug bounty.
