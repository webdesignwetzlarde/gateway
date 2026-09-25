# Reconstruction pipelines

Each pipeline turns untrusted bytes into IR (see [architecture](../architecture.md#4-content-ir)).
Pipelines are untrusted code and run in the isolation tier assigned by policy
([isolation tiers](isolation-tiers.md)). Every removal or transformation is recorded in the receipt.

## Unicode normalization (all text)

Applied by the trusted core to every text node, regardless of format.

- Normalize to NFC. NFKC is used only for detection, never for output.
- Remove and record: the Tags block U+E0000 to U+E007F (ASCII smuggling), zero-width characters
  outside legitimate script contexts, bidirectional control characters, private-use characters,
  C0 and C1 controls except tab and newline, noncharacters, invisible fillers, and misused
  variation selectors.
- Detect mixed scripts and confusables following
  [Unicode Technical Standard #39](https://www.unicode.org/reports/tr39/).
- Flag long base64, hex and similar encoded blobs. They are never decoded into the output.
- Escape characters that would create active Markdown or HTML syntax in the output.

## HTML

- Parse with an HTML5-compliant parser.
- Determine visibility in one of two modes, recorded in the receipt:
  - **heuristic:** evaluate inline styles, `<style>` elements and linked stylesheets fetched by
    Kopist;
  - **rendered:** headless Chromium with JavaScript disabled by default and no network of its own;
    subresources are served through Kopist.
- Treat text as hidden when it is `display:none`, `visibility:hidden`, fully transparent, sized or
  clipped to near zero, positioned off-screen, covered by other elements, has near-zero contrast with
  its background, or is marked `aria-hidden`.
- **Render-versus-DOM diff:** text present in the DOM but absent from the rendering (optionally
  confirmed by OCR of a screenshot) is hidden text. Default: dropped. Policy may keep it under a
  `hidden` label for analysis.
- Remove scripts, styles, comments, forms, iframes, embedded objects and most meta content. Title and
  description are kept and labeled.
- Alt text and ARIA labels are kept as attribute-sourced text, labeled and length-limited.
- Links are made absolute; fragments and known tracking parameters are removed; a mismatch between
  link text and link host is flagged.

Motivation: hidden page text has hijacked browsing agents
([Brave, 2025](https://brave.com/blog/comet-prompt-injection/)) and search answers
([TechCrunch, 2024](https://techcrunch.com/2024/12/26/chatgpt-search-can-be-tricked-into-misleading-users-new-research-reveals/)).

## PDF

- **Structural path:** extract text runs with positions. Neutralize or flag text with render mode 3
  (invisible), text with near-zero contrast or size, text outside the crop box, and content from
  earlier revisions left by incremental updates. Only the last visible revision counts.
- Remove JavaScript, open actions, embedded files, XFA forms and annotations.
- Compare the text layer with OCR of the rendered page; divergence is a finding.
- **Pixel path** (profile `paranoid`, or when the structural path fails): render every page to a
  bitmap at a fixed resolution, run OCR, and emit text and optionally re-rasterized images. The idea
  follows [Dangerzone](https://github.com/freedomofpress/dangerzone); Kopist implements it
  independently because Dangerzone is AGPL-licensed.
- Rendering uses pdfium (BSD-licensed). MuPDF (AGPL) and Poppler (GPL) are not used.

## Images

- Decode inside a WebAssembly sandbox.
- If a [C2PA](https://c2pa.org/) manifest exists, validate it and record the result, then strip all
  metadata (EXIF, XMP, IPTC and similar).
- **Defeat scaling attacks:** resample deterministically to the consumer's target resolution with an
  area or Lanczos filter, so the model receives the pixels a human sees in preview. This addresses the
  attack published by
  [Trail of Bits (2025)](https://blog.trailofbits.com/2025/08/21/weaponizing-image-scaling-against-production-ai-systems/).
- Optionally add low-amplitude noise and re-encode. This reduces least-significant-bit steganography
  ([arXiv 2507.22304](https://arxiv.org/abs/2507.22304)); it does not eliminate steganography.
- Text found by OCR is labeled `text-in-image` and treated as an instruction channel.
- Optional mode "describe instead of pass": a quarantined vision model returns a typed description
  and the image itself is withheld.

## Office documents (Phase 2)

- Remove macros, OLE objects, external links and DDE fields.
- Hidden sheets, rows and columns, white text, comments, tracked changes and speaker notes are
  removed or labeled.
- Guard against decompression bombs.

## Code, repositories and agent skills

- Detect bidirectional control characters (Trojan Source) and homoglyphs.
- Flag instruction-like text in comments, READMEs, `AGENTS.md`, `.cursorrules` and skill manifests.
- Intake mode scans a repository, package or skill and returns a report, motivated by malicious skill
  campaigns such as
  [ClawHavoc (Koi Security, 2026)](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting).
- Writes to agent configuration files are egress (see [egress guard](egress-guard.md)).

## JSON, MCP results and agent messages

- Validate against a schema, drop unknown fields, label string fields, cap lengths, normalize text.
- Pin MCP tool definitions by hash on first use and flag later changes (rug pulls). Tool descriptions
  are served in sanitized form.
- Verify signed Agent Cards for Agent2Agent peers (Phase 3).

## Search results

Each result keeps its own provenance; titles and snippets pass the same normalization.

## Audio and video (Phase 4)

Transcription in a sandbox, frame sampling through the image pipeline, labeled subtitle tracks. The
"resample to what the consumer can perceive" principle applies to sample rate and resolution.
