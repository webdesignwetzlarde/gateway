# Vision

## Problem

AI agents browse, retrieve, summarize and act. Every page, document, image and tool result they read
can carry instructions aimed at them rather than at the human user. Unlike classic malware, these
instructions do not need an exploit: they only need to be read.

Documented consequences include agents that:

- followed hidden page text into cross-site actions such as reading one-time passwords from email
  ([Brave, 2025-08-20](https://brave.com/blog/comet-prompt-injection/));
- leaked organizational data without any user interaction
  ([EchoLeak, CVE-2025-32711](https://arxiv.org/html/2509.10540v1));
- published private repository contents after reading a malicious public issue
  ([Invariant Labs, 2025](https://invariantlabs.ai/blog/mcp-github-vulnerability));
- executed code on developer machines after changing their own configuration
  ([CVE-2025-53773](https://msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2025-53773));
- repeated content that was served only to AI crawlers
  ([SPLX, 2025](https://splx.ai/blog/ai-targeted-cloaking-openai-atlas)).

The same web is the source of training data, where a small, near-constant number of poisoned
documents can implant a backdoor
([Anthropic, UK AISI, Alan Turing Institute, 2025](https://www.anthropic.com/research/small-samples-poison)).

Model vendors are hardening their models, and they state openly that the problem is unlikely to be
solved completely at the model level
([OpenAI, 2025-12-22](https://openai.com/index/hardening-atlas-against-prompt-injection/)).
Research shows that detection-only defenses are bypassed by adaptive attackers
([Nasr, Carlini et al., 2025](https://arxiv.org/abs/2510.09023)).

## Why now

- Agents moved from chat into browsers, IDEs, terminals and company systems in 2025 and 2026.
- Protocols for agents (the Model Context Protocol, Agent2Agent) are being standardized now, and none
  of them carries a trust label for the content an agent receives
  ([MCP 2026-07-28 release notes](https://blog.modelcontextprotocol.io/posts/2026-07-28/)).
- Regulation is taking effect: the EU Cyber Resilience Act's reporting obligations apply from
  2026-09-11, the full regulation from 2027-12-11
  ([European Commission](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting)).
- Quantum-safe signatures are standardized
  ([NIST FIPS 203/204/205, 2024-08-13](https://csrc.nist.gov/projects/post-quantum-cryptography)),
  so long-lived provenance records can be designed correctly from the start.

## What Kopist is

Kopist is a **local-first content trust gateway**. Agents, retrieval pipelines, coding assistants and
crawlers fetch content through it. Kopist:

1. **reconstructs** content into a closed intermediate representation instead of forwarding it;
2. **labels** every output with origin, integrity, confidentiality and form;
3. issues a **signed content receipt** that anyone can verify offline;
4. offers **typed extraction** through a quarantined reader, so agents can receive values instead
   of prose;
5. **guards egress**, so data cannot silently leave through requests, tool calls, markdown images or
   link previews.

Alongside the implementation, the project maintains the open
[Agent Content Trust specification](../spec/README.md), so labels and receipts can be produced and
checked by any tool, not only by Kopist.

## Who it is for

| User | Typical use |
|---|---|
| Individual developers | Let coding agents fetch documentation and issues through Kopist |
| Teams and companies | Run Kopist as a sidecar or shared service in front of agent platforms |
| RAG and search builders | Ingest documents with labels and verifiable provenance |
| Dataset builders and researchers | Crawl with integrity manifests and respect publisher preferences |
| Agent framework authors | Consume labels and receipts through SDKs |
| Security researchers | Use the benchmark and corpus to measure defenses |

## Non-goals

- Kopist is not a web application firewall, a data-loss-prevention suite, an LLM router or a
  hosted service.
- Kopist does not train models and does not ship a proprietary detection model.
- Kopist does not bypass paywalls, CAPTCHAs, logins or other access controls.
- Kopist does not promise to "prevent prompt injection". It removes hidden channels, makes trust
  explicit, and constrains what an agent can do with untrusted content. Its guarantees are listed in
  [the guarantee registry](security/guarantees.md).

## Success in ten years

- Content delivered to agents carries a standard, verifiable trust label by default.
- At least two independent, interoperable implementations of the specification exist.
- Kopist is maintained by a neutral foundation and a diverse maintainer group.
- Security claims in this space are backed by open, adaptive benchmarks.
