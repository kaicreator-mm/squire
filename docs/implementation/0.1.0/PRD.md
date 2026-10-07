# Squire v0.1.0 — Product Requirements Document

Status: **DRAFT — ADVERSARIAL REVIEW REQUIRED / NOT FROZEN**

## 1. Product

Squire is a **harness-agnostic work-offloading runtime for AI agents**.

Squire does not replace Claude Code, Codex, Pi, OpenCode, or another primary Harness. It sits outside the Harness and attempts to route bounded work to cheaper, more deterministic capabilities when doing so is expected to reduce successful-task cost.

Product statement:

> Let the Agent do only work that actually requires Agent-level intelligence.

## 2. Target user

Primary v0.1 user:

- developer or engineering team already using one or more coding-agent Harnesses;
- willing to run a local companion runtime;
- experiences repeated repository exploration, environment/tool discovery, verbose observations, mechanical validation, or capability-selection overhead.

Secondary future user:

- Agent/Harness vendors that want to consume Squire as an external optimization/runtime service.

## 3. Problem

Modern Agent Harnesses expose increasingly capable models and tools, but substantial task work is still performed through frontier-model turns even when the work is deterministic, searchable, parseable, cached, mechanically verifiable, or better served by a specialized capability.

The market already contains strong individual capabilities and Harness-specific efficiency mechanisms. The missing product hypothesis is a neutral layer that decides **whether the primary Agent should do the work at all**.

## 4. Goals

### G1 — Reduce strong-Agent work

For supported work classes, reduce at least one of:

- model calls;
- fresh input tokens;
- tool round-trips;
- redundant reads/searches;
- avoidable retries;
- wall-clock;
- total cost per successful task.

### G2 — Preserve task quality

Efficiency is subordinate to success/correctness. Raw evidence or exact recovery paths must remain available when a reducer/summary/handle is used.

### G3 — Stay Harness-agnostic

Core Work/Capability/Decision/Evidence contracts MUST NOT depend on one Harness. Harness-specific behavior belongs in thin adapters.

### G4 — Compose mature capabilities

Prefer wrapping mature tools/runtimes to rebuilding them. OpenSpace and SoL-Pi are reference subjects, not mandatory dependencies.

### G5 — Make ROI measurable

Every offload decision used in evaluation must be attributable enough to compare decision cost with observed task outcome.

## 5. Non-goals

v0.1 will not:

- implement a primary coding Agent;
- own the reasoning/planning/conversation loop;
- implement general multi-agent orchestration;
- build a generic Skill/MCP marketplace;
- build a general cloud sandbox platform;
- replace mature LSP/search/docs/runtime tools without demonstrated requirement;
- perform uncontrolled online self-modification;
- claim universal benefit across all Harnesses/tasks.

## 6. Core product contracts

Exact schemas are an L2 decision, but v0.1 must preserve these semantic boundaries.

### Work

A bounded unit that may otherwise consume Agent effort.

Examples:

- repository text search;
- symbol/reference lookup;
- project/toolchain inspection;
- documentation lookup;
- test/build log reduction;
- deterministic post-change validation;
- repeated observation recall.

A Work request describes intent and constraints, not a provider-specific command.

### Capability

A mechanism capable of satisfying one or more Work classes.

A Capability exposes at least:

- work classes supported;
- availability/currentness;
- startup/activation cost;
- expected execution cost;
- evidence/recovery behavior;
- permission/security requirements;
- provider identity.

### Decision

A selection result that answers:

```text
execute in Agent
or
offload to Capability X
or
compose bounded Capabilities X -> Y
```

The decision must support a reason code and measurable costs. v0.1 may be rule-based; a learned decision model is NOT required.

### Evidence

The result returned to the Harness, including:

- bounded Agent-facing result;
- provider identity;
- raw/exact recovery reference when applicable;
- failure/uncertainty state;
- timing/cost metadata needed for evaluation.

### Outcome

Task-level result used to judge the decision policy. v0.1 should capture:

- success/failure;
- strong-model calls;
- fresh/cached tokens when available;
- tool calls;
- wall-clock;
- Squire overhead;
- retries/reversal/re-expansion when observable.

## 7. Harness adapter boundary

The adapter layer is intentionally thin.

Adapters may translate:

```text
Harness event -> Squire Work/Observation event
Squire result/action -> Harness-visible result
```

Adapters MUST NOT own:

- provider selection policy;
- core ROI logic;
- cross-Harness runtime state;
- product truth/evaluation policy.

v0.1 target: at least two Harness adapters before claiming Harness-general support.

Initial candidates: **Codex + Pi** because they provide a useful contrast and SoL-Pi supplies a Pi efficiency baseline. Final selection is an L2 current-capability decision.

## 8. Capability/provider strategy

Providers are replaceable.

Initial candidate families:

1. Repository/code:
   - native search/rg;
   - LSP;
   - AST search;
   - optional CodeGraph/Sourcegraph-class provider.

2. Knowledge/docs:
   - local project docs;
   - optional Context7-class provider.

3. Observation/evidence:
   - deterministic parsers;
   - observation handle/raw recovery;
   - optional reducer model.

4. Environment/preflight:
   - repository/toolchain/version detection;
   - command/test/build discovery.

5. External runtime:
   - OpenSpace may be evaluated as a capability/skill/evidence provider;
   - SoL-Pi mechanisms may be reused/adapted where license/API boundaries permit.

No provider is mandatory until L2 evidence supports it.

## 9. Decision policy

v0.1 uses deterministic/rule-based policy first.

Conceptual objective:

```text
ExpectedNetValue =
  ExpectedAgentWorkSaved
  - ActivationCost
  - ExecutionCost
  - ContextCost
  - LatencyCost
  - FailureRiskCost
```

The exact weighting is experimental.

Policy requirements:

- fail open to normal Harness behavior when Squire cannot make a safe decision;
- no offload solely because a capability exists;
- no compression/reduction solely because output is large;
- historical ROI is evidence, not correctness authority.

## 10. MVP scope

v0.1 MVP should implement the smallest complete loop:

```text
Harness Adapter
      ->
Work classification
      ->
Capability availability + selection
      ->
Provider execution
      ->
Evidence return/recovery
      ->
Outcome telemetry
```

Required MVP work classes:

- repository exploration / lookup;
- environment preflight;
- observation/log handling;
- one mechanical validation/fusion pattern.

At least one work class must support more than one competing provider so selection behavior is real rather than hard-coded dispatch.

## 11. Benchmark / acceptance

### Baselines

Where technically applicable:

- Baseline A: unmodified Harness;
- Baseline B: fixed efficiency mechanisms/profile, including SoL-Pi-style baseline for Pi;
- Candidate: Harness + Squire adaptive offloading.

### Product acceptance

v0.1 Product validation must demonstrate:

1. **Quality:** no material correctness regression on the frozen primary benchmark; exact threshold to be frozen after L2 statistical-design evidence.
2. **Net efficiency:** positive reduction in successful-task cost/work after including Squire overhead.
3. **Materiality:** at least one primary work class shows >=20% net reduction in its targeted work metric on matched tasks.
4. **Harness independence:** the same core contracts execute through at least two Harness adapters without separate core implementations.
5. **Adaptive value:** at least one task class demonstrates a repeatable advantage over a fixed always-on provider/mechanism, or the adaptive-selection hypothesis is explicitly rejected.
6. **Recoverability:** reducer/handle paths used in validation can recover exact/raw evidence as designed.

No token/compression metric alone can satisfy product acceptance.

## 12. Product gates

### PG1 — Agent Work Waste Study

Required before claiming broad product value.

Output:

- work-class taxonomy;
- measured offloadable-work ratio;
- harmful-offload counterexamples;
- Harness-specific vs Harness-independent patterns.

### PG2 — Harness interception feasibility

L2 must prove that the chosen Harnesses expose enough supported hooks/interfaces to implement the adapter without patching/forking their core.

### PG3 — Provider composition feasibility

L2 must prove at least one real Work class can be served by interchangeable providers under one contract.

### PG4 — Evidence integrity

Reduction/handle/provider results must have explicit failure and recovery semantics.

### PG5 — Task-level benchmark

Release qualification requires real task-level evidence, not component microbenchmarks only.

## 13. Reference subjects

- `HKUDS/OpenSpace@38277815ed44a53d757973c2bc4454c3b6426698`
- `NVlabs/SoL-Pi@03529c710269d3e3336a033cead49a95189987f1`

These references inform L2 and implementation selection. They do not own Squire product authority and must not silently become normative dependencies.

## 14. Explicit unknowns for adversarial review

- Is an external Harness-agnostic interception layer technically possible without losing the deep integration benefits of Harness-specific extensions?
- Does adaptive capability selection provide enough gain beyond a small fixed mechanism set?
- Can Squire obtain comparable telemetry across Harnesses without fragile provider-specific scraping?
- Does provider startup/indexing overhead erase the expected savings?
- Is the cross-Harness state useful enough to justify a persistent runtime?
- Can the core remain thin, or does provider/Harness compatibility dominate engineering cost?
- Should v0.1 target coding tasks only, or is the Work contract already generic enough to stay domain-neutral without expanding implementation scope?

## 15. Freeze rule

This PRD is **NOT FROZEN**.

It may be frozen only after:
- independent adversarial Product/L1 review;
- P0/P1 disposition;
- explicit decision on v0.1 Harness subjects and benchmark design;
- exact revision checkpoint on `version/v0.1.0`.
