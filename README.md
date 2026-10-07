# Squire

**Squire** is a harness-agnostic work-offloading runtime for AI agents.

Its goal is simple:

> Let strong agents spend their intelligence only on work that actually requires it.

Squire sits outside the agent harness. It classifies engineering work, selects lower-cost deterministic or specialized capabilities when appropriate, preserves recoverable evidence, and measures whether offloading actually improves successful-task economics.

## Product thesis

```text
Existing Agent / Harness
        |
        v
     Squire
        |
        +-- deterministic software
        +-- specialized tools
        +-- retrieval / indexes
        +-- lightweight decision models
        +-- external capability runtimes
```

Squire does **not** own the primary reasoning loop, planning loop, conversation, or sub-agent orchestration.

The initial product evidence and v0.1 planning live under `docs/implementation/0.1.0/`.

## Reference projects

- HKUDS/OpenSpace — capability / skill / evidence / runtime reference.
- NVlabs/SoL-Pi — efficient-harness mechanisms and economics reference.

Squire's differentiator is the layer neither project primarily owns: **cross-harness work classification, offloading, capability composition, and task-outcome ROI measurement**.

## Development standard

This repository follows the immutable `kaicreator-mm/ai-development-standard` revision pinned in `.dev-standard/VERSION`.
