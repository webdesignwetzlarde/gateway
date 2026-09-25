# Egress guard

Input filtering alone does not stop exfiltration or harmful actions. An agent that holds private
data, reads untrusted content and can communicate externally can be made to leak data
([Willison, "lethal trifecta", 2025](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)).
The egress guard controls the third leg.

## Covered channels

| Channel | Integration |
|---|---|
| HTTP requests of the agent | HTTP proxy (Phase 2), SDK hook (Phase 1) |
| MCP tool calls | MCP proxy (Phase 2) |
| Model output about to be rendered or sent | Output guard in SDKs and integrations |
| Link previews in chat platforms | Output guard rewrites URLs |
| Writes to agent configuration files | Agent hooks (for example a Claude Code `PreToolUse` hook) |
| Memory writes of long-running agents | SDK hook (Phase 4) |

## Checks

1. **Destination.** Origin sets modeled on Chrome's agent security design
   ([Google, 2025-12](https://security.googleblog.com/2025/12/architecting-security-for-agentic.html)):
   origins named in the user's task may be read and written; origins discovered in untrusted content
   are read-only; sending data to a new origin requires approval.
2. **Data flow.** Tainted handles, secret patterns and seeded canary tokens are searched in URLs,
   headers, bodies and tool arguments. A request that carries data counts as egress even to an
   allowed domain, because allowlisted domains have been abused as relays
   ([EchoLeak](https://arxiv.org/html/2509.10540v1)).
3. **Channel rules.** Markdown image URLs are rewritten or removed
   ([example](https://embracethered.com/blog/posts/2025/amp-code-fixed-data-exfiltration-via-images/));
   URLs in messages to chat platforms are neutralized to prevent automatic previews
   ([PromptArmor, 2026](https://www.promptarmor.com/resources/llm-data-exfiltration-via-url-previews-(with-openclaw-example-and-test)));
   high-entropy DNS labels and open redirects are flagged.
4. **Agent configuration.** Writes to paths such as `.vscode/settings.json`, `.cursor/mcp.json` or
   MCP configuration files are blocked or require approval. This addresses
   [CVE-2025-53773](https://msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2025-53773) and
   Cursor CVE-2025-54135 and CVE-2025-54136
   ([Tenable](https://www.tenable.com/blog/faq-cve-2025-54135-cve-2025-54136-vulnerabilities-in-cursor-curxecute-mcpoison)).
5. **Budgets.** Rate and byte limits per session and destination.
6. **Rule of Two.** In this mode, a session that combines untrusted input, sensitive data and external
   communication needs human approval or is blocked
   ([Meta, 2025](https://ai.meta.com/blog/practical-ai-agent-security/)).

## Decisions

`allow`, `redact`, `ask` (human approval with a readable diff) or `block`. Every decision produces an
action receipt. Kopist aims for format compatibility with other action-receipt implementations such as
[Pipelock](https://github.com/luckyPipewrench/pipelock).

## Limits

The guard only sees channels routed through it. Integration guides must make bypass difficult, for
example by disabling an agent's native fetch tool when Kopist is configured.
