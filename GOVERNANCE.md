# Governance

Kopist is meant to become neutral, community-owned infrastructure. This document describes how
decisions are made today and how governance will change as the project grows.

## Principles

- **Open by default.** Design discussions, decisions and roadmaps are public.
- **Written decisions.** Significant decisions are recorded as
  [Architecture Decision Records](docs/adr/).
- **No open core.** All project code stays under Apache-2.0. There will be no proprietary
  "enterprise edition" of Kopist maintained by the project.
- **Vendor neutrality.** Kopist works with any model, agent framework and vendor. No single company
  may control the project.

## Current model: time-limited project lead (stage G0)

The project was founded by [@kwdwetz](https://github.com/kwdwetz), who acts as project lead with
final decision authority (a "benevolent dictator for life", BDFL, but explicitly **not** for life).

**Sunset clause.** The project-lead role ends automatically at the earlier of:

- the project having at least three maintainers employed by at least two different organizations, or
- **2028-06-30**.

After that, decisions move to the Maintainers Council described below.

## Roles

| Role | How to get it | Rights |
|---|---|---|
| Contributor | Any merged contribution | Recognition in release notes |
| Committer | Sustained quality contributions, nominated by a maintainer | Triage, review |
| Maintainer | Nominated by maintainers, lazy consensus, no veto from existing maintainers | Merge, release, vote |
| Security team | Maintainers who opt in, at least two people once possible | Embargoed vulnerability handling |
| Spec editor | Appointed per specification version | Edits the specification |

Specification editors and implementation maintainers are separate roles, so the specification can
gain independent implementations.

## Decision making

1. **Lazy consensus** for day-to-day work: a proposal is accepted if no maintainer objects within
   72 hours (7 days for documents under `spec/`).
2. **Design proposals** for large or cross-cutting changes: an issue using the "design proposal"
   template, open for at least 14 days, ending in an ADR.
3. **Votes** only when consensus fails: simple majority of maintainers. During stage G0 the project
   lead decides and documents the reasoning.

## Evolution

| Stage | Target | Model |
|---|---|---|
| G0 | now | Project lead with sunset clause |
| G1 | 2027 H2 | Committers and maintainers, security team, conflict-of-interest register |
| G2 | 2028 | Maintainers Council (3 to 5 people); move to a neutral foundation |
| G3 | 2030 onward | Technical Steering Committee elected by contributors |

**Neutral home.** Candidate homes will be compared against published criteria (neutrality, cost,
capability to act as open-source steward under the EU Cyber Resilience Act, fit): for the code, for
example the OpenSSF or the Eclipse Foundation; for the specification, for example the Agentic AI
Foundation, CoSAI or OASIS.

## Trademark and domain

The project name and domains are currently held by the founder. The founder commits to transfer them
to the neutral home chosen in stage G2, free of charge.

## Conflicts of interest

Maintainers disclose relevant affiliations in [MAINTAINERS.md](MAINTAINERS.md). A maintainer does not
decide alone on matters that primarily benefit their employer or their own business. The founder runs
an independent business; that business does not sell Kopist, Kopist support or hosted Kopist services.

## Changing this document

Changes to this document follow the design-proposal process and need approval by the project lead
(stage G0) or a two-thirds majority of maintainers (from stage G1).
