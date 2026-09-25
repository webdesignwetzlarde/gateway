# Documentation style guide

## Language

- English (US spelling) is normative for all documentation and the specification.
- German summaries live in `docs/de/`. They are non-normative. If they differ from the English text,
  the English text wins. Further translations follow the same pattern (`docs/<language-code>/`).
- Use the terms from the [glossary](glossary.md) and, for German, [docs/de/glossar.md](de/glossar.md).

## Claims and sources

- Every factual claim about attacks, tools, standards, laws or numbers needs a link to its source,
  preferably the primary source.
- Date sources where the date matters (`2025-08-21`).
- Mark unverified statements explicitly as unverified, or leave them out.
- Never claim that Kopist "prevents" or "solves" prompt injection. Describe guarantees, labels and
  measured effects instead.

## Wording

- Short sentences, active voice, concrete nouns.
- No marketing superlatives.
- Use a colon, comma or parentheses instead of em dashes.
- Normative words in the specification follow
  [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) and
  [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174) (MUST, SHOULD, MAY) and appear in capitals only
  when normative.

## Formatting

- One sentence or clause per line is not required; wrap prose at roughly 100 characters.
- Use tables for comparisons and registries, Mermaid for diagrams.
- Dates in ISO 8601 (`2026-09-25`).
- File and directory names in code formatting.
