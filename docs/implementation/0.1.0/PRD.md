# Squire v0.1.0 — Product Requirements Document

Status: **DRAFT R2 — SUCCESSOR ADVERSARIAL REVIEW REQUIRED / NOT FROZEN**

## 1. Product

Squire is a **Harness-independent work-offloading core with Harness-specific integration adapters**.

It does not replace the primary Coding Agent/Harness. For bounded supported work, it decides whether to:
- leave the work to the Harness/strong Agent;
- offload it to a lower-cost capability;
- use a bounded composition of capabilities.

Product statement:

> Let the Agent do only work that actually requires Agent-level intelligence.

"Harness-independent" describes **core semantics and policy ownership**, not shallow integration. A Harness adapter may be deeply integrated with lifecycle hooks while remaining policy-thin.

## 2. Target user and adoption hypothesis

Primary v0.1 user:
- developer/team already using coding-agent Harnesses;
- willing to run a local companion runtime;
- values reduced Agent work/cost without accepting correctness loss.

Product-adoption hypothesis:
- users will tolerate a separate companion process only if installation/configuration/compatibility burden remains small relative to measured benefit.

This operational burden is a Product gate, not an implementation afterthought.

## 3. Problem / gap

Offloadable work exists, and OpenSpace/SoL-Pi demonstrate adjacent solutions.

Squire does **not** claim novelty from:
- being external to one Harness;
- being cross-Agent;
- exposing tools/Skills/MCP;
- context reduction;
- generic runtime/capability management.

The independent product hypothesis is the following combined layer:

1. normalize a bounded Work request across supported Harnesses;
2. decide whether strong-Agent execution is warranted;
3. choose among eligible providers under explicit safety/net-value constraints;
4. preserve exact/recoverable evidence;
5. attribute task-level outcome/resource cost across Harnesses.

## 4. Goals

- **Quality first:** no efficiency result may override correctness/security/evidence gates.
- **Reduce Agent work:** lower total measured task resource cost after all Squire overhead.
- **Harness-independent core:** same core Work/Decision/Evidence policy for at least one Work class across two Harnesses.
- **Composition before reinvention:** wrap/reuse mature providers.
- **Measurable ROI:** decisions and outcomes must be attributable enough for matched evaluation.
- **Safe failure:** side-effect uncertainty must never be converted into blind retry.

## 5. Non-goals

v0.1 will not:
- implement a primary coding Agent or own reasoning/planning/conversation;
- implement general multi-agent orchestration;
- build a generic Skill/MCP marketplace;
- build a general sandbox platform;
- recreate mature search/LSP/docs/runtime systems without evidence;
- claim every Harness supports every Work class;
- patch/fork Harness cores to manufacture portability;
- perform uncontrolled online self-modification.

## 6. Core contracts

### 6.1 Work

A provider-neutral bounded unit that may otherwise consume Agent effort.

v0.1 primary Work families:
- **W1 Repository Lookup** — read-only cross-Harness primary.
- **W2 Post-change Mechanical Validation** — deterministic/fusion secondary.

Environment/toolchain preflight is a supporting capability, not a third primary acceptance Work class.

### 6.2 Capability

A provider/binding able to satisfy a Work class.

Required semantics:
- supported Work class;
- current availability/currentness;
- activation/startup cost;
- expected execution/context cost;
- permission/effect class;
- evidence/recovery behavior;
- provider identity/version;
- deterministic/idempotent characteristics where relevant.

### 6.3 Decision

One of:
```text
HARNESS
OFFLOAD(provider)
COMPOSE(bounded providers)
NO_ACTION
RECONCILE
```

Every adaptive decision must carry a reason code and enough predicted/observed cost data for evaluation.

v0.1 policy may be deterministic/rule-based. A learned model is explicitly not required.

### 6.4 Evidence

Result returned to the Harness:
- bounded Agent-facing result;
- provider identity/version;
- uncertainty/failure class;
- raw/exact recovery reference when material;
- effect/reconciliation state for mutations;
- timing/resource metadata needed for evaluation.

### 6.5 Outcome

Task-level evaluation record:
- accepted success/failure;
- correctness/security gate status;
- strong-model calls;
- fresh/cached tokens when available;
- tool calls;
- wall-clock;
- provider and Squire overhead;
- retries/re-expansion/reversal;
- failed/abandoned attempt cost;
- Harness / model / provider configuration identity.

## 7. Harness integration contract

A Harness is supported **per Work class**.

Minimum W1 semantics:
1. supported capability registration/invocation or equivalent supported interception;
2. stable run/task correlation;
3. project/cwd identity and bounded request handoff;
4. permission propagation without widening;
5. bounded result admission;
6. exact/raw recovery when applicable;
7. sufficient accounting for matched resource evaluation.

Side-effecting Work additionally requires:
8. effect identity/reconciliation sufficient to distinguish success, pre-effect failure and `UNKNOWN_EFFECT`.

If these semantics are unavailable without patching/forking the Harness, that Harness is **UNSUPPORTED for that Work class**.

Adapter rule:

> Adapters may be integration-deep but MUST remain policy-thin.

Adapters may own host API translation, lifecycle hookup, serialization and correlation. They MUST NOT own provider selection, ROI thresholds, evidence correctness rules or Product benchmark policy.

v0.1 must demonstrate W1 through two Harnesses. Initial L2 candidates remain **Codex + Pi**, but L2 may replace either only with explicit current-interface evidence and Product-equivalent coverage.

## 8. Provider strategy

At least one primary Work class must have >=2 eligible competing providers under one Work contract.

Candidate W1 providers:
- low-cost text/search provider (e.g. rg/native indexed text search);
- semantic provider (e.g. LSP or code-graph/search provider).

Candidate W2 provider/mechanism:
- deterministic command fusion/validation inspired by SoL-Pi Action Fusion.

OpenSpace:
- may be a Skill/capability/evidence provider or reusable reference;
- is not a mandatory dependency.

SoL-Pi:
- is the primary fixed-efficiency reference/baseline for Pi;
- mechanisms may be reused/adapted where the exact license/API boundary permits;
- Squire does not reimplement them merely for ownership.

## 9. Decision and ROI policy

Conceptual net value:

```text
ExpectedNetValue =
  ExpectedAgentWorkSaved
  - ActivationCost
  - ExecutionCost
  - ContextCost
  - LatencyCost
  - FailureRiskCost
```

Safety/correctness gates precede this calculation.

Rules:
- capability existence is not permission to offload;
- negative/borderline expected net value => `HARNESS`;
- stale/unknown-currentness state cannot justify a currentness-dependent offload;
- insufficient telemetry cannot justify an adaptive ROI claim;
- historical ROI is evidence, not correctness authority.

## 10. Failure and fallback semantics

Canonical v0.1 failure classes:

- `UNSUPPORTED_INTEGRATION`
- `STALE_OR_UNKNOWN_STATE`
- `PROVIDER_UNAVAILABLE`
- `EVIDENCE_INTEGRITY_FAILURE`
- `PERMISSION_MISMATCH`
- `INSUFFICIENT_TELEMETRY`
- `NON_POSITIVE_OR_BORDERLINE_VALUE`
- `UNKNOWN_EFFECT`

Fallback policy:
- read-only/pre-effect failures may defer to the normal Harness path when permissions/evidence remain valid;
- evidence reducer/handle failure returns original/exact evidence or fails closed;
- permission mismatch always fails closed;
- `UNKNOWN_EFFECT` **MUST stop automatic retry/fallback mutation** and enter reconciliation/verification before any retry;
- Squire MUST NOT represent unknown side-effect completion as ordinary failure.

## 11. MVP scope

Smallest complete v0.1 loop:

```text
Harness Adapter
 -> Work
 -> Eligibility / state
 -> Decision
 -> Provider
 -> Evidence / recovery
 -> Outcome telemetry
```

Mandatory:
- W1 through two supported Harness adapters;
- >=2 W1 providers under the same contract;
- W2 one deterministic/fusion implementation;
- safe fallback/failure semantics;
- matched benchmark harness.

Optional / deferred:
- generalized log reducer;
- general environment-preflight product surface;
- learned decision model;
- Skill marketplace;
- broad provider ecosystem.

## 12. Frozen benchmark contract requirements

The exact benchmark subject/task list is frozen during L2 before candidate evaluation, but the Product-level rules below are already mandatory.

### Sampling / split

- predeclare task source and inclusion/exclusion;
- predeclare Harness/model versions and resource budget;
- use a calibration split for provider/policy selection;
- use a held-out evaluation split for primary claims;
- do not tune on held-out results.

### Offload classification

Freeze a rubric before candidate outcome evaluation. Include harmful/non-offloadable negative examples.

### Stochastic treatment

Use matched tasks/configuration; repeat runs where stochasticity is material. Predeclare aggregation and uncertainty method.

### Quality guardrail

For v0.1 exploration:
- accepted task-success non-inferiority margin: **5 percentage points** vs matched baseline;
- any critical correctness/security/evidence-integrity regression: **automatic FAIL**.

Efficiency is evaluated only after quality passes.

### Resource accounting

Include:
- all strong-model/API resource cost;
- all failed/retried/abandoned attempts;
- Squire startup/runtime/provider overhead;
- provider activation/index costs amortized only by a predeclared rule;
- wall-clock and tool calls as secondary dimensions.

Primary economic metric:

> **total measured resource cost per accepted successful outcome**, with all attempt costs included.

Report success rate separately; cost-per-success never hides failure-rate regression.

### Adaptive baseline

Adaptive Squire must beat or justify itself against the **best fixed policy selected on calibration** using the same eligible providers and budget.

### Reporting

Report:
- aggregate;
- per Harness;
- per Work class;
- uncertainty/confidence;
- negative outcomes and fallback/reconciliation frequency.

Post-hoc winners on held-out data are exploratory only and cannot satisfy the primary Product gate without independent revalidation.

## 13. Product acceptance gates

### PG1 — Agent Work Waste / materiality
Using the frozen rubric/sample, >=25% of measured W1/W2-relevant Agent tool/turn resource is safely offloadable/avoidable.

### PG2 — Net Work reduction
At least one primary Work class shows >=20% net reduction in targeted resource cost after all Squire overhead while passing quality.

### PG3 — Harness-independent core
W1 runs through two supported Harness adapters under one core Work/Decision/Evidence policy; adapter-specific policy duplication is not allowed.

### PG4 — Adaptive value
At least one predeclared W1 stratum shows repeatable positive delta over the calibration-selected best fixed policy with the same provider set/budget. If not, adaptive selection is rejected/narrowed rather than declared successful.

### PG5 — Evidence integrity
Exact/raw recovery succeeds for all primary validation paths that use reduction/handles; mismatch is correctness FAIL.

### PG6 — Operational burden
Measure install/start/config/update and idle/runtime resource burden. Dogfood must show the companion runtime can remain enabled in normal use without operational burden erasing the measured benefit.

## 14. Strategic durability hypothesis

Potential durable value is in:
- cross-Harness compatibility corpus;
- Work/Capability/Evidence/Outcome normalization;
- provider benchmark/currentness data;
- outcome attribution;
- safety/recovery semantics;
- transferable selection policy.

If those assets do not become transferable across Harnesses, Squire SHOULD narrow into a smaller library/provider set or upstream mechanisms instead of becoming generic integration glue.

## 15. Reference subjects

- `HKUDS/OpenSpace@38277815ed44a53d757973c2bc4454c3b6426698`
- `NVlabs/SoL-Pi@03529c710269d3e3336a033cead49a95189987f1`

They are evidence/reference subjects, not Squire authority and not mandatory dependencies.

## 16. L2 obligations after Product Freeze

L2 must prove, before implementation expansion:
- supported current integration surfaces for both selected Harnesses;
- W1 same-core feasibility without patching/forking;
- provider interchangeability for W1;
- effect/reconciliation model for W2;
- telemetry/accounting feasibility;
- benchmark exact subject/task split and statistical treatment;
- REUSE / WRAP / EXTEND / NEW / NOT_NEEDED disposition for OpenSpace and SoL-Pi components.

## 17. Freeze rule

This PRD is **NOT FROZEN**.

Freeze requires:
1. fresh independent adversarial review of the successor exact SHA/tree;
2. 0 unresolved P0/P1;
3. durable Product Freeze checkpoint on Issue #1;
4. explicit L2 authorization only after that checkpoint.
