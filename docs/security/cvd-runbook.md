# Coordinated vulnerability disclosure runbook

For maintainers. The public policy is [SECURITY.md](../../SECURITY.md).

## 1. Intake

1. A report arrives through GitHub private vulnerability reporting (or the fallback address).
2. Acknowledge within 5 business days. Thank the reporter, confirm the channel, give the next date.
3. Create a draft GitHub Security Advisory. Keep all discussion in the advisory, not in public issues.

## 2. Triage (target: 14 days)

1. **Scope check** against the [guarantee registry](guarantees.md):
   - violation of a listed guarantee: vulnerability;
   - detector evasion or visible-instruction case: not a vulnerability; with the reporter's
     consent, move it to a public corpus issue and thank them publicly;
   - third-party issue: help the reporter reach the upstream project.
2. **Reproduce** on the latest release and `main`.
3. **Score** with CVSS 4.0 and note the affected versions and components.
4. **Exploitation status:** is it actively exploited? If yes, follow the fast path (target 7 days).

## 3. Fix

1. Develop the fix in a temporary private fork linked to the advisory.
2. Add a regression test and, where applicable, a corpus case.
3. Request a CVE through the advisory (GitHub is a CVE Numbering Authority).
4. Agree on a disclosure date with the reporter. Default: when the fix is released, at most 90 days
   after the report.

## 4. Release and disclose

1. Release the fix with signed artifacts.
2. Publish the advisory, crediting the reporter if they wish.
3. Update [CHANGELOG.md](../../CHANGELOG.md).
4. Notify known downstream integrators listed in the security contacts file (once it exists).

## 5. EU Cyber Resilience Act decision tree (not legal advice)

The Cyber Resilience Act (Regulation (EU) 2024/2847) distinguishes manufacturers, which place products
on the market in a commercial activity, from open-source software stewards
([ORC WG FAQ](https://cra.orcwg.org/faq/stewards/)).

1. **Is the project itself a manufacturer?** No, as long as Kopist is developed and supplied
   outside a commercial activity (see [GOVERNANCE.md](../../GOVERNANCE.md)).
2. **Is there a steward?** Once Kopist moves to a foundation, that foundation may act as steward
   with its own obligations (Art. 24). Until then, the maintainers follow steward-grade practices
   voluntarily: this runbook, a published security policy, and cooperation with authorities.
3. **Is a downstream manufacturer affected?** Commercial products that integrate Kopist carry
   manufacturer obligations, including reporting actively exploited vulnerabilities through ENISA's
   single reporting platform from 2026-09-11
   ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting)). Give
   known integrators early, embargoed notice so they can meet their deadlines.

## 6. Records

Keep a private log per case: dates of report, acknowledgement, triage, fix, release and disclosure.
Publish yearly statistics (number of reports, median response times).
