# Typed extraction and handles

## Idea

The safest way for a privileged agent to use untrusted text is to not read it. Kopist implements the
quarantined-reader idea from the
[Dual LLM pattern (Willison, 2023)](https://simonwillison.net/2023/Apr/25/dual-llm-pattern/) and its
formalization in [CaMeL](https://arxiv.org/abs/2503.18813) and the
[design patterns paper](https://arxiv.org/abs/2506.08837) as a local service.

## API sketch

```text
extract(content_handle, json_schema, trusted_instructions) -> typed_result
```

- `content_handle` refers to reconstructed content already in the store.
- `json_schema` constrains the result: enums, numeric ranges, date formats, regular expressions,
  maximum string lengths, URL allowlists.
- `trusted_instructions` come from the privileged side only.
- `typed_result` is validated against the schema by the trusted core, labeled with provenance and
  taint, and linked to the parent receipt.

## The quarantined reader

- A model with **no tools, no network and no secrets**.
- Runs locally (for example through llama.cpp or Ollama) or at an OpenAI-compatible endpoint the user
  configures.
- Its output is untrusted until it passes schema validation. Free-text fields remain labeled as
  untrusted prose.

## Handles

The privileged agent can receive an opaque handle plus metadata instead of content. With a handle it
can:

- request a typed extraction;
- ask Kopist to show the content to the **user** without the model reading it;
- pass the handle to a tool, subject to the egress guard's data-flow check.

## Label flow

- Low-entropy typed values (enums, bounded numbers, dates) may flow where untrusted prose may not,
  if policy allows. Their integrity label is unchanged; policy decides what they may influence.
- Any value derived from untrusted content keeps its taint through the session.

## Limits

Full control-flow integrity in the style of CaMeL requires owning the agent's planner. Kopist provides
building blocks (handles, labels, policy, SDK helpers). Complete enforcement arrives as agent
frameworks integrate them.
