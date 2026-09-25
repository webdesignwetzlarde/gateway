# Related work and differentiation

Status as of 2026-09. Corrections are welcome. Licenses were checked on the linked repositories where
stated; please report changes.

## Research on defenses

| Work | Core idea | Relation to Kopist |
|---|---|---|
| [Dual LLM pattern](https://simonwillison.net/2023/Apr/25/dual-llm-pattern/) (Willison, 2023) | A privileged model holds tools; a quarantined model reads untrusted data and returns symbolic handles | Basis of typed extraction and handles |
| [CaMeL](https://arxiv.org/abs/2503.18813) (Debenedetti et al., 2025) | Planner sees only the trusted query; an interpreter tracks capabilities and data flow | Label flow rules; Kopist supplies building blocks for framework integration |
| [Design Patterns for Securing LLM Agents](https://arxiv.org/abs/2506.08837) (Beurer-Kellner et al., 2025) | Six patterns with provable properties | Kopist implements context minimization and dual-LLM patterns as a service |
| [FIDES](https://arxiv.org/abs/2505.23643) (Microsoft Research, 2025) | Information-flow control with confidentiality and integrity labels | Label lattice in the specification |
| [Spotlighting](https://arxiv.org/abs/2403.14720) (Hines et al., 2024) | Mark untrusted text by delimiting, datamarking or encoding | Fallback model binding |
| [Instruction hierarchy](https://arxiv.org/abs/2404.13208) (OpenAI, 2024) | Train models to rank instructions by privilege | Complementary; Kopist uses native untrusted channels when available |
| [SecAlign](https://arxiv.org/abs/2410.05451) and [Meta SecAlign](https://arxiv.org/abs/2507.02735) | Model-level training against injection | Complementary |
| [Agents Rule of Two](https://ai.meta.com/blog/practical-ai-agent-security/) (Meta, 2025) | At most two of: untrusted input, sensitive data, external action | Enforcement mode of the egress guard |
| [The Attacker Moves Second](https://arxiv.org/abs/2510.09023) (Nasr, Carlini et al., 2025) | Adaptive attacks bypass published detectors | Reason detectors are advisory in Kopist |
| [DataFilter](https://arxiv.org/abs/2510.19207) (2025) | Model-agnostic removal of injected instructions from data | Candidate advisory detector |
| [Chrome agentic security](https://security.googleblog.com/2025/12/architecting-security-for-agentic.html) (Google, 2025) | Alignment critic sees only action metadata; agent origin sets | Origin sets in the egress guard |

## Tools

| Tool | What it does | License | Relation to Kopist |
|---|---|---|---|
| [Pipelock](https://github.com/luckyPipewrench/pipelock) | Go egress firewall for HTTP, MCP, A2A; text normalization, pattern detection, signed action receipts | Apache-2.0 core; parts under Elastic License 2.0 | Closest project. Kopist focuses on reconstruction across formats and content receipts; aims for receipt interoperability |
| [LlamaFirewall](https://github.com/meta-llama/PurpleLlama/tree/main/LlamaFirewall) (Meta) | Orchestrates guard models and scanners | MIT | Possible advisory detector adapter |
| [NeMo Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) (NVIDIA) | Programmable rails for input, output, retrieval and execution | Apache-2.0 | Complementary at the application layer |
| [garak](https://github.com/NVIDIA/garak) (NVIDIA) | LLM vulnerability scanner | Apache-2.0 | Regression testing |
| [LLM Guard](https://github.com/protectai/llm-guard) | Input and output scanners | MIT; archived in 2026 | Shows the fragility of the detector ecosystem |
| [Rebuff](https://github.com/protectai/rebuff) | Heuristic and model-based injection detection | archived | Same |
| [Invariant guardrails](https://github.com/invariantlabs-ai/invariant) | Rule language for agent traces | Apache-2.0 | Policy ideas for tool-call flows |
| [Snyk Agent Scan](https://github.com/snyk/agent-scan) (formerly MCP-scan) | Scans MCP servers and skills | Apache-2.0 | Complementary scanner |
| [mcp-context-protector](https://github.com/trailofbits/mcp-context-protector) (Trail of Bits) | MCP wrapper with definition pinning and quarantine | Apache-2.0 | Similar ideas for the MCP proxy |
| [agentgateway](https://github.com/agentgateway/agentgateway) (Linux Foundation) | Rust proxy for LLM, MCP and A2A traffic: routing, authentication, observability | Apache-2.0 | Complementary; no content reconstruction |
| [Docker MCP Gateway](https://github.com/docker/mcp-gateway) | Runs MCP servers in containers with interceptors | MIT | Complementary |
| [IBM ContextForge](https://github.com/IBM/mcp-context-forge) | Gateway for MCP, A2A and REST with plugins | Apache-2.0 | Complementary |
| [Dangerzone](https://github.com/freedomofpress/dangerzone) | Converts documents to pixels and back to safe PDFs in a sandbox | AGPL-3.0 | Inspiration for the pixel path; not linked |
| [Firecrawl](https://github.com/firecrawl/firecrawl), [Crawl4AI](https://github.com/unclecode/crawl4ai), [Jina Reader](https://github.com/jina-ai/reader) | Fetch web pages as Markdown for LLMs | AGPL-3.0, Apache-2.0, Apache-2.0 | Fetch tools without security reconstruction |

## Benchmarks

See [evaluation](evaluation.md).

## Standards and protocols

| Standard | Status | Relation to Kopist |
|---|---|---|
| [Model Context Protocol](https://blog.modelcontextprotocol.io/posts/2026-07-28/) | Specification 2026-07-28 with an extensions framework; governed by the Agentic AI Foundation ([announcement](https://blog.modelcontextprotocol.io/posts/2025-12-09-mcp-joins-agentic-ai-foundation/)) | First interface; target for a content-trust extension |
| [MCP trust and sensitivity annotations (SEP-1913)](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/1913) | Proposal | Agent Content Trust labels can serve as evidence |
| [Agent2Agent](https://github.com/a2aproject/A2A) | Linux Foundation project | Later interface |
| [IETF AIPREF](https://ietf-wg-aipref.github.io/drafts/draft-ietf-aipref-vocab.html) | Working group drafts | Usage decision in the fetcher |
| [Web Bot Auth](https://github.com/cloudflare/web-bot-auth) on [RFC 9421](https://www.rfc-editor.org/rfc/rfc9421) | Emerging | Signed fetcher identity |
| [C2PA](https://c2pa.org/) | Content credentials specification | Validated for images, recorded in receipts |
| [OpenTelemetry GenAI semantic conventions](https://github.com/open-telemetry/semantic-conventions-genai) | Development status | Telemetry attributes; proposal for content-trust attributes |
| [SLSA 1.2](https://slsa.dev/blog/2025/11/announce-slsa-v1.2), [Sigstore](https://www.sigstore.dev/) | Released | Supply chain of Kopist itself; receipt envelope compatibility |
| [NIST post-quantum cryptography](https://csrc.nist.gov/projects/post-quantum-cryptography) | FIPS 203, 204, 205 final | Hybrid receipt signatures |

## The gap Kopist fills

1. No open, local-first component **reconstructs** content across HTML, PDF and images for agents;
   existing gateways route, inspect or block.
2. No standard **trust label and receipt** travels with content delivered to agents.
3. No network service offers the **quarantined-reader** pattern with typed results.
4. No open, maintained **evaluation harness** measures pre-model content defenses across formats
   together with utility.
5. No fetcher combines **publisher preferences, signed agent identity and sanitization**.
