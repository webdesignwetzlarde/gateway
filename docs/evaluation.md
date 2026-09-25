# Evaluation method

Kopist's claims must be measurable by anyone. This document defines how.

## Metrics

| Metric | Definition |
|---|---|
| Guarantee pass rate | Share of guarantee cases (hidden channels) where the forbidden content does not appear in the output. Target: 100 % in the `strict` profile |
| Attack success rate (ASR) | Share of attack cases where the agent performs the attacker's goal, measured end to end with and without Kopist |
| Relative ASR reduction | `1 - ASR_with / ASR_without`, reported per attack class |
| Utility retention | Share of benign tasks the agent completes with Kopist, relative to without |
| False positive rate | Share of benign documents that receive blocking decisions or lose content |
| Latency | p50 and p95 per format and profile |

Hidden-channel results and visible-instruction results are always reported **separately**
(see the [threat model](threat-model.md#5-the-central-split-hidden-channels-versus-visible-instructions)).

## Benchmarks and adapters (planned)

| Benchmark | Scope | Source |
|---|---|---|
| AgentDojo | Tool-using agents, 97 tasks, 629 injection cases | [GitHub](https://github.com/ethz-spylab/agentdojo) |
| BrowseSafe-Bench | Injections embedded in realistic HTML | [arXiv 2511.20597](https://arxiv.org/abs/2511.20597) |
| AgentDyn | Open-ended tasks with helpful third-party instructions | [arXiv 2602.03117](https://arxiv.org/abs/2602.03117) |
| LivePI | Attacks across seven surfaces on real machines | [arXiv 2605.17986](https://arxiv.org/abs/2605.17986) |
| WASP | Web agent hijacking in a sandbox | [arXiv 2504.18575](https://arxiv.org/abs/2504.18575) |
| InjecAgent | Tool-integrated agents | [arXiv 2403.02691](https://arxiv.org/abs/2403.02691) |
| BIPIA | Indirect injection in five scenarios | [GitHub](https://github.com/microsoft/BIPIA) |
| WAInjectBench | Detectors for web agents | [arXiv 2510.01354](https://arxiv.org/abs/2510.01354) |

## Kopist corpus

The project maintains its own corpus in `bench/corpus/` (see [bench/README.md](../bench/README.md)):

- one or more cases per attack class in the threat model, across HTML, PDF and images;
- benign look-alike cases to measure false positives (for example legitimately hidden navigation
  menus, accessibility text, documents with comments);
- every case links to the public source of the technique.

Every new public attack technique becomes a corpus case, and guarantee cases are CI gates.

## Adaptive evaluation

Static benchmarks overstate robustness. Following
[Nasr, Carlini et al. (2025)](https://arxiv.org/abs/2510.09023), each release is also evaluated with
adaptive attacks that know Kopist's design and may optimize against it. Detector results are expected
to degrade under adaptive attack; guarantees are not.

## Reporting rules

- Publish configuration, model versions, profile, corpus version and raw results.
- Report utility next to every security number.
- Never merge hidden-channel and visible-instruction results into one "protection rate".
- Report negative results.
