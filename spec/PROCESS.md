# Specification process

## Roles

- **Editors** maintain the specification text and decide on changes by consensus. Each version lists
  its editors. Editors should come from more than one organization; the project actively invites
  editors from other implementations.
- **Contributors** propose changes through issues and pull requests.

## Maturity levels

| Level | Meaning | Change policy |
|---|---|---|
| draft | Open for review, may change completely | Any change after discussion |
| candidate | Feature-complete, waiting for implementation experience | Only changes required by implementation findings |
| stable | Two independent interoperable implementations and a conformance suite exist | Only backward-compatible changes, or a new major version |

## Proposing a change

1. Open an issue with the "design proposal" template and the label `spec`.
2. Discussion stays open for at least 7 days (14 days for breaking changes).
3. Editors record the decision in the issue. Accepted changes land as pull requests that update
   prose, schemas, examples and test vectors together.

## Versioning

- Semantic versioning. Draft versions carry the suffix `-draft`.
- Receivers ignore unknown fields ("must-ignore"), except fields listed in `crit`, which they must
  understand or reject.
- A new major version gets a new predicate type URI and a new schema directory. Old versions stay
  published.

## Intellectual property

Contributions to prose are licensed under CC-BY-4.0, contributions to schemas and test vectors under
Apache-2.0. Before v1.0, the project will adopt an explicit patent commitment for specification
contributions, modeled on an established royalty-free policy, and document it here.

## Path to standardization

| Target | Step |
|---|---|
| 2026 Q4 | v0.1-draft published for review |
| 2027 H1 | Experimental MCP extension; proposal for OpenTelemetry attributes |
| 2027 H2 | Individual Internet-Draft for the HTTP field and receipt media type |
| 2028 | v1.0 with conformance suite; transfer to a neutral standards home |
