# Kopist

**A local-first content trust gateway for AI agents.**
Never forward what you fetched: Kopist gives agents a rebuilt, typed and labeled copy of web content,
together with a signed receipt, and controls what the agent is allowed to send back out.

> Status: **pre-alpha, design phase.** This repository currently contains the concept, threat model,
> architecture and a draft specification. There is no product code yet. Reviews of the design are the
> most valuable contribution right now.
>
> Deutsch: [Kurzfassung auf Deutsch](docs/de/README.md)

## Why

AI agents read the open web, documents, images and tool output, and they act on what they read.
That makes every fetched byte a potential instruction:

- Hidden text in web pages hijacked browsing agents
  ([Brave on Perplexity Comet, 2025](https://brave.com/blog/comet-prompt-injection/)).
- Images that look harmless can carry instructions that only appear after the pipeline downscales them
  ([Trail of Bits, 2025](https://blog.trailofbits.com/2025/08/21/weaponizing-image-scaling-against-production-ai-systems/)).
- Injected content led coding agents to execute code on developer machines
  ([CVE-2025-53773](https://msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2025-53773)).
- Around 250 poisoned documents were enough to backdoor models of every size tested
  ([Anthropic, UK AISI, Alan Turing Institute, 2025](https://www.anthropic.com/research/small-samples-poison)).
- Vendors state that prompt injection is "unlikely to ever be fully solved"
  ([OpenAI, 2025](https://openai.com/index/hardening-atlas-against-prompt-injection/)).

Detectors alone do not hold up against adaptive attackers
([Nasr, Carlini et al., 2025](https://arxiv.org/abs/2510.09023)).
Kopist therefore puts its security boundary into architecture, not classification.

## How

1. **Reconstruct, don't filter.** Content is parsed into a closed intermediate representation and re-emitted.
   Whatever the representation cannot express (hidden text, invisible Unicode, scripts, metadata,
   scaling payloads) does not reach the model.
2. **Label everything.** Every output carries an
   [Agent Content Trust](spec/agent-content-trust/v0.1-draft/specification.md) label (origin, integrity,
   confidentiality, form) and a signed, offline-verifiable receipt.
3. **Prefer typed extraction.** Where a task allows it, agents receive schema-validated values from a
   quarantined reader instead of free prose.
4. **Deny egress by default.** Outbound requests, tool calls, markdown images and link previews pass one
   guard that checks destinations and data flow.
5. **Run locally.** A single binary on `127.0.0.1`, no telemetry, any model, any agent framework
   (MCP, HTTP, SDKs).

What Kopist does **not** claim: it does not "prevent prompt injection". Reconstruction removes hidden
channels; visible instructions inside legitimate content are handled by labels, typed extraction and
egress control, and are measured separately. See [the guarantee registry](docs/security/guarantees.md).

## Documentation

| Topic | Document |
|---|---|
| Vision and non-goals | [docs/vision.md](docs/vision.md) |
| Design principles | [docs/principles.md](docs/principles.md) |
| Threat model | [docs/threat-model.md](docs/threat-model.md) |
| Architecture | [docs/architecture.md](docs/architecture.md) |
| Related work | [docs/landscape.md](docs/landscape.md) |
| Evaluation method | [docs/evaluation.md](docs/evaluation.md) |
| Specification (draft) | [spec/](spec/README.md) |
| Architecture decisions | [docs/adr/](docs/adr/) |
| Roadmap | [ROADMAP.md](ROADMAP.md) |

## Contributing

Design reviews, threat-model critique and publicly documented attack techniques for the test corpus are
welcome now. Read [CONTRIBUTING.md](CONTRIBUTING.md) and [GOVERNANCE.md](GOVERNANCE.md) first.
Security issues: [SECURITY.md](SECURITY.md).

## License

- Code, schemas and configuration: [Apache-2.0](LICENSE)
- Documentation and specification prose (`docs/`, `spec/**/*.md`): [CC-BY-4.0](LICENSES/CC-BY-4.0.txt)

Licensing per file is declared in [REUSE.toml](REUSE.toml).

The project was started under the working title "gateway"; see
[ADR 0010](docs/adr/0010-project-name.md).
