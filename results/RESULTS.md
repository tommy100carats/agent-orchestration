# Results — Anonymized

What the pipeline actually delivers in production. Numbers are **illustrative of scale**, not tied to any identifiable person or company.

## Throughput
- **N items prepared per day**, each fully researched + tailored + audited.
- **Cap respected**: the system produces fewer rather than pad when quality drops.

## Quality gates
- **0 invented fields** policy: missing data is flagged `to verify`, never fabricated.
- **Live-link verification** on every item before it reaches the human.
- **Consistency audit** in a fresh context catches upstream drift.

## Learning curve
- Rejection reasons are captured into memory.
- Sourcing precision improves run-over-run without any model fine-tuning.
- Result: fewer false positives reaching the human gate over time.

## Operating cost
- Orchestration by a non-developer (architect role), execution delegated to coding agents in parallel.
- Heavy lifting (extraction, generation) runs in sub-agents; planning and final review on the strong model.

## What this proves for AI/Ops
- A reliable, auditable multi-agent system in daily production use.
- Design discipline (ownership, gates, memory loop) over prompt tricks.
- Measurable, compounding improvement.
