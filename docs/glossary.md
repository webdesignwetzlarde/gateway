# Glossary

| Term | Meaning in this project |
|---|---|
| Agent | Software that uses a language model to decide and perform actions |
| Consumer | Whatever receives Kopist output: an agent, a retrieval pipeline, a crawler |
| Content IR | Kopist's closed intermediate representation of documents; only what it can express reaches the consumer |
| Reconstruction | Parsing content into the IR and re-emitting it, instead of forwarding original bytes |
| CDR | Content disarm and reconstruction, the general technique Kopist applies to agents |
| Hidden channel | Content a human reviewer would not perceive but a model would read |
| Visible instruction | Instruction-like text that is part of the visible, legitimate content |
| Trust label | Compact metadata attached to output: origin, integrity, confidentiality, form, findings, receipt reference |
| Content receipt | Signed record of how an output was produced from its sources |
| Action receipt | Signed record of an egress decision |
| Finding | A detector or pipeline observation, for example `hidden-text` or `cloaking` |
| Guarantee | A property enforced by architecture and listed in the [guarantee registry](security/guarantees.md) |
| Advisory detection | Best-effort detection that can only lower trust, never make content trusted |
| Egress | Any action that sends data out or changes state outside the agent |
| Origin set | Origins an agent may read from or write to in a session |
| Taint | Marker that data derives from untrusted content |
| Quarantined reader | Model without tools, network or secrets that turns content into typed values |
| Handle | Opaque reference to content that a privileged agent can pass around without reading it |
| Profile | Named policy preset: `relaxed`, `standard`, `strict`, `paranoid` |
| Isolation tier | Sandbox level T0 to T3 for untrusted parsers and renderers |
| TCB | Trusted computing base: the small set of components whose correctness guarantees rely on |
| Model binding | Adapter that places untrusted content into a model's best channel for it |
| View | One fetch of a resource with a given identity; comparing views detects cloaking |
| PQC | Post-quantum cryptography |
