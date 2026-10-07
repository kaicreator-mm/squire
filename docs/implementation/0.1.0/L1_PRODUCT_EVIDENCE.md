# Squire v0.1.0 — L1 Product Evidence

Status: **DRAFT R2 — SUCCESSOR REVIEW REQUIRED / NOT FROZEN**

Review lineage:
- R1 subject: `08d284c788d799e08f5ae54743c393237d2ce7a0` / tree `4fb134a97a9b221e91a8bf4b5f8ce643ccb15980`
- R1 Fresh Adversarial Review: Issue #2 — `CHANGES_REQUESTED / NARROW / 0×P0 / 4×P1 / 3×P2`
- R2 incorporates all four P1 dispositions and intentionally adopts P2 scope narrowing.

## 1. Product hypothesis

Squire is a **Harness-independent work-offloading core with Harness-specific integration adapters**.

It runs outside the primary Agent reasoning/planning loop. For bounded work that a supported Harness can expose safely, Squire decides whether the work should remain in the strong-Agent loop or be delegated to a cheaper deterministic/specialized capability.

Core thesis:

> Let the Agent do only work that actually requires Agent-level intelligence.

Squire does **not** claim shallow or uniform integration across Harnesses. A supported Harness may require deep lifecycle integration, but Harness-specific integration MUST remain translation/integration logic rather than owning Squire's provider-selection, ROI, evidence, or product policy.

Optimization target:

```text
first satisfy:
  task success / correctness / permission / evidence integrity

then minimize:
  strong-model calls
  fresh input tokens
  tool round-trips
  repeated exploration
  avoidable retries
  wall-clock
  total measured task cost
```

Implementation principle:

> Offload before optimize. Compose before reinvent. Measure at task outcome.

## 2. Primary reference subjects

The findings below are pinned to exact upstream subjects and MUST NOT be silently updated.

### R1 — HKUDS/OpenSpace

- Repository: `HKUDS/OpenSpace`
- Subject: `38277815ed44a53d757973c2bc4454c3b6426698`
- OpenSpace is already an external, cross-Agent companion layer: its pinned README explicitly targets Claude Code, Codex, OpenClaw and other MCP/Skill-capable hosts.
- It provides local/remote MCP transport, Skill discovery/retrieval, task execution, quality/evidence records, evolution, and a broad internal runtime surface including LSP, sandbox, memory, scheduler, capability profiles, tool inventory and latency/cost support.

### R2 — NVlabs/SoL-Pi

- Repository: `NVlabs/SoL-Pi`
- Subject: `03529c710269d3e3336a033cead49a95189987f1`
- SoL-Pi is a Pi-specific efficient-Harness extension.
- Its four explicit mechanisms are Action Fusion, ObservationPack, Evidence-Preserving Reducer and Online Context Compact.
- It demonstrates direct removal of model round-trips, recoverable observation handles, verified log reduction and economic break-even logic.

## 3. Explicit overlap / non-overlap

| Concern | OpenSpace | SoL-Pi | Squire hypothesis |
|---|---|---|---|
| Cross-Agent external companion shape | **YES** | NO | YES |
| Skill retrieval/evaluation/evolution | **CORE** | NO | Provider/reference, not core |
| Own Agent Harness / task execution | **YES** | Extends Pi | NO |
| Harness-specific efficiency mechanisms | Partial | **CORE** | May consume/reuse |
| Decide Agent vs lower-cost provider | Partial/indirect | Fixed mechanisms | **CORE hypothesis** |
| Compare heterogeneous providers for same bounded Work | Not product center | NO | **CORE hypothesis** |
| Explicit net-value / safety gate per offload | Partial profiles | Compaction-specific | **GENERALIZED hypothesis** |
| Exact/recoverable evidence | YES in evidence flows | YES | **CORE contract** |
| Normalize task-level ROI across Harnesses | Not product center | NO | **CORE hypothesis** |

Therefore **Harness-agnosticism/external deployment alone is NOT the differentiator**.

The claimed gap is the combination:

1. decide whether bounded work should remain in the strong-Agent loop at all;
2. choose among Agent/provider alternatives under explicit net-value and safety constraints;
3. preserve exact/recoverable evidence;
4. normalize task-level outcome/ROI across supported Harnesses.

## 4. Problem evidence

### P1 — Offloadable work exists

The pinned references provide direct examples of work that need not consume a full strong-model turn or full repeated context:
- predictable post-edit validation;
- repeated replay of large observations;
- long diagnostic-log reading;
- context-compaction decisions;
- skill/tool retrieval and preselection.

This establishes existence, not yet market-wide percentage.

### P2 — Offloading has economics

Every reducer/index/provider has activation, execution, latency, context and failure cost.

A safe candidate is valuable only when:

```text
Expected Agent Work Saved
>
Activation + Execution + Context + Latency + Failure-Risk Cost
```

SoL-Pi's compaction economics are direct supporting evidence for this style of decision.

### P3 — Capability transport is increasingly commoditized

MCP/plugins/hooks/Skills make external capability access easier. Therefore transport, registry, tool enumeration and generic integration are weak standalone moats.

Squire must prove value in **decision/evidence/outcome policy**, not tool plumbing.

## 5. Harness support contract — product-level minimum

A Harness is supported for a given Squire Work class only if an official/supported integration surface can provide the minimum semantics required by that class **without patching/forking the Harness core**.

For the v0.1 primary read-only Work class, minimum semantics are:

1. **Invocation** — register/invoke a Squire high-level Work capability or equivalent supported interception point.
2. **Correlation** — stable task/session/run correlation sufficient to attribute request/result/outcome.
3. **Context handoff** — pass the bounded task/work request and current working-directory/project identity.
4. **Permission propagation** — preserve relevant read/execute permission boundary; adapter MUST NOT widen authority.
5. **Result admission** — return bounded Squire evidence/result to the Harness with provider and uncertainty metadata.
6. **Recovery** — permit exact/raw recovery reference where the provider/reducer uses a handle/summary.
7. **Accounting** — expose or permit collection of enough usage/timing/tool data to compare matched resource cost.

For side-effecting Work classes, support additionally requires:
8. **effect identity / reconciliation** — enough semantics to determine whether an attempted mutation completed, failed before effect, or is `UNKNOWN_EFFECT`.

Falsification rule:

> If a selected Harness cannot expose the minimum semantics for a Work class through supported interfaces, that Harness is **UNSUPPORTED for that Work class**. Squire MUST NOT weaken the core contract or patch/fork the Harness merely to claim portability.

"Thin adapter" means **policy-thin**, not necessarily few lines of code. Adapters may implement deep host lifecycle translation but MUST NOT own provider-selection policy, ROI policy, evidence correctness rules, or benchmark/product policy.

## 6. v0.1 product scope

To reduce confounding, v0.1 validates only two primary Work families.

### W1 — Repository Lookup (cross-Harness primary)

Read-only bounded repository discovery/lookup.

The same core `Work -> Decision -> Evidence -> Outcome` semantics MUST execute through at least two Harness adapters.

At least two eligible providers must exist under the same contract, e.g. a low-cost text/search provider and a semantic provider. Exact providers are an L2 evidence decision.

### W2 — Post-change Mechanical Validation (secondary)

A deterministic/fusion pattern such as edit/write followed by an expected test/build/check command.

This validates that Squire can remove predictable Agent decisions, but it does not by itself prove cross-Harness generality.

Environment/toolchain preflight is a supporting capability for W1/W2, not a separate v0.1 product acceptance class.

Observation/log reduction remains a reference/provider candidate and may be used where needed, but is not a third mandatory primary work class.

## 7. Counter-evidence

### C1 — OpenSpace already proves an external cross-Agent layer
Therefore externality/cross-Agent shape is not novel.

### C2 — Fixed mechanisms may capture most ROI
SoL-Pi may already represent the practical optimum: a small number of carefully validated mechanisms instead of a general adaptive selector.

### C3 — High-value savings may require deep Harness hooks
The best mechanisms may not survive a lowest-common-denominator integration.

### C4 — Native Harnesses can absorb primitives
Tool search, context reduction, plugins, hooks, caching and semantic code support can move upstream.

### C5 — Efficiency can reduce task success
Any savings that violate the quality guardrail are invalid.

### C6 — A separate companion runtime creates operational burden
Installation, process lifecycle, permissions, version compatibility and provider configuration can erase user-perceived value even when benchmark cost improves.

## 8. Differentiation / durability hypothesis

The durable layer, if Squire succeeds, is expected to accumulate in:

- cross-Harness Work/Capability/Evidence/Outcome compatibility corpus;
- provider compatibility/currentness evidence;
- normalized task/outcome benchmark data;
- attribution of provider/offload decisions to task outcomes;
- safety/recovery semantics;
- rule-based or learned selection policy transferable across Harnesses.

If Squire does not accumulate transferable value in these areas, the product SHOULD be allowed to narrow into a smaller library/provider set or contribute mechanisms upstream rather than becoming permanent integration glue.

## 9. Failure semantics

Squire must classify failures before fallback.

| Failure class | Required behavior |
|---|---|
| `UNSUPPORTED_INTEGRATION` | Do not offload; use normal Harness path if safe |
| `STALE_OR_UNKNOWN_STATE` | Do not make currentness-dependent offload; refresh or defer to Harness |
| `PROVIDER_UNAVAILABLE` / pre-effect timeout | Read-only: safe fallback allowed; side-effect: only if proven no effect occurred |
| `EVIDENCE_INTEGRITY_FAILURE` | Reject reduced/derived evidence; return exact/original evidence or fail closed |
| `PERMISSION_MISMATCH` | Fail closed; never widen authority |
| `INSUFFICIENT_TELEMETRY` | No ROI-dependent adaptive claim; use predeclared safe policy or Harness path |
| `NON_POSITIVE_OR_BORDERLINE_VALUE` | Do not offload |
| `UNKNOWN_EFFECT` | STOP automatic retry; reconcile/verify effect state before any retry/fallback |

Exact/raw recovery integrity is a correctness requirement, not an efficiency metric.

## 10. Benchmark contract

The primary Product benchmark MUST be frozen before candidate evaluation and MUST resist post-hoc winner selection.

Required design:

1. **Sampling frame** — predeclare benchmark/task source, inclusion/exclusion rules, Harnesses and primary W1/W2 work classes.
2. **Frozen evaluation split** — policy/provider tuning occurs only on a calibration set; final Product claims use a held-out evaluation set.
3. **Offload rubric** — freeze a deterministic classification rubric before measuring candidate outcomes; retain negative/harmful-offload examples.
4. **Matched stochastic treatment** — same task set, Harness/model configuration and resource budget across policies; repeated runs where model stochasticity is material.
5. **Quality first** — primary non-inferiority margin for accepted task success is **5 percentage points** for v0.1 exploration; critical correctness/security regressions are zero-tolerance blockers. Efficiency is evaluated only after quality passes.
6. **All-attempt accounting** — total measured resource cost includes Squire startup/runtime overhead and costs of failed, retried or abandoned attempts.
7. **Adaptive baseline** — adaptive Squire must be compared with the **best fixed policy selected on the calibration set using the same eligible provider set and budget**, not an arbitrary fixed mechanism.
8. **Stratification** — report per-Harness and per-work-class results; aggregate gains cannot hide a failing stratum.
9. **Uncertainty** — report confidence intervals or other predeclared uncertainty treatment appropriate to sample size.
10. **No post-hoc gate promotion** — exploratory winners discovered on held-out evaluation do not satisfy the primary Product gate until independently revalidated.

Primary decision pair:

```text
A. task-success / correctness non-inferiority guardrail
B. total measured resource cost per accepted successful outcome,
   with all attempt and Squire overhead included
```

Tokens, context reduction and individual tool latency are secondary diagnostics.

## 11. Key gates

### G1 — Offloadable Work Exists
Using the frozen rubric and primary task sample, >=25% of measured Agent tool/turn resource attributable to W1/W2-relevant activity is classified as safely offloadable or avoidable. Harmful/unsafe examples remain explicit counterevidence.

### G2 — Offloading Pays
At least one primary Work class shows >=20% net reduction in its targeted measured resource cost on held-out matched evaluation after Squire overhead, while passing the quality guardrail.

### G3 — Harness-independent core semantics are real
W1 executes through at least two supported Harness adapters using the same core Work/Decision/Evidence semantics without duplicating provider-selection/ROI policy.

### G4 — Adaptive Selection Adds Value
On at least one predeclared W1 stratum, adaptive selection shows positive repeatable delta against the best calibration-selected fixed policy using the same provider set/budget. If not, the adaptive-selector hypothesis is rejected and v0.1 narrows to fixed/provider mechanisms.

### G5 — Evidence integrity
All reduced/handled evidence used for primary validation satisfies exact/raw recovery requirements; integrity mismatch is a correctness failure.

### G6 — Companion runtime burden is acceptable
Install/start/config/update overhead and runtime resource use are measured. The v0.1 dogfood result must show that the companion runtime can remain enabled for normal tasks without operational burden erasing the measured benefit.

## 12. Agent Work Waste Study

Before broad product-value claims, produce a study using the frozen work taxonomy and negative examples.

The study must distinguish:
- useful reasoning/implementation;
- repository discovery/lookup;
- environment/tool discovery;
- repeated reads/searches;
- output/log filtering;
- deterministic/mechanical validation;
- avoidable retries;
- unsafe/non-offloadable work.

At least two Harnesses are required before claiming cross-Harness generality.

## 13. Recommendation

**PROCEED_TO_SUCCESSOR_REVIEW — NARROWED / EVIDENCE-GATED**

The offloading problem is credible.

The independent Squire hypothesis is now explicitly narrower than OpenSpace, SoL-Pi and native Harness capability transport:

> cross-Harness normalization of bounded Work/Capability/Decision/Evidence/Outcome plus safe Agent-vs-provider selection and task-level ROI attribution.

Failure to prove transferability or adaptive value is a valid outcome and MUST narrow the product rather than forcing the original thesis.
