# ADR 0010: Project name "Kopist"

- Status: accepted
- Date: 2026-09-25

## Context

The repository started under the working title "gateway". That word is a generic category noun: hard
to search for, not protectable as a mark, and already used by several AI and MCP gateways
(for example [agentgateway](https://github.com/agentgateway/agentgateway) and
[Docker MCP Gateway](https://github.com/docker/mcp-gateway)).

## Decision

The project is named **Kopist**, the German word for a copyist: a scribe who produces a clean copy
and never hands over the original. This expresses the core principle "reconstruct, don't filter".
As of 2026-09-25 the name was not taken on crates.io, PyPI or npm.

## Open items

- A GitHub organization will host the repository. Until the transfer, links point to
  `https://github.com/kwdwetz/gateway`; GitHub redirects after a rename or transfer.
- A project domain will be registered for specification identifiers. Until then the specification
  uses the placeholder `<spec-domain>` for URIs that must be permanent.
- A trademark search (EUIPO, DPMA, USPTO; Nice classes 9 and 42) is pending. A registered mark, if
  any, will be transferred to the neutral home chosen under the governance plan.

## Consequences

- Package names `kopist` and `kopist-*` are used for crates, and the equivalent names for Python and
  npm packages.
- Placeholders must be replaced before specification v0.1 is finalized.
