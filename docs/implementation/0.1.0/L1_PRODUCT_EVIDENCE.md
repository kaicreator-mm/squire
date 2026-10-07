# Squire v0.1.0 — L1 Product Evidence

Status: **DRAFT — REVIEW REQUIRED / NOT FROZEN**

## 1. Product hypothesis

Squire is a **harness-agnostic work-offloading runtime for AI agents**.

It sits outside the primary Agent Harness and attempts to remove work from the strong-agent loop when the same work can be completed more cheaply and reliably by deterministic software, specialized tools, retrieval/index systems, or lightweight models.

Core thesis:

> Let the Agent do only work that actually requires Agent-level intelligence.

Optimization target:

```text
minimize:
  strong-model calls
  fresh input tokens
  tool round-trips
  repeated exploration
  avoidable retries
  wall-clock
  total task cost

subject to:
  task success
  correctness
  evidence recoverability
  permission/security boundaries
```

Implementation principle:

> Offload before optimize. Compose before reinvent. Measure at task outcome.

## 2. Primary reference subjects

The findings below are pinned to exact upstream subjects and MUST NOT be silently updated when upstream repositories move.

### R1 — HKUDS/OpenSpace

- Repository: `HKUDS/OpenSpace`
- Subject: `38277815ed44a53d757973c2bc4454c3b6426698`
- Observed role: Skill Management Layer plus a substantially broader runtime surface.
- Relevant capabilities observed in source:
  - Agent Harness / turn loop / multi-agent orchestration;
  - capability profiles and active-tool limits;
  - tool inventory and deferred tools;
  - LSP integration;
  - sandbox, scheduler, memory, session recovery;
  - cost/latency profiling;
  - skill ranking with BM25 + embedding reranking;
  - evidence and skill evolution.
- Product center remains skill retrieval/evaluation/evolution and OpenSpace-owned execution/harness behavior.

Interpretation for Squire:

OpenSpace is strong evidence that a unified capability/runtime layer is practical and useful. It is also a likely provider/reference rather than functionality Squire should recreate wholesale.

### R2 — NVlabs/SoL-Pi

- Repository: `NVlabs/SoL-Pi`
- Subject: `03529c710269d3e3336a033cead49a95189987f1`
- Observed role: Pi-specific efficient-harness extension.
- Four explicit mechanisms:
  - Action Fusion;
  - ObservationPack;
  - Evidence-Preserving Reducer;
  - Online Context Compact.
- Source shows direct removal of model decisions/round-trips, recoverable raw observations, verified log reduction, and economic break-even logic for context compaction.

Interpretation for Squire:

SoL-Pi is direct evidence that meaningful portions of Agent work can be removed from a frontier-model loop without simply truncating the task. Its main boundary is that the mechanisms are implemented *inside and specifically for Pi*.

## 3. Problem evidence

### P1 — Strong Agents perform repeated work that does not always need frontier reasoning

The reviewed references demonstrate repeated classes of work that can be delegated or removed:

- predictable post-edit validation;
- replay of large historical observations;
- reading long diagnostic logs;
- context compaction decisions;
- tool/skill discovery;
- project and runtime support work.

SoL-Pi explicitly removes a model round-trip for edit/write + validation patterns. OpenSpace implements preselection/ranking, deferred capability behavior, and runtime support around its own agent.

This supports the existence of **offloadable work**.

### P2 — Specialized mechanisms can reduce work, but local optimization is not enough

A reducer, cache, index, or specialized tool has startup/runtime/context costs. Therefore an optimization is valuable only when:

```text
Expected Agent Work Saved
>
Offloading + Startup + Context + Failure Cost
```

SoL-Pi's compaction economics are strong supporting evidence for this form of decision.

### P3 — Existing capability ecosystems are fragmented

Relevant capabilities already exist as mature or maturing tools: LSP, rg, AST search, code graph/search, documentation retrieval, shell/output reducers, sandbox runtimes, skills, MCP servers, parsers, caches, and small decision/retrieval models.

The product opportunity is therefore not primarily to invent each capability. It is to decide **which work should leave the Agent loop, which capability should perform it, and whether doing so improved the completed task**.

## 4. User workflow evidence

A current advanced coding-agent environment commonly requires the user or harness to assemble multiple independent capabilities:

```text
Agent
+ code intelligence
+ repository search
+ docs retrieval
+ shell/runtime
+ skills
+ sandbox
+ evidence/log handling
+ hooks/plugins
```

OpenSpace shows one approach: bring many of those functions into a skill-centric agent runtime.

SoL-Pi shows another: install fixed efficiency mechanisms inside one harness.

Squire's proposed workflow differs:

```text
Existing Harness
      |
      v
thin harness adapter
      |
      v
Squire runtime
      |
      +--> deterministic provider
      +--> specialized tool
      +--> external capability runtime
      +--> lightweight model
      |
      v
bounded evidence/result
      |
      v
Existing Harness
```

The primary Agent remains owned by the user's chosen harness.

## 5. Alternatives / competitors

### OpenSpace

Strong overlap in capability/runtime/evidence infrastructure. It is broader and more mature than Squire would be initially, but its product center is Skill management/evolution plus its own Agent Harness.

### SoL-Pi

Highest overlap in efficiency philosophy. It directly reduces model turns/context work, but it is Pi-specific and operates inside that Harness.

### Native Harness features

Claude, Codex, Pi, OpenCode and future Harnesses can absorb tool search, context management, caching, programmatic tool use, skills and environment logic.

This is the largest strategic threat to any isolated Squire feature.

### Single-purpose capabilities

Context/document retrieval, code intelligence, code graph, reducers, sandbox/runtime and skill systems may each remove enough friction that a separate composition layer provides little additional value.

## 6. Gap finding

The reviewed evidence supports a narrower gap:

> A vendor-neutral, harness-agnostic runtime that classifies work before it consumes strong-Agent effort, selects an appropriate lower-cost capability, preserves recoverable evidence, and measures task-level ROI across Harnesses.

This L1 does **not** claim no other project implements any part of this gap.

The differentiating combination under test is:

```text
Harness-agnostic
+
Work classification
+
Offloading
+
Capability/provider selection
+
Economic/ROI gating
+
Task-outcome attribution
```

## 7. Counter-evidence

### C1 — Harness vendors can internalize the best mechanisms

If Claude/Codex/Pi natively implement high-quality offloading and expose no useful interception boundary, Squire may become redundant or limited to a small set of Harnesses.

### C2 — Adaptive composition may not beat fixed mechanisms

SoL-Pi may already capture most of the economically valuable work with a handful of simple mechanisms. A dynamic resolver adds complexity and can make outcomes worse.

### C3 — Integration cost can dominate product value

Supporting many Harnesses and providers can turn the project into adapter maintenance. The core MUST stay thin and provider contracts MUST prevent per-Harness logic from leaking inward.

### C4 — Efficiency optimization can reduce task success

Removing context, changing execution paths, or substituting specialized tools may hide useful evidence or cause incorrect decisions. Success/correctness MUST dominate efficiency.

### C5 — Attribution is difficult

Agent outputs are stochastic. A lower token/tool count in one run does not prove Squire caused an improvement. Matched baselines and repeated tasks are required.

## 8. Product shape finding

Squire should own:

- Work classification;
- Capability abstraction / provider binding;
- Offloading decisions;
- Bounded composition;
- Evidence shaping and exact/raw recovery references;
- Task-level efficiency telemetry;
- Cross-Harness runtime state required for these functions.

Squire should not own:

- Primary reasoning/planning loop;
- Conversation UX;
- Sub-agent orchestration;
- General Skill marketplace;
- General sandbox product;
- A replacement implementation of mature tools without demonstrated need.

## 9. Key assumptions / validation gates

### G1 — Offloadable Work Exists

A material portion of real Agent task cost is attributable to work that can be completed by cheaper/deterministic capabilities.

Initial target: **>=25% of measured tool/turn work on the selected benchmark set is plausibly offloadable.**

### G2 — Offloading Pays

For at least one bounded work class, Squire produces a repeatable net reduction in work/cost while preserving task success.

### G3 — Harness Independence Is Real

The same Work/Capability contracts can operate across at least two Harnesses without duplicating the core.

### G4 — Adaptive Selection Adds Value

Task/state-aware selection must show positive delta over a fixed always-on mechanism/profile on at least one meaningful task class.

### G5 — Quality Dominates Efficiency

Efficiency improvement with unacceptable success/correctness regression is a FAIL regardless of tokens saved.

### G6 — Runtime Overhead Is Bounded

Squire's own startup, memory, latency and context overhead must be measured and included in ROI.

## 10. Required pre-PRD / PRD validation

The first evidence campaign should produce an **Agent Work Waste Study**:

- 50–100 real coding-agent task traces if practical;
- at least two Harnesses before claiming Harness-general value;
- classify actions into core reasoning/implementation vs repo discovery, environment discovery, docs discovery, repeated reads/search, output filtering, mechanical verification, avoidable retries and other;
- quantify offloadable work by task, Harness and work class;
- retain negative examples where offloading would have been harmful.

The first MVP benchmark should compare:

```text
Baseline Harness
vs
Harness + fixed SoL-Pi-style mechanisms where applicable
vs
Harness + Squire adaptive offloading
```

## 11. Recommendation

**PROCEED_TO_PRD — EVIDENCE-GATED**

The product problem is sufficiently supported to write a PRD and perform L2.

However, the independent product case is not yet proven. v0.1 must demonstrate that a Harness-agnostic offloading layer creates measurable value beyond:
1. native Harness optimization; and
2. a small fixed set of SoL-Pi-style mechanisms.

Failure to establish that delta is a valid product outcome and may lead to narrowing Squire into a smaller library/provider set or contributing mechanisms upstream instead.
