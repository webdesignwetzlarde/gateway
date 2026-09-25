# ADR 0004: Reconstruct, don't filter

- Status: accepted
- Date: 2026-09-25

## Context

Filters that remove known-bad patterns fail open when attackers find new encodings. Hidden channels
(CSS-hidden text, invisible Unicode, image-scaling payloads, document metadata, active content) are
numerous and keep growing.

## Decision

Kopist parses every input into a closed, versioned intermediate representation (IR) and emits output
only from that IR. Whatever the IR cannot represent is dropped. Unknown IR block types are dropped as
well. Original bytes are never passed through in reconstruction profiles.

## Consequences

- Entire classes of hidden channels are removed by construction and can be covered by guarantees.
- Some fidelity is lost (layout, interactivity, exotic formatting). Profiles and the diff view make
  the loss visible.
- Visible instructions are not removed by reconstruction; they are handled by labels, typed
  extraction and egress control (see the threat model).
- The IR is part of the trusted computing base and must stay small.
