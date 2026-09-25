# Findings taxonomy, v0.1 (draft)

Finding categories are lowercase identifiers (`^[a-z][a-z0-9-]{1,63}$`). Producers MAY define
additional categories; consumers MUST treat unknown categories as having no effect on their decisions
unless their policy says otherwise.

Taxonomy references use these prefixes:

- `OWASP-LLMxx:2025` for the
  [OWASP Top 10 for LLM Applications 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/);
- `OWASP-ASIxx:2026` for the
  [OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/);
- `ATLAS-<technique>` for [MITRE ATLAS](https://atlas.mitre.org/).

The mappings below are initial and will be reviewed against the current versions of each taxonomy
before v0.1 is finalized.

| Category | Meaning | Typical references |
|---|---|---|
| `hidden-text` | Text present in the source but not visible to a human reader | OWASP-LLM01:2025, OWASP-ASI01:2026, ATLAS-AML.T0051 |
| `render-divergence` | Rendered output differs from the structural text layer (HTML DOM, PDF text) | OWASP-LLM01:2025 |
| `unicode-smuggling` | Invisible or tag characters that can carry hidden text | OWASP-LLM01:2025 |
| `bidi-control` | Bidirectional control characters that reorder displayed text | OWASP-LLM01:2025 |
| `homoglyph` | Mixed-script or confusable characters | OWASP-LLM01:2025 |
| `cloaking` | Different content served to different client identities | OWASP-LLM01:2025, OWASP-LLM04:2025 |
| `url-fragment-removed` | A URL fragment was removed before delivery | OWASP-LLM01:2025 |
| `prompt-parameter` | A URL parameter appears to carry instructions for an assistant | OWASP-LLM01:2025, OWASP-ASI06:2026 |
| `instruction-like` | Visible text that addresses an AI system with instructions | OWASP-LLM01:2025, OWASP-ASI01:2026 |
| `tool-reference` | Content names tools, functions or agent capabilities | OWASP-ASI02:2026 |
| `exfil-vector` | Content contains URLs or markup patterns suited for data exfiltration | OWASP-LLM02:2025, OWASP-LLM05:2025 |
| `encoded-payload` | Long base64, hex or similar blobs | OWASP-LLM01:2025 |
| `scaling-anomaly` | Image content changes materially under downscaling | OWASP-LLM01:2025 |
| `text-in-image` | Text recognized inside an image | OWASP-LLM01:2025 |
| `stego-suspect` | Statistical indications of steganography | OWASP-LLM01:2025 |
| `active-content-removed` | Scripts, macros, document JavaScript or embedded files were removed | OWASP-LLM05:2025 |
| `metadata-stripped` | Metadata was removed | none |
| `definition-changed` | A pinned tool or agent definition changed | OWASP-ASI04:2026 |
| `provenance-verified` | Upstream provenance (for example C2PA) validated | none |
| `provenance-invalid` | Upstream provenance present but invalid | OWASP-LLM04:2025 |
| `usage-restricted` | The source signals usage restrictions for this mode | none |
| `recent-edit` | The source changed shortly before retrieval (frontrunning risk) | OWASP-LLM04:2025 |
