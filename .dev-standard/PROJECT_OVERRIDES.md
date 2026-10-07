# Project Overrides

## Project Identity

- Repository: `kaicreator-mm/squire`
- Product: Squire
- Standard revision: read `.dev-standard/VERSION`

## Structure / Integration

- Repository profile: service/library hybrid; final implementation language is an L2 decision.
- Integration mode: version-branch
- Version branch pattern: `version/vX.Y.Z`
- Issue-based execution DAG: enabled after Task DAG freeze.
- Stacked PR: only for real unmerged code-baseline dependency.
- v4.adoption_level: A1_MANUAL_PROTOCOL
- v4.compatibility_mode: native-v4
- v4.interchange: disabled for v0.1 planning; harness adapters are product interfaces, not ADS authority transport.
- v4.reducer/controllers: disabled as ADS automation; Squire product runtime logic is separate from ADS lifecycle authority.
- v4.fast_path: canonical

## Review / Validation

- Review profile: risk-based
- Default Task Review Policy: recommended
- Required review triggers: public capability/work contracts; harness adapter mutation/interception semantics; security/permission boundaries; evidence integrity; telemetry attribution; benchmark methodology; release blocker.
- CI profile: minimal once implementation exists.
- Exact-SHA clean-validation fallback: trusted local Build Host / Local Agent, recorded against exact SHA.
- Linux validation: Ubuntu Build Host when implementation begins.
- Windows validation: Windows workstation when Windows adapter/runtime behavior is in scope.
- macOS validation: NOT_RUN — not yet in v0.1 product scope.

## v0.1 Product Boundaries

Squire owns:
- work classification;
- capability abstraction and provider binding;
- offloading / capability selection;
- bounded capability composition;
- evidence shaping and recovery references;
- efficiency / task-outcome telemetry;
- cross-harness runtime state needed for those functions.

Squire does not own:
- primary LLM reasoning loop;
- conversation UX;
- planning loop;
- sub-agent orchestration;
- general-purpose sandbox product;
- generic MCP/Skill marketplace;
- replacement implementations for mature capabilities without evidence that wrapping is insufficient.

## Reference Projects

- `HKUDS/OpenSpace`: capability/skill/evidence/runtime reference and possible provider.
- `NVlabs/SoL-Pi`: harness-efficiency mechanisms, evidence-preserving reduction, and economic-gating reference.

These are references, not normative dependencies. L2 must decide REUSE / WRAP / EXTEND / NEW / NOT_NEEDED per capability.

## Required Commands

Planning-only baseline: implementation commands are `NOT_RUN — implementation stack not frozen`.
Commands become mandatory only after L2/Task authority establishes the implementation stack.

## Release Gates

For v0.1, product/architecture authority may require:
- Frozen Product/L1 + PRD;
- Frozen L2 Architecture Evidence;
- required Task Validation / Review;
- version-level successful-task benchmark against frozen baseline;
- Candidate Freeze / Hidden Validation / Fresh Closeout / Release Qualification according to the pinned standard.

PR PASS != Release PASS.
