# AGENTS.md

This project follows the immutable `kaicreator-mm/ai-development-standard` revision recorded in `.dev-standard/VERSION`.

## Read order

1. Read `.dev-standard/VERSION`.
2. Read `.dev-standard/PROJECT_OVERRIDES.md`.
3. Read the pinned standard's `AGENTS.md`.
4. For lifecycle work, read `standards/DEVELOPMENT_WORKFLOW.md`.
5. For L1/L2 work, read the pinned L1/L2 prompts and architecture research standard as applicable.
6. Treat GitHub repository state, Issues, PRs, exact SHAs and durable evidence as execution truth; chat is not project state.

## Squire-specific invariants

- Squire is harness-agnostic. Harness adapters MUST remain thin and MUST NOT become separate product cores.
- Squire does not own the primary reasoning loop, planning, conversation, sub-agent orchestration, or primary model selection.
- Prefer composition over reinvention: mature external tools SHOULD be wrapped/reused before equivalent functionality is built.
- Any optimization MUST be judged at task outcome. Token compression, tool speed, or local accuracy alone do not prove product value.
- Offloading MUST preserve correctness, recoverability, and evidence boundaries.
- Product/architecture semantics must not be changed by implementation agents without the applicable amendment path.
