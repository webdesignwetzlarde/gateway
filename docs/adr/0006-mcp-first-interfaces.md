# ADR 0006: MCP, CLI and HTTP API first

- Status: accepted
- Date: 2026-09-25

## Context

Kopist must be usable by many agents quickly. A transparent TLS-intercepting proxy reaches every
client but is complex, fragile with certificate pinning, and risky to operate.

## Decision

The first interfaces are:

1. a CLI for inspection and testing;
2. a local daemon with a small REST API;
3. an MCP server exposing `fetch`, `read_document`, `extract` and `inspect` tools, plus ready-made
   configurations that replace an agent's native fetch tools.

The MCP proxy and the HTTP forward proxy follow in Phase 2, the Agent2Agent proxy in Phase 3.

## Consequences

- Coding agents and desktop agents can adopt Kopist through configuration alone.
- Channels that bypass Kopist remain unguarded until the proxies ship; integration guides must say so.
